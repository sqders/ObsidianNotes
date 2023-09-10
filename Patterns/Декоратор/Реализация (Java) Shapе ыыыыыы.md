  
Мы создадим интерфейс Shape и конкретные классы, реализующие этот интерфейс. Затем мы создадим абстрактный класс-декоратор ShapeDecorator, реализующий интерфейс Shape и имеющий объект Shape в качестве поля класса.
![[Pasted image 20230829112530.png]]
  
— Shape — имя интерфейса
— Классы Rectangle, Triangle и класс Circle будут конкретными классами, реализующими интерфейс Shape 
— ShapeDecorator — это абстрактный класс-декоратор, реализующий тот же интерфейс Shape 
— RedShapeDecorator — конкретный класс, реализующий ShapeDecorator 
— Demo — демонстрационный класс, в котором мы будем использовать RedShapeDecorator для декорирования объектов Shape

### **Шаг 1**: создаем интерфейс Shape
```java
public interface Shape { 
	void draw();
}
```
### **Шаг 2**: создадим несколько реализаций этого интерфейса. 
В примере ниже будет только круг, а по факту создадим еще парочку: прямоугольник и треугольник.
```java
public class Circle implements Shape{ 
@Override 
public void draw() 
	{ 
		System.out.println("Я круг!"); 
	} 
}
```
### **Шаг 3**: создадим абстрактный декоратор, реализующий интерфейс Shape
```java
public abstract class ShapeDecorator implements Shape 
{ 
	protected Shape decoratedShape;
	//Конструктор, принимающий объект 
	Shape public ShapeDecorator(Shape decoratedShape) {
		this.decoratedShape = decoratedShape; 
	} 
	public void draw() { 
		decoratedShape.draw(); 
	} 
}
```
### **Шаг 4**: создадим конкретный класс-декоратор, наследующийся от абстрактного класса
```java
public class RedShapeDecorator extends ShapeDecorator{ 
	public RedShapeDecorator(Shape decoratedShape) { 
		super(decoratedShape); 
		} 
	@Override 
	public void draw() { 
		decoratedShape.draw();
		setRedBorder(decoratedShape);
		}
	private void setRedBorder(Shape decoratedShape) { 
		System.out.println("Сообщение от RedShapeDecorator. Цвет границы: красный"); 
		} 
}
```
### **Шаг 5**: используем RedShapeDecorator чтобы раскрасить наши объекты
```java
public class Demo { 
	public static void main(String[] args) { 
		Shape circle = new Circle(); 
		Shape redCircle= new RedShapeDecorator(new Circle());
		Shape redRectangle= new RedShapeDecorator(new Rectangle());
		Shape redTriangle = new RedShapeDecorator(new Triangle());
		System.out.println("\nОбычный круг:");
		circle.draw();
		System.out.println("\nКруг с красной границей:"); 
		redCircle.draw(); 
		System.out.println("\nПрямоугольник с красной границей:"); 
		redRectangle.draw(); 
		System.out.println("\nТреугольник с красной границей:"); 
		redTriangle.draw(); 
	} 
}
```
### **Шаг 6**: смотрим в консоль и радуемся
```
Обычный круг: 
Я круг! 
Круг с красной границей: 
Я круг! 
Сообщение от RedShapeDecorator. Цвет границы: красный 
Прямоугольник с красной границей: 
Я прямоугольник! 
Сообщение от RedShapeDecorator. Цвет границы: красный
Треугольник с красной границей: 
Я треугольник!
Сообщение от RedShapeDecorator. Цвет границы: красный
```