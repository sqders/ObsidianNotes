`Function` — этот функциональный интерфейс принимает аргумент T и приводит его к объекту типа R, который и возвращается как результат:

```java
@FunctionalInterface
public interface Function<T, R> {
   R apply(T t);
}
```

В качестве примера возьмём `Function`, который конвертирует числа из формата строк (String) в формат чисел (Integer): 

```java
public static void main(String[] args) {
   Function<String, Integer> valueConverter = x -> Integer.valueOf(x);
   System.out.println(valueConverter.apply("678"));
}
```

Запустив, получим вывод в консоль:
```
678
```

P.S.: если в строку мы передадим не только числа, но и другие символы, вылетит exception — `NumberFormatException.`