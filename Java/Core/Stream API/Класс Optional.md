
Методы интерфейса **Stream** `findAny()`, `findFirst()`, `max()`, `min()` и `reduce()` возвращают объект класса [**Optional**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/Optional.html)

```java
public final class Optional<T> extends Object
```

Это объект-**контейнер**, который может содержать или не содержать **нулевое значение**.  
Ссылка на объект класса **Optional** может быть `null   `Если значение присутствует, метод `isPresent()` вернет `true`

Позволяет избавиться от проверки на `null   `Без этого класса приходилось писать проверку на **NullPointerException**.  
Благодарю этому один объект **Optional** можно **сравнить** с другим объектом **Optional** через метод `equals()`, даже если они хранят в себе ссылки на `null`.

**Получить элемент Optional** `get()   `Метод класса **Optoinal** – возвращает значение, если оно присутствует, в противном случае бросит **NoSuchElementException** .

```java
Stream.of("1", "22", "333")
        .max(Comparator.comparingInt(String::length))
        .get();
```