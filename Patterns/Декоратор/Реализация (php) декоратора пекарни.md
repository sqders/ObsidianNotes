### Теория паттерна Декоратор

Паттерн имеет один интерфейс, называемый **Component**, который реализуется двумя классами: **ConcreteComponent** и **Decorator**.

Класс **Decorator** имеет экземпляр **Component**, который может быть экземпляром **ConcreteComponent** или **Decorator**. Это позволит размещать несколько декораторов друг над другом, что добавляет новые функциональные возможности к переопределенным методам.

Последний тип класса, в шаблоне декоратора, это **ConcreteDecorator**. Это классы, которые декорируют **Component**, добавляя ему поведение.
![[Pasted image 20230829111215.png]]
### Реализация

Прежде всего, давайте обновим UML-диаграмму для нашего примера.
![[Pasted image 20230829111356.png]]
В этом примере интерфейс **Cake** имеет два метода: **ingredients** и **price**, которые возвращают ингредиенты и цену, соответственно.
```php
interface Cake
{
    public function ingredients(): string;
  
    public function price(): double;
}
```
**CakeDecorator** — это абстрактный класс, который заставляет каждый конкретный декоратор, расширяющий **CakeDecorator**, реализовывать свои собственные версии **ingredients** и **price**. Обратите внимание, что **CakeDecorator** получает экземпляр **Cake**, который делает возможным создание цепочек.
```php
abstract class CakeDecorator implements Cake
{
    /**
     * @var Cake
     */
    protected $cake;
    public function __construct(Cake $cake)
    {
        $this->cake = $cake;
    }
}
```
Класс **SimpleCake** является примером класса **Component**, который можно декорировать. Эта функциональность очень легко расширяема. Если пекарня хочет сделать дополнительную опцию для шоколадных тортов, все, что нужно, это создать новый компонент, который называется **ChocolateCake**. Существующий код остается нетронутым.
```php
class SimpleCake implements Cake
{
    public function ingredients(): string
    {
        return 'Простой торт';
    }
    public function price(): double
    {
        return 12.5;
    }
}
```
**WithWhippedCream** — это класс декоратора, который расширяет **CakeDecorator**. Этот класс использует родительское свойство **$cake**, чтобы добавить поведение (декорировать) родителю.
```php
class WithWhippedCream extends CakeDecorator
{
    public function ingredients(): string
    {
        return $this->cake->ingredients() + ' со взбитыми сливками';
    }
    public function price(): double
    {
        return $this->cake->price() + 2.5;
    }
}
```
Класс **WithSprinkles** выглядит почти так же, как класс **WithWhippedCream**. Разница между ними заключается в том, что каждый из них украшает своего родителя по-своему. Оба имеют разную цену и разные ингредиенты.
```php
class WithSprinkles extends CakeDecorator
{
    public function ingredients(): string
    {
        return $this->cake->ingredients() + ' с обсыпкой';
    }
    public function price(): double
    {
        return $this->cake->price() + 1.25;
    }
}
```
Видите, как легко расширить функциональность? Допустим, пекарня хочет предоставить покупателям больше возможностей для украшения своих тортов. Например, им в голову пришла блестящую идея: начать продавать клубнику. Все, что нам нужно сделать, это создать новый декоратор под названием **WithStrawberries**. Опять же, существующий код остается нетронутым.
### Настало время испечь наш собственный торт

Теперь, когда вся основная работа выполнена, пришло время готовки.
```php
$cake = new SimpleCake();
$cake = new WithWhippedCream($cake);
$cake = new WithSprinkles($cake);
echo $cake->ingredients(); // Простой торт со взбитыми сливками с обсыпкой
echo $cake->price(); // 16.25
// или можно цепочкой
$cake = new WithSprinkles(new WithWhippedCream(new SimpleCake()));
echo $cake->ingredients(); // Простой торт со взбитыми сливками с обсыпкой
echo $cake->price(); // 16.25
```
