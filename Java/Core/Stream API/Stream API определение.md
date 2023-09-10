**Stream API** – по сути это **поток данных** и **последовательные** **операции** над ними.

Интерфейсы [**Stream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/Stream.html), [**IntStream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/IntStream.html), [**LongStream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/LongStream.html) и [**DoubleStream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/DoubleStream.html) – это потоки **объектов** и **примитивных** **типов** `int`, `long` и `double`.

## Интерфейс `Stream<T>

```java
public interface Stream<T> extends BaseStream<T,Stream<T>>
```

Интерфейс [**Stream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/Stream.html) представляет собой последовательность элементов, поддерживающих **последовательные** и **параллельные** агрегатные операции.

> К агрегатным операциям относят различные **операции над выборкой**, например, получение числа элементов, получение минимального, максимального и среднего значения в выборке, а также суммирование значений.

Stream **не предоставляет** средств для **прямого доступа** к элементам или манипулирования ими. Вместо этого он **описывает** **источник** данных и **вычислительные операции**, которые будут выполняться над этими данными.
## Стрим примитивных типов

Чтобы обеспечить способ работы с тремя наиболее часто используемыми типами примитивов – `int`_,_ `long` и `double` – библиотека [**java.util.stream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/package-summary.html) включает три реализации стрима примитивов:

[**IntStream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/IntStream.html)

```
public interface IntStream extends BaseStream<Integer,IntStream>
```

[**LongStream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/LongStream.html)

```
public interface LongStream extends BaseStream<Long,LongStream>
```

[**DoubleStream**](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/stream/DoubleStream.html)

```
public interface DoubleStream extends BaseStream<Double,DoubleStream>
```