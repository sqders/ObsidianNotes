Посмотрим что лежит в переменной `filterChain`
![[Pasted image 20230829114335.png]]
Здесь интересен фильтр `springSecurityFilterChain` именно он делает всю работу SS в веб части. Сам `DelegatingFilterProxyRegistrationBean` не очень интересен, посмотрим кому он делегирует свою работу
![[Pasted image 20230829114357.png]]
Свою работу он делегирует классу `FilterChainProxy`. Внутри него происходит несколько интересных вещей.  
  
Прежде всего посмотрим на метод `FilterChainProxy#doFilterInternal`. **Что здесь происходит?** Получаем фильтры, создаем `VirtualFilterChain` и запускаем по ним запрос и ответ.
```java
List<Filter> filters = getFilters(fwRequest); 
... 
VirtualFilterChain vfc = new VirtualFilterChain(fwRequest, chain, filters); vfc.doFilter(fwRequest, fwResponse);
```
Внутри метода getFilters берем первый `SecurityFilterChain` который совпадет с запросом.
```java
private List<Filter> getFilters(HttpServletRequest request) { 
	for (SecurityFilterChain chain : filterChains) { 
		if (chain.matches(request)) 
			{ 
				return chain.getFilters(); 
			} 
		} 
	return null;
}
```
Зайдем в деббагер и посмотрим какой список итерируется.
![[Pasted image 20230829114634.png]]
Что нам говорит этот список?  
  
В обоих листах лежит `OrRequestMatcher`, который попытается сматчить текущий url с хотя бы с одним паттерном из списка.  
Первый элемеyт списка имеет пустой список фильтров, соответственно никакой дополнительной фильтрации не будет, и как следствие не будет и защиты.  

> Любой url который совпадет с этим паттернами, по умолчанию не будет защищен SS.  
> `"/css/**", "/js/**", "/images/**", "/webjars/**", "/**/favicon.ico", "/error"`

Гораздо интереснее второй SecurityFilterChain который совпадет с любым url `/**`  
  
В нашем случае имеется следующий список фильтров.
```
0 = {WebAsyncManagerIntegrationFilter} 
1 = {SecurityContextPersistenceFilter} 
2 = {HeaderWriterFilter} 
3 = {CsrfFilter} 
4 = {LogoutFilter} 
5 = {BasicAuthenticationFilter} 
6 = {RequestCacheAwareFilter} 
7 = {SecurityContextHolderAwareRequestFilter} 
8 = {AnonymousAuthenticationFilter} 
9 = {SessionManagementFilter} 
10 = {ExceptionTranslationFilter} 
11 = {FilterSecurityInterceptor}
```
Этот список может изменяться в зависимости от настроек и добавленных зависимостей.  
Например с такой конфигурацией:
```java
http 
.authorizeRequests()
.anyRequest()
.authenticated() 
.and() 
.formLogin() 
.and() 
.httpBasic();
```

В этот список добавились бы фильтры:
```
UsernamePasswordAuthenticationFilter
DefaultLoginPageGeneratingFilter
```
### 0 = {WebAsyncManagerIntegrationFilter}

  
Нам не очень интересен, согласно документации он «интегрирует» SecurityContext с WebAsyncManager который отвественнен за асинхронные запросы.  
  

### 1 = {SecurityContextPersistenceFilter}

Ищет SecurityContext в сессии и заполняет SecurityContextHolder если находит.  
По умолчанию используется ThreadLocalSecurityContextHolderStrategy которая хранит SecurityContext в ThreadLocal переменной.  
### 2 = {HeaderWriterFilter}
Просто добавляет заголовки в response.  
  
Отключаем кэш:  
– Cache-Control: no-cache, no-store, max-age=0, must-revalidate  
– Pragma: no-cache  
– Expires: 0  
  
Не разрешаем браузерам автоматически определять тип контента:  
– X-Content-Type-Options: nosnif  
Не разрешаем iframe   
– X-Frame-Options: DENY   
Включаем встроенную зашиту в браузер от cross-site scripting (XSS)   
– X-XSS-Protection: 1; mode=block  
### 3 = {CsrfFilter}

Пожалуй нет ни одного разработчика который при знакомстве с SS не столкнулся бы с ошибкой «отсутсвия [csrf](obsidian://open?vault=Java&file=%D0%A1%D0%B5%D1%82%D0%B8%2FCRFS) токена».  
### 4 = {LogoutFilter}

  
Далее идет logout фильтр, он проверяет совпадает ли url c паттерном  
`Ant [pattern='/logout', POST] - по умолчанию`  
и запускает процедуру логаута  

```
handler = {CompositeLogoutHandler} 
 logoutHandlers = {ArrayList}  size = 2
  0 = {CsrfLogoutHandler} 
  1 = {SecurityContextLogoutHandler}
```
по дефолту происходит следующие:  
1. Удаляется Csrf токен.
2. Завершается сессия
3. Чистится SecurityContextHolder
### 5 = {BasicAuthenticationFilter}  
Теперь мы добрались непосредственно до аутентификации. Что происходит внутри?  
Фильтр проверяет, есть ли заголовок Authorization со значением начинающийся на Basic  
Если находит, извлекает логин\пароль и передает их в `AuthenticationManager`  
#### AuthenticationManager

```java
public interface AuthenticationManager {
  Authentication authenticate(Authentication authentication) 
      throws AuthenticationException;
}
```

  
`AuthenticationManager` представляет из себя интрефейс, который принимает `Authentication` и возвращает тоже `Authentication`. В нашем случае в имплементацией `Authentication` будет `usernamePasswordAuthenticationToken`. Можно было бы реализовать `AuthenticationManager` самому, но смысла в этом мало, существует дефолтная реализация — `ProviderManager`.  
  
`ProviderManager` авторизацию делегирует другому интерфейсу:  

```java
public interface AuthenticationProvider {
  Authentication authenticate(Authentication authentication) 
      throws AuthenticationException;
  boolean supports(Class<?> authentication);
}
```

Когда мы передаем объект `Authentication` в `ProviderManager`, он перебирает существующие `AuthenticationProvider`-ры и проверяет суппортит ли  
`AuthenticationProvider` эту имплементацию `Authentication`  
```java
public boolean supports(Class<?> authentication) {
  return (UsernamePasswordAuthenticationToken.class
          .isAssignableFrom(authentication));
}
```

В результате внутри `AuthenticationProvider.authenticate` мы уже можем скастить переданный Authentication в нужную реализацию без каст эксепшена.  
  
Далее из конкретной реализации вытаскиваем креденшеналы.  

Если аутентификация не удалась `AuthenticationProvider` должен бросить эксепшен, `ProviderManager` поймает его и попробует следующий AuthenticationProvider из списка, если ни один `AuthenticationProvider` не вернет успешную аутентификацию, то `ProviderManager` пробросит последний пойманный эксепшен. 
  
Далее `BasicAuthenticationFilter` сохраняет полученный `Authentication` в `SecurityContextHolder`  
При помощи метода:
`SecurityContextHolder.getContext().setAuthentication(authResult); ` 
Процесс аутентификации на этом завершен.  
  
Если выбросится AuthenticationException то будет сброшен `SecurityContextHolder.clearContext();` контекст и вызовится `AuthenticationEntryPoint`.  
```java
public interface AuthenticationEntryPoint {
  void commence(HttpServletRequest request, 
               HttpServletResponse response,
               AuthenticationException authException)  throws IOException, ServletException;
}
```

Задачей AuthenticationEntryPoint является записать в ответ информацию о том что аутентификация не удалась.  
  
В случае бейсик аутентификации это будет:  
```java
response.addHeader("WWW-Authenticate", "Basic realm=\"" + realmName + "\"");
response.sendError(HttpServletResponse.SC_UNAUTHORIZED, authException.getMessage());
```
В результате браузер покажет окошко basic авторизации.

### 6 = {RequestCacheAwareFilter}

Для чего нужен этот фильтр? Представим сценарий:  
  
1. Пользователь заходит на защишенный url.  
2. Его перекидывает на страницу логина.  
3. После успешной авторизации пользователя перекидывает на страницу которую он запрашивал в начале.  
  
Именно для восстановления оригинального запроса существует этот фильтр.  
Внутри проверяется есть ли сохраненный запрос, если есть им подменяется текущий запрос.  
Запрос сохраняется в сессии.

### 7 = {SecurityContextHolderAwareRequestFilter}

Оборачивает существущий запрос в SecurityContextHolderAwareRequestWrapper
```java
chain.doFilter(this.requestFactory.create((HttpServletRequest) req,
			    (HttpServletResponse) res),
			    res);
```

  
Имплементация может отличаться в зависимости от servlet api версии servlet 2.5/3  
  

### 8 = {AnonymousAuthenticationFilter}

Если к моменту выполнения этого фильтра SecurityContextHolder пуст, т.е. не произошло аутентификации фильтр заполняет объект SecurityContextHolder анонимной аутентификацией — AnonymousAuthenticationToken с ролью «ROLE_ANONYMOUS».  
  
Это гарарантирует что в SecurityContextHolder будет объект, это позволяет не бояться NP, а также более гибко подходить к настройке доступа для неавторизованных пользователей.  
### 9 = {SessionManagementFilter}

На это этапе производятся действия связанные с сессией.  
  
Это может быть:  
  
— смена идентификатора сессии  
— ограничение количества одновременных сессий  
— сохранение SecurityContext в securityContextRepository  
  
В нашем случае происходит следующе:  
`SecurityContextRepository` с дефолтной реализацией HttpSessionSecurityContextRepository сохраняет SecurityContext в сессию.  
Вызывается `sessionAuthenticationStrategy.onAuthentication`  
  
Внутри sessionAuthenticationStrategy лежит:

```
sessionAuthenticationStrategy = {CompositeSessionAuthenticationStrategy}
 delegateStrategies
  0 = {ChangeSessionIdAuthenticationStrategy} 
  1 = {CsrfAuthenticationStrategy}
```
Происходят 2 вещи:  

1. По умолчанию включена защита от session fixation attack, т.е. после аутентификации меняется id сессии.  
2. Если был передан csrf токен, генерируется новый csrf токен  

### 10 = {ExceptionTranslationFilter}

  
К этому моменту SecurityContext должен содеражть анонимную, либо нормальную аутентификацию.  
  
ExceptionTranslationFilter прокидывает запрос и ответ по filter chain и обрабатывает возможные ошибки авторизации.  
  
SS различает 2 случая:  
1. AuthenticationException  
Вызывается `sendStartAuthentication`, внутри которого происходит следующиее:  
  
`SecurityContextHolder.getContext().setAuthentication(null);` — отчищает SecurityContextHolder  
`requestCache.saveRequest(request, response);` — сохраняет в requestCache текущий запрос, чтобы RequestCacheAwareFilter было что восстанавливать.  
`authenticationEntryPoint.commence(request, response, reason);` — вызывает authenticationEntryPoint — который записывает в ответ сигнал о том что необходимо произвести аутентификацию (заголовки \ редирект)  
  
2. AccessDeniedException  
  
Тут опять возможны 2 случая: 
1. Пользователь с анонимной аутентификацией, или с аутентификацией по rememberMe токену вызывается sendStartAuthentication  
  
2. Пользователь с полной, не анонимной аутентификацией вызывается:  
accessDeniedHandler.handle(request, response, (AccessDeniedException) exception)  
который по дефолту проставляет ответ forbidden 403  
### 11 = {FilterSecurityInterceptor}
  
**На последнем этапе происходит авторизация на основе url запроса**
FilterSecurityInterceptor наследуется от AbstractSecurityInterceptor и решает, имеет ли текущий пользователь доступ до текущего url.  
  
Существует другая реализация MethodSecurityInterceptor который отвественнен за допуск до вызова метода, при использовании аннотаций`@Secured` `@PreAuthorize`.  
  
Внутри вызывается AccessDecisionManager  
  
Есть несколько стратегий принятия решения о том давать ли допуск или нет, по умолчанию используется: AffirmativeBased  
```java

for (AccessDecisionVoter voter : getDecisionVoters()) {
  int result = voter.vote(authentication, object, configAttributes);
  switch (result) {
  case AccessDecisionVoter.ACCESS_GRANTED:
    return;
  case AccessDecisionVoter.ACCESS_DENIED:
    deny++;
    break;
  default:
    break;
  }
}
if (deny > 0) {
  throw new AccessDeniedException();
}
checkAllowIfAllAbstainDecisions();
```
  
Иными словами если кто-то голосует за, пропускаем, если хоть 1 голосует против не пускаем, если никто не проголосовал не пускаем.  
  
# Подведем небольшой итог:  
  
`springSecurityFilterChain` — набор фильтров spring security.  
  
Пример набора фильтров для basic авторизации:  
  
`WebAsyncManagerIntegrationFilter` — Интегрирует SecurityContext с WebAsyncManager  
`SecurityContextPersistenceFilter` — Ищет SecurityContext в сессии и заполняет SecurityContextHolder если находит  
`HeaderWriterFilter` — Добавляет «security» заголовки в ответ  
`CsrfFilter` — Проверяет на наличие сsrf токена  
`LogoutFilter` — Выполняет logout  
`BasicAuthenticationFilter` — Производит basic аутентификацию  
`RequestCacheAwareFilter` — Восстанавливает сохраненный до аутентификации запрос, если такой есть  
`SecurityContextHolderAwareRequestFilter` — Оборачивает существущий запрос в SecurityContextHolderAwareRequestWrapper  
`AnonymousAuthenticationFilter` — Заполняет SecurityContext ананонимной аутентификацией  
`SessionManagementFilter` — Выполняет действия связанные с сессией  
`ExceptionTranslationFilter` — Обрабатывает AuthenticationException\AccessDeniedException которые происходят ниже по стеку.  
`FilterSecurityInterceptor` — Проверяет имеет ли текущей пользователь доступ к текущему url.  
  
`FilterComparator` — здесь можно посмотреть список фильтров и их возможный порядок.  
  
`AuthenticationManager` — интерфейс, ответственнен за аутентификацию  
`ProviderManager` — реализация AuthenticationManager, которая использует внутри использует `AuthenticationProvider`  
`AuthenticationProvider` — интерфейс, отвественнен за аутентификаци конкретной реализации `Authentication`.  
`SecurityContextHolder` — хранит в себе аутентификацию обычно в ThreadLocal переменной.  
`AuthenticationEntryPoint` — модифицирует ответ, чтобы дать понять клиенту что необходима аутентификация (заголовки, редирект на страницу логина, т.п.)  
  
`AccessDecisionManager` решает имеет ли `Authentication` доступ к какому-то ресурсу.  
`AffirmativeBased` — стратегия используемая AccessDecisionManager по умолчанию.  

https://habr.com/ru/articles/346628/