Сначала нам нужно определить пример. Мы работаем над приложением для производителя транспортных средств. Изначально у нас был только клиент. Этот клиент создавал транспортные средства с двигателем, работающим только на топливе. Итак, чтобы следовать принципу [единой ответственности](https://www.baeldung.com/solid-principles#s) (SRP) и принципу [открытия-закрытия](https://www.baeldung.com/solid-principles#o) (OCP), мы используем шаблон проектирования factory method.

Прежде чем мы перейдем к какому-либо коду, мы определяем для этого шаблона диаграмму [UML](https://www.baeldung.com/java-composition-aggregation-association) по умолчанию:

[![Шаблон метода фабрики по умолчанию](https://www.baeldung.com/wp-content/uploads/2022/11/factory_design_pattern_base.png)](https://www.baeldung.com/wp-content/uploads/2022/11/factory_design_pattern_base.png)

Используя приведенную выше диаграмму UML в качестве ссылки, мы определяем некоторые основные понятия, связанные с этим шаблоном. **Шаблон метода factory ослабляет код связи, отделяя код построения нашего _продукта_ от кода, который использует этот _продукт_**. Этот дизайн позволяет легко извлекать конструкцию _продукта_ независимо от остальной части приложения. Кроме того, он позволяет внедрять новые продукты, не нарушая существующий код.

Давайте перейдем к коду. Сначала в нашем примере приложения мы определяем интерфейс _MotorVehicle_. У этого интерфейса есть только метод _build()_. Этот метод используется для сборки конкретного автомобиля. Фрагмент кода интерфейса:

```java
public interface MotorVehicle {
    void build();
}
```

Следующим шагом является реализация конкретных классов, которые реализуют интерфейс _MotorVehicle_. Мы создаем два типа: _мотоцикл_ и _автомобиль_. Код для первого из них:

```java
public class Motorcycle implements MotorVehicle {
    @Override
    public void build() {
        System.out.println("Build Motorcycle");
    }
}
```

В случае класса _Car_ код является:

```java
public class Car implements MotorVehicle {
    @Override
    public void build() {
        System.out.println("Build Car");
    }
}
```

Затем мы создаем класс _MotorVehicleFactory_. Этот класс отвечает за создание каждого нового экземпляра транспортного средства. Это абстрактный класс, потому что он создает конкретное транспортное средство для своей конкретной фабрики. Код для этого класса:

```java
public abstract class MotorVehicleFactory {
    public MotorVehicle create() {
        MotorVehicle vehicle = createMotorVehicle();
        vehicle.build();
        return vehicle;
    }
    protected abstract MotorVehicle createMotorVehicle();
}
```

Как вы можете заметить, метод _create()_ вызывает абстрактный метод _createMotorVehicle()_ для создания определенного типа транспортного средства. Вот почему каждый конкретный завод по производству автотранспортных средств должен реализовать свой правильный тип _MotorVehicle_. Ранее мы реализовали два типа _автотранспортных_ средств: _мотоцикл_ и _автомобиль_. Теперь мы расширяем наш базовый класс _MotorVehicleFactory_ для реализации обоих.

Во-первых, класс _MotorcycleFactory_ :

```java
public class MotorcycleFactory extends MotorVehicleFactory {
    @Override
    protected MotorVehicle createMotorVehicle() {
        return new Motorcycle();
    }
}
```

Затем класс _CarFactory_:

```java
public class CarFactory extends MotorVehicleFactory {
    @Override
    protected MotorVehicle createMotorVehicle() {
        return new Car();
    }
}
```

Вот и все. Наше приложение разработано с использованием шаблона заводского метода. Теперь мы можем добавлять столько новых автомобилей, сколько захотим. Наконец, нам нужно посмотреть, как выглядит наш окончательный дизайн с использованием нотации [UML](https://www.baeldung.com/java-composition-aggregation-association):

[![Результат шаблона фабричного метода](https://www.baeldung.com/wp-content/uploads/2022/11/factory_design_pattern_result.png)](https://www.baeldung.com/wp-content/uploads/2022/11/factory_design_pattern_result.png)