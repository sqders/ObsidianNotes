Аннотация используется для внедрения зависимостей в Spring, т.е. для автовставки компонента в поле бина(компонента или контроллера) с помощью IoC.
# Как работает
Подбирает подходящие бины по их типу данных
1. Spring сканирует все классы с аннотацией [[@Component]] и создает бины для этих классов
2. Spring сканирует все созданные бины и проверяет, подходит ли хотя бы один бин в качестве зависимости там, где мы указали `**@Autowired**`
3. Если находится 1 подходящий bean, то он внедряется, если не находится ни одного - ошибка, а если больше 1 - неоднозначность, которую необходимо разрешить программисту вручную.
# Использование
**На полях, конструкторах и сеттерах.**

**Даже если поле приватное и нет ни конструктора, ни сеттера, `@Autowired` внедрит зависимость, с помощью рефлексии.**


### Тем не менее внедрению зависимостей через поле присущи перечисленные ниже недостатки, поэтому его обычно принято избегать. 
•  Несмотря на всю простоту такого способа внедрения зависимостей, необходи­мо соблюдать особую осторожность, чтобы не нарушить **принцип единствен­ной ответственности**. Чем больше зависимостей, тем больше обязанностей у класса, а это может затруднить разделение обязанностей во время реорганиза­ции кода. Ситуацию, когда класс становится слишком громоздким, легче выя­вить, когда зависимости внедряются через конструкторы или методы установ­ки, но довольно трудно обнаружить, когда они внедряются через поля.
• В каркасе Spring обязанности по внедрению зависимостей передаются контей­неру инверсии управления, но класс должен явно сообщить типы требующихся
зависимостей через открытый интерфейс, методы или конструкторы. Если же зависимости внедряются через поля, то может быть неясно, какого зависи­мость именно типа требуется и является ли она обязательной или нет. 
•  Внедрение зависимостей через поле вносит зависимость от контейнера инвер­сии управления, поскольку аннотация **@Autowired является компонентом Spring**.  Следовательно, **компонент Spring  Bean  больше не является простым объектом POJO**,  и его экземпляр нельзя получить независимым путем. 
•  Внедрение зависимостей через поле неприменимо к оконечным полям. Такие поля могут быть инициализированы только с помощью внедрения зависимос­тей через конструктор. 
•  Внедрение зависимостей через поле **затрудняет написание тестов**, поскольку зависимости приходится внедрять вручную. 


[[Component]]
[[Bean]]
[[Как Spring работает с классами]]
[[IoC Container]]