#### Этап обновления контекста


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

1. `BeanFactory` создает `BeanFactoryPostProcessor`-ы используя конструктор без аргументов
	- `BeanFactory` может создать экземпляр `BeanFactoryPostProcessor` только с конструктором без аргументов. В противном случае вы получите сообщение об ошибке со следующим сообщением: `No default constructor found.` 
	- Обратные вызовы инициализации и уничтожения не работают как у обычных bean-компоненты если вы используете конфигурацию на основе аннотаций. Но они работают если использовать конфигурации на основе XML. Подробности в [Жизненный цикл bean-компонента](obsidian://open?vault=ObsidianNotes&file=Spring%2FSpring%20Core%2F%D0%96%D0%B8%D0%B7%D0%BD%D0%B5%D0%BD%D0%BD%D1%8B%D0%B9%20%D1%86%D0%B8%D0%BA%D0%BB%20Bean%60a).
	- Если вы пометили `BeanFactoryPostProcessor` как лениво инициализируемый, то `BeanFactory` проигнорирует это
2. `ApplicationContext` вызывает метод `BeanFactoryPostProcessor#postProcessBeanFactory`
3. `BeanFactory` creates `BeanPostProcessor`-ы
4. `ApplicationContext` регистрирует `BeanPostProcessor`-ы
5. Инициализация **singleton** bean-компонентов. Подробности в  [Жизненный цикл bean-компонента](obsidian://open?vault=ObsidianNotes&file=Spring%2FSpring%20Core%2F%D0%96%D0%B8%D0%B7%D0%BD%D0%B5%D0%BD%D0%BD%D1%8B%D0%B9%20%D1%86%D0%B8%D0%BA%D0%BB%20Bean%60a).
6. `ApplicationContext` проверяет флаг `SmartLifecycle#isRunning` и вызывает метод `SmartLifecycle#start`, если флаг имеет значение `false`
7. `ApplicationContext` публикует `ContextRefreshedEvent`
8. Методы обратного вызова, помеченные аннотацией `@EventListener` с типом параметра метода `ContextRefreshedEvent`, обрабатывают это событие. Также здесь может быть `ApplicationListener`

