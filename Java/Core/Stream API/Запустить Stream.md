Поток элементов может быть получен разными способами, например:

Stream из **List**

``` java
List<String> list = new ArrayList<>();

list.stream();

list.parallelStream();           // параллельный поток
```

Stream из **Map**

``` java
Map<String, String> map = new HashMap<>();

map.entrySet().stream();

map.values().stream();
```

Stream из **массива**, используя **статический** метод класса **Arrays**

```java
String[] array = new String[10];

Arrays.stream(array);
```

Stream из **элементов**, используя **статические** методы **Классов-потоков**

```java
Stream.of("a", "b", "c");        // поток из элементов

Stream.of(array);                // поток из элементов массива

Stream.of(list);                 // поток из элементов списка List

Stream.generate(Math::random);   // генерация потока рандомных чисел

Stream.concat(stream1, stream2); // объединяет два потока в один

IntStream.range(1, 10);          // поток диапазона чисел от 1 до 9

IntStream.rangeClosed(1, 10);    // поток диапазона чисел от 1 до 10
```

Stream из **строк** **буфера BufferedReader**

```java
BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

bufferedReader.lines();
```

Stream из **строк файла** через **статический** **метод** класса **Files**

```java
Path path = Path.of("/root/test.txt");

Files.lines(path);
```

Stream из **случайных** **чисел Random**

```java
Random random = new Random();

random.ints();

random.longs();

random.doubles();
```