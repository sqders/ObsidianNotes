В данном подразделе пойдет речь о: IoC Container, DI, `BeanDefinition`, `BeanFactory`, `ApplicationContext`, `BeanFactoryPostProcessor`, `BeanPostProcessor`, `ApplicationListener`, `Lifecycle`, `SmartLifcycle`.

[Инверсия управления (Inversion of Control)](obsidian://open?vault=ObsidianNotes&file=Spring%2FSpring%20Core%2FIOC%20(inversion%20of%20control)) - это принцип, при котором фреймворк вызывает пользовательский код. Это отличается от случая с библиотеками, потому что пользовательский код вызывает код библиотеки.

[Внедрение зависимостей (Dependency Injection)](https://en.wikipedia.org/wiki/Dependency_injection) - это шаблон проектирования, в котором объект получает объекты, от которых он зависит. Это отделяет создание объектов от их использования.

[IoC Контейнер](obsidian://open?vault=ObsidianNotes&file=Spring%2F%D0%90%D0%BD%D0%BD%D0%BE%D1%82%D0%B0%D1%86%D0%B8%D0%B8%2FCore%2FIoC%20Container) (IoC Container) - это реализация IoC и DI. Контейнер IoC создает и управляет bean-компонентами на основе мета-информации. Он также может решать и предоставлять зависимости для создаваемых им объектов.

[BeanDefinition](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanDefinition.html) - описывает bean-компоненты. Создается на основе разобранной мета-информации.

[BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html) - это интерфейс который создает и предоставляет bean-компоненты на основе BeanDefinition-ов. Он является ядром `ApplicationContext`.

[ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html) - это центральный интерфейс который предоставляет следующий список возможностей:

- возможности BeanFactory
    
- загрузка ресурсов
    
- публикация событий
    
- интернационализация
    
- автоматическая регистрация `BeanPostProcessor` и `BeanFactoryPostProcessor`
    

[BeanFactoryPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html) - это интерфейс, который позволяет настраивать определения bean-компонентов контекста приложения. Он создается и запускается перед BeanPostProcessor.

[BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html) - это интерфейс для обеспечения интеграции кастомной логики создания экземпляров, разрешения зависимостей и т. д. Каждый компонент, созданный `BeanFactory`, проходит через каждый зарегистрированный `BeanPostProcessor`.

[ApplicationContextEvent](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/event/ApplicationContextEvent.html) - основной класс для событий, возникающих в процессе жизненного цикла ApplicationContext. Его подклассы:

- **ContextRefreshedEvent** - публикуется автоматически после поднятия контекста
    
- ContextStartedEvent - публикуется методом `ApplicationContext#start`
    
- ContextStoppedEvent - публикуется методом `ApplicationContext#stop`
    
- ContextClosedEvent - публикуется автоматически перед закрытием контекста
    

[ApplicationListener](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationListener.html) - интерфейс который позволяет обрабатывать [ApplicationEvent](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationEvent.html) события. Можно использовать аннотацию [@EventListener](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/event/EventListener.html) вместо интерфейса.

[Lifecycle](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/Lifecycle.html) - интерфейс похожий на `ApplicationListener`, но в нем определено 2 метода, которые срабатывают во время запуска (start) и остановки (stop) контекста.

[SmartLifecycle](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/SmartLifecycle.html) - это расширение Lifecycle интерфейса. Отличие в том, что он срабатывает во время обновления (refresh) и закрытия (close) контекста.

## Создание Bean`a

1. `BeanFactory` создает bean-компонент
2. Срабатывает статический и не статический блок инициализации
3. Внедрение зависимостей на основе конструктора и`setter`-ов
6. Отрабатывают методы стандартного набора `*Aware` интерфейсов
7.  Компонент начинают обрабатывать встроенные Spring BeanPostProcessor`BeanPostProcessor#postProcessBeforeInitialization` обрабатывает bean-компонент
8. Вызываются методы обратного вызова, помеченные аннотацией `@PostConstruct`
9. `BeanFactory` вызывает метод `InitializingBean#afterPropertiesSet`то есть bean инициализаруются
10. Вызывается метод `initMethod`
11. После чего компонент попадает в контекст

## Этап уничтожения bean-компонента

Этап уничтожения срабатывает только для **singleton** bean-компонентов, так как только эти компоненты храниться в `BeanFactory`.

1. `InitDestroyAnnotationBeanPostProcessor.postProcessBeforeDestruction` вызывает методы обратного вызова, отмеченные как`@PreDestroy`
2. `BeanFactory` вызывает метод `InitializingBean#destroy`
3. `BeanFactory` вызывает метод обратного вызова, зарегистрированный как `destroyMethod`
Стоит знать

По умолчанию bean-компоненты, определенные с конфигурацией Java, которые имеют public метод `close()` или `shutdown()`, автоматически становятся методами обратного вызова уничтожения.

### Дополнения к жизненному циклу

В данном подразделе я кратко расскажу об инструментах, о которых я не рассказывал ранее, так как они позволяют добавить более специфическую логику в жизненный цикл Spring-a. А моя цель была подробно разобрать типичный жизненный цикл.

[Ordered](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/Ordered.html) - интерфейс, позволяющий управлять порядком работы компонентов. Например, если компоненты реализуют `BeanPostProcessor`/`BeanFactoryPostProcessor` и `Ordered` интерфейсы, то мы можем контролировать порядок их выполнения.

[FactoryBean](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/FactoryBean.html) - интерфейс, позволяющий внедрить сложную логику создания объекта. Если у вас есть сложный код инициализации, который лучше выражается на Java, вы можете создать свой собственный `FactoryBean`, написать сложную инициализацию внутри этого класса, а затем подключить свой собственный `FactoryBean` к контейнеру.