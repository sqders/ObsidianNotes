# Определение
**`HashSet`** - структура данных - множество - реализующая [[Интерфейс Set]] использующая [[Хэш Таблицы]]. В своей основе имеет устройство **HashMap** 
# Устройство
Под капотом, `Set` использует реализацию [[Интерфейс Map|интерфейса Map]], что и делает его методы такими быстрыми. Ключ этого внутреннего **Map** - и есть значение **Set**, значение внутреннего Map неважно - там просто константа какая-то.
**Не запоминает порядок добавления элементов.**
# Методы и использование
Элементы, добавляемые в **`HashSet`** должны реализовывать `equals()` и `hashcode()`
Отсутствует метод `get()` - т.к. в нём нет смысла, мы можем только проверить, есть ли элемент в множестве методом `contains()` и исходя из этой инфы уже делать то, что нам нужно.
**Методы `HashSet`:**
1. `boolean add(E e)`: добавляет элемент в `HashSet`, если таковой отсутствует, если же такой элемент уже присутствует, метод возвращает _false_.
2. `void clear():` удаляет все элементы из множества.
3. `boolean contains(Object o)`: Возвращает _true_, если данный элемент присутствует в множестве.
4. `boolean remove(Object o)`: удаляет данный элемент из множества, если таковой присутствует.
5. `Iterator iterator()`: возвращает итератор для элементов множества.
6. `Boolean isEmpty()`: возвращает _true_, если в множестве нет элементов.
7. `Object clone()`: выполняет поверхностное клонирование `HashSet`.

![[Pasted image 20230719145826.png]]