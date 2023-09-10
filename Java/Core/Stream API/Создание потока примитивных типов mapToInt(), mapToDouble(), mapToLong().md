
``` java
List<Integer> numbers = new ArrayList<>();

numbers.stream().mapToInt(value -> value);

numbers.stream().mapToDouble(value -> value);
        
numbers.stream().mapToLong(value -> value);
```

Стримы примитивных типов данных имеют ряд уникальных методов, например **диапазон** и **сумма** всех элементов потока.

Методы диапазона доступны только для **IntStream** и **LongStream**.  
Существует два метода получения стрима из диапазона чисел:  
  
`range(int startInclusive, int endExclusive)   `  
`rangeClosed(int startInclusive, int endInclusive)`

В методе `range()`второй аргумент метода **не входит** в диапазон чисел, в то время как метод `rangeClosed()` **включает** его в диапазон.