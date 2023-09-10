Метод `collect()` интерфейса Stream **собирает данные** в необходимую **структуру данных**, например в коллекции — `List<T>`, `Set<T>`, `Map<T, R>` 

```java
public interface Collector<T,A,R>
```

В метод `collect()` в качестве **параметра** принимает объект типа [**Collector**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/Collector.html).  
Статические методы класса [**Collectors**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/Collectors.html) возвращают такой объект класса **Collector**.

```java
public final class Collectors extends Object
```

Класс [**Collectors**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/Collectors.html) содержит статические методы для **сбора элементов** в коллекцию, **обобщения** и **группировки элементов** в соответствии с различными критериями и т. п., которые возвращают готовые объекты коллекций.

**Преобразование потока в** **List** `toList()`

```java
list.stream().collect(Collectors.toList());
```

**Преобразование потока в** **Set** `toSet()`

```java
list.stream().collect(Collectors.toSet());
```

**Преобразование потока в** **Map** `toMap()`

```java
map.entrySet().stream()
        .map(e -> String.valueOf(e).split("="))
        .collect(Collectors.toMap(e -> e[0], e -> e[1]));
```

**Объединение элементов в строку String** `joining()`  
Объединение потока коллекции **List** в одну строку через запятую

```java
list.stream().collect(Collectors.joining(", "));
```

**Сумма элементов потока** `summingInt(), summingDouble(), summingLong()   `Например, сумма заработной платы сотрудников

```java
List<Employee> employees = new ArrayList<>()

employees.stream()
        .collect(Collectors.summingDouble(Employee::getSalary)));
```

**Сгруппировать элементы по условию** `groupingBy()`  
Группировка людей по стране

```java
List<Person> people = new ArrayList<>()

people.stream()
        .collect(Collectors.groupingBy(Person::getCountry));
```