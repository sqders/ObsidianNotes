Supplier (с англ. — поставщик) — функциональный интерфейс, который не принимает никаких аргументов, но возвращает некоторый объект типа T:

```java
@FunctionalInterface
public interface Supplier<T> {
   T get();
}
```

В качестве примера рассмотрим Supplier, который будет выдавать рандомные имена из списка:

```java
public static void main(String[] args) {
   ArrayList<String> nameList = new ArrayList<>();
   nameList .add("Elena");
   nameList .add("John");
   nameList .add("Alex");
   nameList .add("Jim");
   nameList .add("Sara");

   Supplier<String> randomName = () -> {
       int value = (int)(Math.random() * nameList.size());
       return nameList.get(value);
   };

   System.out.println(randomName.get());
}
```

И если мы это запустим, то увидим в консоли случайные результаты из списка имен.