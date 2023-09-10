После первой итерации нашего приложения нашей системой заинтересовались две новые компании-производители автомобилей: _NextGen_ и _FutureVehicle_. Эти новые компании производят не только автомобили, работающие только на топливе, но и электромобили. У каждой компании свой дизайн автомобиля.

Наша текущая система не готова к этим новым сценариям. Мы должны поддерживать электромобили и учитывать, что у каждой компании свой дизайн. Для решения этих проблем мы можем использовать [абстрактный шаблон фабрики](https://www.baeldung.com/java-abstract-factory-pattern). **Этот шаблон обычно используется, когда мы начинаем использовать шаблон метода Factory, и нам нужно развить нашу систему до более сложной системы**. **Он централизует код создания продукта в одном месте**. Представление UML является:

[![Абстрактный шаблон фабрики по умолчанию](https://www.baeldung.com/wp-content/uploads/2022/11/abstract_factory_design_pattern_base.png)](https://www.baeldung.com/wp-content/uploads/2022/11/abstract_factory_design_pattern_base.png)

У нас уже есть интерфейс _MotorVehicle_. Кроме того, мы должны добавить интерфейс для представления электромобилей. Фрагмент кода для нового интерфейса является:

```java
public interface ElectricVehicle {
    void build();
}
```

Далее мы создаем нашу абстрактную фабрику. Новый класс является абстрактным, потому что ответственность за создание объекта будет лежать на нашей конкретной фабрике. Это поведение соответствует [OCP](https://www.baeldung.com/solid-principles#o) и [SRP](https://www.baeldung.com/solid-principles#s). Давайте перейдем к определению класса:

```java
public abstract class Corporation {
    public abstract MotorVehicle createMotorVehicle();
    public abstract ElectricVehicle createElectricVehicle();
}
```

Прежде чем мы создадим конкретный завод для каждой компании, мы должны внедрить некоторые транспортные средства для наших новых компаний. Давайте создадим несколько новых классов для компании _FutureVehicle_.

```java
public class FutureVehicleMotorcycle implements MotorVehicle {
    @Override
    public void build() {
        System.out.println("Future Vehicle Motorcycle");
    }
}
```

Затем экземпляр электромобиля:

```java
public class FutureVehicleElectricCar implements ElectricVehicle {
    @Override
    public void build() {
        System.out.println("Future Vehicle Electric Car");
    }
}
```

Мы делаем то же самое для компании _NexGen_:

```java
public class NextGenMotorcycle implements MotorVehicle {
    @Override
    public void build() {
        System.out.println("NextGen Motorcycle");
    }
}
```

Кроме того, другая конкретная реализация электромобиля:

```java
public class NextGenElectricCar implements ElectricVehicle {
    @Override
    public void build() {
        System.out.println("NextGen Electric Car");
    }
}
```

Наконец, мы готовы строить наши бетонные заводы. Во-первых, завод _FutureVehicle_ :

```java
public class FutureVehicleCorporation extends Corporation {
    @Override
    public MotorVehicle createMotorVehicle() {
        return new FutureVehicleMotorcycle();
    }
    @Override
    public ElectricVehicle createElectricVehicle() {
        return new FutureVehicleElectricCar();
    }
}
```

Далее, другой:

```java
public class NextGenCorporation extends Corporation {
    @Override
    public MotorVehicle createMotorVehicle() {
        return new NextGenMotorcycle();
    }
    @Override
    public ElectricVehicle createElectricVehicle() {
        return new NextGenElectricCar();
    }
}
```

И это сделано. Мы завершаем реализацию, используя [абстрактный шаблон Factory](https://www.baeldung.com/java-abstract-factory-pattern). Вот диаграмма UML для нашей пользовательской реализации:

[![Результат абстрактного шаблона фабрики](https://www.baeldung.com/wp-content/uploads/2022/11/abstract_factory_design_pattern_result.png)](https://www.baeldung.com/wp-content/uploads/2022/11/abstract_factory_design_pattern_result.png)