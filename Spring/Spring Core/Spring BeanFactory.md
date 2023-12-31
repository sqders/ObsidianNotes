Если в приложении требуется лишь поддержка внедрения зависимостей, то с кон­тейнером внедрения зависимостей в Spring  можно взаимодействовать через интерфейс **BeanFactory**.
 Внутренне конфигурация компонентов Spring Beans пред­ставлена экземплярами классов, реализующих интерфейс **BeanDefinition**. В кон­фигурации компонента Spring Bean хранятся сведения не только о самом компоненте, но и о тех компонентах Spring Beans, от которых он зависит.
Для любых классов реа­лизации интерфейса **BeanFactory**, в которых также реализуется интерфейс **BeanDefinitionReader**, данные типа **BeanDefinition** можно прочитать из файла кон­фигурации, используя классы **PropertiesBeanDefinitionReader** или **XmlBeanDefinitionReader**.
Итак, компоненты Spring  Beans  можно идентифицировать в  интерфейсе Bean 
Factory, *и каждому компоненту Spring Bean  может быть назначен идентификатор, имя или то и другое*.
Экземпляр компонента Spring Bean можно получить и без иден­тификатора или имени (это так называемый анонимный компонент Spring Bean) или 
как один компонент внутри другого компонента Spring Bean.