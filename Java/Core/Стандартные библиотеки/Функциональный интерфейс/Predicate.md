Predicate — функциональный интерфейс для проверки соблюдения некоторого условия. Если условие соблюдается, возвращает true, иначе — false:

```java
@FunctionalInterface
public interface Predicate<T> {
   boolean test(T t);
}
```

 В качестве примера рассмотрим создание Predicate, который будет проверять на чётность числа типа Integer:

```java
public static void main(String[] args) {
   Predicate<Integer> isEvenNumber = x -> x % 2==0;

   System.out.println(isEvenNumber.test(4));
   System.out.println(isEvenNumber.test(3));
}
```
Вывод в консоль:
```
true
false
```