UnaryOperator — функциональный интерфейс, принимает в качестве параметра объект типа T, выполняет над ним некоторые операции и возвращает результат операций в виде объекта того же типа T:

```java
@FunctionalInterface
public interface UnaryOperator<T> {
   T apply(T t);
}
```

UnaryOperator, который своим методом apply возводит число в квадрат:

```java
public static void main(String[] args) {
   UnaryOperator<Integer> squareValue = x -> x * x;
   System.out.println(squareValue.apply(9));
}
```

Вывод в консоль:

```
81
```

Мы рассмотрели пять функциональных интерфейсов. Это не все, что доступно нам начиная с Java 8 — это основные интерфейсы. Остальные из доступных — это их усложненные аналоги. Полный список можно посмотреть в официальной документации Oracle.