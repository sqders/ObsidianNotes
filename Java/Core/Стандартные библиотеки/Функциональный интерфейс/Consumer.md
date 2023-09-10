Consumer (с англ. — “потребитель”) — функциональный интерфейс, который принимает в качестве входного аргумента объект типа T, совершает некоторые действия, но при этом ничего не возвращает:

```java
@FunctionalInterface
public interface Consumer<T> {
   void accept(T t);
}
```

В качестве примера рассмотрим Consumer, задача которого — выводить в консоль приветствие с переданным строковым аргументом: 

```java
public static void main(String[] args) {
   Consumer<String> greetings = x -> System.out.println("Hello " + x + " !!!");
   greetings.accept("Elena");
}
```

Вывод в консоль:

```
Hello Elena !!!
```