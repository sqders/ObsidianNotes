# Определение
Необходим для различных действий с массивами
# Наиболее важные методы
1. `Arrays.asList(T[] arrT)` - преобразовывает `T[]` в `ArrayList<T>`
2. `Arrays.sort()` - сортирует коллекцию, даже если не имплементирован интерфейс [[Comparable и Comparator]], но возбуждает исключение `ClassCastException`, надо быть осторожным
3. `Arrays.binarySearch()` - работает как [[Collections - util class|Collections.binarySearch()]]