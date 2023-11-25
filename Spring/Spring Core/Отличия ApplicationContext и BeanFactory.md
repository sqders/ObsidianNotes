Помимо nредоставления модели, которая в большей степени сосредоточена на де­кларативном конфигурировании, в интерфейсе ApplicationContext поддержива­ется следующий ряд дополнительных возможностей. 
•  Интернационализация. 
•  Публикация событий. 
•  Доступ к ресурсам и управление ими. 
•  Дополнительные интерфейсы жизненного цикла. 
•  Улучшенное автоматическое конфигурирование компонентов инфраструктуры. 
В последующих разделах будут описаны самые важные функциональные возмож-
ности интерфейса ApplicationContext, помимо внедрения зависимостей. 

# Интернационализация средствами интерфейса MessageSource

Одной из областей, в которых Spring действительно превосходит другие карка­сы, является поддержка интернационализации (сокращенно i18n). 

Через интерфейс MessageSource приложение может получать доступ к строковым ресурсам тиnа String, называемым сообщениями и хранящимся на разных языках.

Помимо интерфейса ApplicationContext, в Spring  предоставляются следую-
щие реализации интерфейса MessageSource: 
•  ResourceBundleMessageSource  - загружает сообщения при помощи класса ResourceBundle
•  ReloadaЬleResourceBundleMessageSource - такой же но поддерживает перезагрузку
•  StaticMessageSource - не рекомендуется потому что не поддерживается внешняя конфигурация

Чтобы воспользоваться этим ресурсом в Spring нам требуется создать бин класса MessageSource под названием messageSource.
```java
@Bean
public MessageSource messageSource() {
	ReloadableResourceBundleMessageSource messageSource = new   
			ReloadableResourceBundleMessageSource();
	messageSource.setBasename("classpath:messages");
	messageSource.setCacheSeconds(3600);
	messageSource.setDefaultEncoding("UTF-8");
	return messageSource;
}
@Bean
public LocaleResolver localeResolver() {
    return new UserLocaleResolver();
}
```


```java
public class UserLocaleResolver implements LocaleResolver {

    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        final String lang = request.getHeader("Language");
        return Locale.forLanguageTag(lang);
    }

    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
        // Not needed for this example
    }

}
```
# События в приложениях

Еще одной особенностью интерфейса `ApplicationContext`, отсутствующей в интерфейсе `BeanFactory`, является возможность публиковать и получать события, используя интерфейс `ApplicationContext` в качестве посредника.

 События публикуются с помощью метода `ApplicationEventPublisher.publishEvent ()`

![[Pasted image 20231124152351.png]]

Слушателя в Spring реализуется через имплементацию интерфейса `ApplicationListener<>`

```java
package  com.apress.prospring5.ch4; 
import  org.springframework.context.ApplicationListener; 
public class  MessageEventListener 
	implements  ApplicationListener<MessageEvent>{ 
	@Override 
	public void  onApplicationEvent(MessageEvent  event)  { 
		MessageEvent  msgEvt  =  (MessageEvent)  event; 
		System.out.println("Received:  "+ msgEvt.getMessage()); 
	}
}
```
Далее компонент который вызывает эти эвенты при помощи своего метода publish()
![[Pasted image 20231124153049.png]]


# Доступ к ресурсам 

Приложение может одинаково получить доступ к файловому ресурсу, где бы он ни находился: в файловой системе, по пути к классам или на удаленном сервере.

В  основу  всей  поддержки  ресурсов  в  Spring  положен  интерфейс  org.springframework.core.io.Resource. В интерфейсе Resource определены де­сять самоочевидных методов: `contentLength()`, `exists()`, `getDescription()`,` getFile()`, `getFileName()`, `getURI()`, `getURL()`, `isOpen()`, `isReadable()` и `lastModified()`

Зачастую применяется одна из встроенных в Spring реализаций для доступа к файлам (класс `FileSystemResource`), по пути к классам (класс `ClassPathResource`) или URL-pecypcaм (класс `UrlResource`).

Для обнару­жения и получения экземпляров типа Resource в Spring применяется другой интерфейс, `ResourceLoader`, и его стандартная реализация `DefaultResourceLoader` или через реализацию в `ApplicationContext`.

![[Pasted image 20231124154204.png]]
