Автору известно два способа реализации шаблона с нормальной инициализацией.  

###### 1 Static field

```java
public class Singleton {
	public static final Singleton INSTANCE = new Singleton();
}
```

**+** Простая и прозрачная реализация  
**+** Потокобезопасность  
**-** Не ленивая инициализация  
###### 2 Enum Singleton

По мнению Joshua Bloch’а это лучший способ реализации шаблона [1].  
  
```java
public enum Singleton {
	INSTANCE;
}
```

**+** Остроумно  
**+** Сериализация из коробки  
**+** Потокобезопасность из коробки  
**+** Возможность использования EnumSet, EnumMap и т.д.  
**+** Поддержка switch  
**-** Не ленивая инициализация