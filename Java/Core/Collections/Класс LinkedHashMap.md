# Определение
`LinkedhashMap` является наследником **`HashMap`** и хранит информацию о порядке добавления элементов или порядке их использования. Производительность методов немного ниже, чем у [[класс HashMap|HashMap]]
# Применение
Применяется, когда важен порядок **добавления/использования** элементов.
Это переключается в конструкторе, который последним параметром принимает **bool** значение - `accessOrder`. Если **true** - то порядок элементов меняется в зависимости от его использования в данный момент - только что использованные элементы становятся последними в `LinkedHashMap`, **false** - порядок элементов соответствует изначальному добавлению элементов в `LinkedHashMap`
# Итого
	Данная структура может слегка уступать по производительности родительскому **HashMap**, при этом время выполнения операций **add(), contains(), remove()** остается константой — O(1). Понадобится чуть больше места в памяти для хранения элементов и их связей, но это совсем небольшая плата за дополнительные фишечки.  
Вообще, из-за того что всю основную работу на себя берет родительский класс, серьезных отличий в реализации **HashMap** и **LinkedHashMap** не много. Можно упомянуть о парочке мелких:  
- Методы **transfer()** и **containsValue()** устроены чуть проще из-за наличия двунаправленной связи между элементами;
- В классе **LinkedHashMap.Entry** реализованы методы **recordRemoval()** и **recordAccess()** (тот самый, который помещает элемент в конец при **accessOrder = true**). В **HashMap** оба этих метода пустые.