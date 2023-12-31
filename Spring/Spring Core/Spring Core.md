**Spring Core** — это та часть фреймворка Spring, которая обеспечивает фундаментальные механизмы его интеграции в приложение. 
Spring работает по принципу *инверсии управления* ([inversion of control](obsidian://open?vault=ObsidianNotes&file=Spring%2FSpring%20Core%2FIOC%20(inversion%20of%20control)), IoC): вместо того чтобы приложение само контролировало свое выполнение, управление передается некоторому другому программному обеспечению — в данном случае фреймворку Spring. Посредством системы настроек мы предоставляем фреймворку инструкции о том, как распоряжаться написанным нами кодом, что и определяет логику работы приложения. 
### ПРИМЕЧАНИЕ 
В этом контексте «управление» означает такие действия, как «создание экземпляра» или «вызов метода». Фреймворк может создавать объекты классов, определенных в приложении. На основании написанных вами конфигураций Spring способен перехватывать выполнение метода и дополнять его различными функциями, к примеру регистрацией любой ошибки, которая может возникнуть в процессе выполнения этого метода.

![[Pasted image 20230910163530.png]]

Аспектно-ориентированное программирование (aspect-oriented programming, AOP) в Spring  *позволяет управлять экземплярами, помещенными в контроллер IoC, и, в частности, **перехватывать методы**, описывающие поведение этих экземпляров*. Такая возможность называется **аспектированием метода**. Spring AOP — один из наиболее типичных способов взаимодействия фреймворка с приложением.
