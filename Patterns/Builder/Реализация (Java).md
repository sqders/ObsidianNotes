- Паттерн проектирования Builder разработан для обеспечения гибкого решения различных задач создания объектов в объектно-ориентированном программировании.
- Паттерн проектирования Builder позволяет отделить построение сложного объекта от его представления.
- Паттерн Builder создает сложные объекты, используя простые объекты и поэтапный подход.
- Паттерн предоставляет один из лучших способов создания сложных объектов.
- Это один из [паттернов проектирования банды четырех (GoF),](https://en.wikipedia.org/wiki/Design_Patterns) которые описывают, как решать периодически возникающие задачи проектирования в объектно-ориентированном программном обеспечении.
- Этот паттерн полезен для создания разных иммутабельных объектов с помощью одного и того же процесса построения объекта.
**Паттерн проектирования** **Builder** решает такие проблемы, как:
- Как класс (тот же самый процесс строительства) может создавать различные представления сложного объекта?
- Как можно упростить класс, занимающийся созданием сложного объекта?

### Пример со сборкой автомобилей с использованием паттерна проектирования Builder

**Шаг 1:** Создайте класс **Car** (автомобиль), который в нашем примере является продуктом:

```java
package org.trishinfotech.builder;

public class Car {

    private String chassis;
    private String body;
    private String paint;
    private String interior;
    
    public Car() {

        super();
    }

    public Car(String chassis, String body, String paint, String interior) {
        this();
        this.chassis = chassis;
        this.body = body;
        this.paint = paint;
        this.interior = interior;
    }

    public String getChassis() {
        return chassis;
    }

		public void setChassis(String chassis) {
        this.chassis = chassis;

    }

    public String getBody() {
        return body;
    }

    public void setBody(String body) {
        this.body = body;
    }

    public String getPaint() {
        return paint;
    }

    public void setPaint(String paint) {
        this.paint = paint;
    }
		public String getInterior() {
        return interior;
    }

    public void setInterior(String interior) {
        this.interior = interior;
    }

    public boolean doQualityCheck() {
        return (chassis != null && !chassis.trim().isEmpty()) && (body != null && !body.trim().isEmpty())
                && (paint != null && !paint.trim().isEmpty()) && (interior != null && !interior.trim().isEmpty());
    }

    @Override
    public String toString() {
        // StringBuilder class also uses Builder Design Pattern with implementation of java.lang.Appendable interface
        StringBuilder builder = new StringBuilder();
        builder.append("Car [chassis=").append(chassis).append(", body=").append(body).append(", paint=").append(paint)
        return builder.toString();
    }

}
```

Обратите внимание, что я добавил в класс проверочный метод `doQualityCheck`. Я считаю, что Builder не должен создавать неполные или невалидные Product-объекты. Таким образом, этот метод поможет нам в проверке сборки автомобилей.

**Шаг 2:** Создайте абстрактный класс/интерфейс `CarBuilder`, в котором определите все необходимые шаги для создания автомобиля.

```java
package org.trishinfotech.builder;

public interface CarBuilder {

    // Этап 1
    public CarBuilder fixChassis();

    // Этап 2
    public CarBuilder fixBody();

    // Этап 3
    public CarBuilder paint();

    // Этап 4

    public CarBuilder fixInterior();

    // Выпуск автомобиля

    public Car build();
}
```

Обратите внимание, что я сделал тип `CarBuilder` типом возврата всех этапов, созданных здесь. Это позволит нам вызывать этапы по цепочке. Здесь есть один очень важный метод `build`_,_ который заключается в том, чтобы получить результат или создать конечный объект `Car`. Этот метод фактически проверяет годность автомобиля и выпускает (возвращает) его только в том случае, если его сборка завершена успешно (все валидно).

**Шаг 3:** Теперь пора написать `ConcreteBuilder`. Как я уже упоминал, у нас могут быть разные варианты `ConcreteBuilder`, и каждый из них выполняет сборку по-своему, чтобы предоставить нам различные представления сложного объекта `Car`.

Итак, ниже приведен код `ClassicCarBuilder`, который собирает старые модели автомобилей.

```java
package org.trishinfotech.builder;

public class ClassicCarBuilder implements CarBuilder {

    private String chassis;
    private String body;
    private String paint;
    private String interior;

    public ClassicCarBuilder() {
        super();
    }

    @Override
    public CarBuilder fixChassis() {
        System.out.println("Assembling chassis of the classical model");
        this.chassis = "Classic Chassis";
        return this;
    }

    @Override
    public CarBuilder fixBody() {
        System.out.println("Assembling body of the classical model");
        this.body = "Classic Body";
        return this;
    }

    @Override
    public CarBuilder paint() {
        System.out.println("Painting body of the classical model");
        this.paint = "Classic White Paint";
        return this;
    }
  
    @Override
    public CarBuilder fixInterior() {
        System.out.println("Setting up interior of the classical model");
        this.interior = "Classic interior";
        return this;
    }

    @Override
		public Car build() {
        Car car = new Car(chassis, body, paint, interior);
        if (car.doQualityCheck()) {
            return car;
        } else {
            System.out.println("Car assembly is incomplete. Can't deliver!");
        }
        return null;
    }

}
```

Теперь напишем еще один строитель `ModernCarBuilder` для сборки последней модели автомобиля.

```java
package org.trishinfotech.builder;

public class ModernCarBuilder implements CarBuilder {

    private String chassis;
    private String body;
    private String paint;
    private String interior;

    public ModernCarBuilder() {
        super();
    }

    @Override
    public CarBuilder fixChassis() {
        System.out.println("Assembling chassis of the modern model");
        this.chassis = "Modern Chassis";
        return this;
    }

    @Override
    public CarBuilder fixBody() {
        System.out.println("Assembling body of the modern model");
        this.body = "Modern Body";
        return this;
    }
  
    @Override
    public CarBuilder paint() {
        System.out.println("Painting body of the modern model");
        this.paint = "Modern Black Paint";
        return this;
    }

    @Override
    public CarBuilder fixInterior() {
        System.out.println("Setting up interior of the modern model");
        this.interior = "Modern interior";
        return this;
    }

    @Override
    public Car build() {
        Car car = new Car(chassis, body, paint, interior);
        if (car.doQualityCheck()) {
            return car;
        } else {
            System.out.println("Car assembly is incomplete. Can't deliver!");
        }
        return null;
    }
}
```

 И еще один `SportsCarBuilder` для создания спортивного автомобиля.

```java
package org.trishinfotech.builder;

public class SportsCarBuilder implements CarBuilder {

    private String chassis;
    private String body;
    private String paint;
    private String interior;

    public SportsCarBuilder() {
        super();
    }

    @Override
    public CarBuilder fixChassis() {
        System.out.println("Assembling chassis of the sports model");
        this.chassis = "Sporty Chassis";
        return this;
    }
 
    @Override
    public CarBuilder fixBody() {
        System.out.println("Assembling body of the sports model");
        this.body = "Sporty Body";
        return this;
    }
  
    @Override
    public CarBuilder paint() {
        System.out.println("Painting body of the sports model");
        this.paint = "Sporty Torch Red Paint";
        return this;
    }

    @Override
    public CarBuilder fixInterior() {
        System.out.println("Setting up interior of the sports model");
        this.interior = "Sporty interior";
        return this;
    }

    @Override
    public Car build() {
        Car car = new Car(chassis, body, paint, interior);
        if (car.doQualityCheck()) {
            return car;
        } else {
            System.out.println("Car assembly is incomplete. Can't deliver!");
        }
        return null;
    }

}
```

**Шаг 4:** Теперь мы напишем класс-распорядитель `AutomotiveEngineer`, под руководством которого строитель будет собирать автомобиль (объект `Car`) шаг за шагом в четко определенном порядке.

```java
package org.trishinfotech.builder;

public class AutomotiveEngineer {

    private CarBuilder builder;

    public AutomotiveEngineer(CarBuilder builder) {
        super();
        this.builder = builder;
        if (this.builder == null) {
            throw new IllegalArgumentException("Automotive Engineer can't work without Car Builder!");
        }
    }

    public Car manufactureCar() {
        return builder.fixChassis().fixBody().paint().fixInterior().build();
    }

}
```

 Мы видим, что метод `manufactureCar` вызывает этапы сборки автомобиля в правильном порядке.

Теперь пришло время написать класс `Main` для выполнения и тестирования нашего кода.

```java
package org.trishinfotech.builder;

public class Main {

    public static void main(String[] args) {
        CarBuilder builder = new SportsCarBuilder();
        AutomotiveEngineer engineer = new AutomotiveEngineer(builder);
        Car car = engineer.manufactureCar();
        if (car != null) {
            System.out.println("Below car delievered: ");
            System.out.println("======================================================================");
            System.out.println(car);
            System.out.println("======================================================================");
        }
    }

}
```

 Ниже приведен вывод программы:

```
Assembling chassis of the sports model
Assembling body of the sports model
Painting body of the sports model
Setting up interior of the sports model
Below car delievered: 
======================================================================
Car [chassis=Sporty Chassis, body=Sporty Body, paint=Sporty Torch Red Paint, interior=Sporty interior]
======================================================================
```

 Я надеюсь, что вы хорошо разобрались в объяснении и примере, чтобы понять паттерн `Builder`. Некоторые из нас также находят у него сходство с [**паттерном абстрактной фабрики (Abstract Factory)**](https://dzone.com/articles/abstract-factory-pattern-in-java), о котором я рассказывал в другой статье. Основное различие между строителем и абстрактной фабрикой состоит в том, что строитель предоставляет нам больший или лучший контроль над процессом создания объекта. Если вкратце, то паттерн абстрактной фабрики отвечает на вопрос «что», а паттерн строитель - «как».

 Исходный код можно найти здесь: [Real-Builder-Design-Pattern-Source-Code](https://github.com/BrijeshSaxena/design-pattern-real-builder)

Я нашел паттерн `Builder` невероятно полезным и одним из наиболее часто используемых в приложениях в настоящее время. Я пришел к выводу, что `Builder` лучше подходит для работы с иммутабельными объектами. Все мы знаем, как много есть хороших иммутабельных объектов, и их использование увеличивается день ото дня, особенно после релиза Java 8.

Я использую `Builder` для написания своих сложных иммутабельных классов, и я бы хотел продемонстриовать здесь эту идею.

В качестве примера у нас есть класс `Employee`, в котором есть несколько полей.

```java
public class Employee {

    private int empNo;
    private String name;
    private String depttName;
    private int salary;
    private int mgrEmpNo;
    private String projectName;
}
```

 Предположим, только два поля `EmpNo` и `EmpName` являются обязательными, а все остальные - опциональные. Поскольку это иммутабельный класс, у меня есть два варианта написания конструкторов.

1. Написать конструктор с параметрами под все поля.
    
2. Написать несколько конструкторов для разных комбинаций параметров, чтобы создать разные представления объекта `Employee`.
    

Я решил, что первый вариант мне не подходит, так как мне не нравится, когда в методе больше трех-четырех параметров. Это выглядит не очень хорошо и становится еще хуже, когда многие параметры равны нулю или `null`.

```java
Employee emp1 = new Employee (100, "Brijesh", null, 0, 0, "Builder Pattern");
```

 Второй вариант тоже не очень хорош, так как мы создаем слишком много конструкторов.

```java
  public Employee(int empNo, String name) {
        super();
        if (empNo <= 0) {
            throw new IllegalArgumentException("Please provide valid employee number.");
        }
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Please provide employee name.");
        }
        this.empNo = empNo;
        this.name = name;
    }

    public Employee(int empNo, String name, String depttName) {
        this(empNo, name);
        this.depttName = depttName;
    }

    public Employee(int empNo, String name, String depttName, int salary) {
        this(empNo, name, depttName);
        this.salary = salary;
    }

    public Employee(int empNo, String name, String depttName, int salary, int mgrEmpNo) {
        this(empNo, name, depttName, salary);
        this.mgrEmpNo = mgrEmpNo;
    }

    public Employee(int empNo, String name, String depttName, int salary, int mgrEmpNo, String projectName) {
        this(empNo, name, depttName, salary, mgrEmpNo);
        this.projectName = projectName;
    }
```

 Итак, вот решение с помощью паттерна `Builder`:

```java
package org.trishinfotech.builder.example;

public class Employee {

    private int empNo;
    private String name;
    private String depttName;
    private int salary;
    private int mgrEmpNo;
    private String projectName;

    public Employee(EmployeeBuilder employeeBuilder) {
        if (employeeBuilder == null) {
            throw new IllegalArgumentException("Please provide employee builder to build employee object.");
        }
        if (employeeBuilder.empNo <= 0) {
            throw new IllegalArgumentException("Please provide valid employee number.");
        }
        if (employeeBuilder.name == null || employeeBuilder.name.trim().isEmpty()) {
            throw new IllegalArgumentException("Please provide employee name.");
        }
        this.empNo = employeeBuilder.empNo;
        this.name = employeeBuilder.name;
        this.depttName = employeeBuilder.depttName;
        this.salary = employeeBuilder.salary;
        this.mgrEmpNo = employeeBuilder.mgrEmpNo;
        this.projectName = employeeBuilder.projectName;
    }

    public int getEmpNo() {
        return empNo;
    }

    public String getName() {
        return name;
    }
    
    public String getDepttName() {
        return depttName;
    }

    public int getSalary() {
        return salary;
    }

    public int getMgrEmpNo() {
        return mgrEmpNo;
    }
    
    public String getProjectName() {
        return projectName;
    }

    @Override
    public String toString() {
        // Класс StringBuilder также использует паттерн проектирования Builder с реализацией
        // интерфейса java.lang.Appendable
        StringBuilder builder = new StringBuilder();
        builder.append("Employee [empNo=").append(empNo).append(", name=").append(name).append(", depttName=")
                .append(depttName).append(", salary=").append(salary).append(", mgrEmpNo=").append(mgrEmpNo)
                .append(", projectName=").append(projectName).append("]");
        return builder.toString();
    }

    public static class EmployeeBuilder {
        private int empNo;
        protected String name;
        protected String depttName;
        protected int salary;
        protected int mgrEmpNo;
        protected String projectName;

        public EmployeeBuilder() {
            super();
        }
        
        public EmployeeBuilder empNo(int empNo) {
            this.empNo = empNo;
            return this;
        }

        public EmployeeBuilder name(String name) {
            this.name = name;
            return this;
        }

        public EmployeeBuilder depttName(String depttName) {
            this.depttName = depttName;
            return this;
        }

        public EmployeeBuilder salary(int salary) {
            this.salary = salary;
            return this;
        }

        public EmployeeBuilder mgrEmpNo(int mgrEmpNo) {
            this.mgrEmpNo = mgrEmpNo;
            return this;
        }
        public EmployeeBuilder projectName(String projectName) {
            this.projectName = projectName;
            return this;
        }

        public Employee build() {
            Employee emp = null;
            if (validateEmployee()) {
                emp = new Employee(this);
            } else {
                System.out.println("Sorry! Employee objects can't be build without required details");
            }
            return emp;
        }

        private boolean validateEmployee() {
           return (empNo > 0 && name != null && !name.trim().isEmpty());
        }
    }
}
```

Я написал `EmployeeBuilder` как публичный статический вложенный класс. Вы можете написать его как обычный публичный класс в отдельном файл Java. Большой разницы я не вижу.

Теперь напишем программу `EmployeeMain` для создания объекта `Employee`:

```java
package org.trishinfotech.builder.example;

public class EmployeeMain {

    public static void main(String[] args) {
        Employee emp1 = new Employee.EmployeeBuilder().empNo(100).name("Brijesh").projectName("Builder Pattern")
                .build();
        System.out.println(emp1);
    }
}
```

Надеюсь, вам понравилась идея. Мы можем использовать это при создании более сложных объектов. Я не реализовал здесь распорядителя (**Director**), так как все шаги (сбор значений для полей) не являются обязательными и могут выполняться в любом порядке. Чтобы убедиться, что я создаю объект `Employee` только после получения всех обязательных полей, я написал **метод проверки**.

### Пример с оформлением заказа в ресторане с использованием паттерна Builder

Я хочу еще показать вам пример кода для оформления заказа в ресторане, где Order (заказ) является иммутабельным объектом и требует тип обслуживания заказа - Order Service Type (Take Away - с собой/Eat Here - в заведении), всех необходимых нам продуктов питания (Food Items) и имени клиента (Customer Name - опционально) в время оформления заказа. Продуктов питания может быть сколько угодно. Итак, вот код этого примера.

Код для перечисления `OrderService`:

```java
package org.trishinfotech.builder;

public enum OrderService {

    TAKE_AWAY("Take Away", 2.0d), EAT_HERE("Eat Here", 5.5d);

    private String name;
    private double tax;

    OrderService(String name, double tax) {
        this.name = name;
        this.tax = tax;
    }

    public String getName() {
        return name;
    }

    public double getTax() {
        return tax;
    }

}
```

 Код для интерфейса `FoodItem`:

```java
package org.trishinfotech.builder.meal;

import org.trishinfotech.builder.packing.Packing;

public interface FoodItem {

    public String name();

    public int calories();

    public Packing packing();

    public double price();
}
```

 Код для класса `Meal` (блюдо). Класс `Meal` предлагает заранее определенные продукты питания со скидкой на цену товара (не на цену упаковки).

```java
package org.trishinfotech.builder.meal;

import java.util.ArrayList;
import java.util.List;
import java.util.Objects;
import java.util.stream.Collectors;

import org.trishinfotech.builder.packing.MultiPack;
import org.trishinfotech.builder.packing.Packing;

public class Meal implements FoodItem {

    private List<FoodItem> foodItems = new ArrayList<FoodItem>();
    private String mealName;
    private double discount;

    public Meal(String mealName, List<FoodItem> foodItems, double discount) {
        super();
        if (Objects.isNull(foodItems) || foodItems.stream().filter(Objects::nonNull).collect(Collectors.toList()).isEmpty()) {
            throw new IllegalArgumentException(
                    "Meal can't be order without any food item");
        }
        this.mealName = mealName;
        this.foodItems = new ArrayList<FoodItem>(foodItems);
        this.discount = discount;
    }

    public List<FoodItem> getFoodItems() {
        return foodItems;
    }

    @Override
    public String name() {
        return mealName;
    }
  
    @Override
    public int calories() {
        int totalCalories = foodItems.stream().mapToInt(foodItem -> foodItem.calories()).sum();
        return totalCalories;
    }

    @Override
    public Packing packing() {
        double packingPrice = foodItems.stream().map(foodItem -> foodItem.packing())
                .mapToDouble(packing -> packing.packingPrice()).sum();
        return new MultiPack(packingPrice);
    }

    @Override
    public double price() {
        double totalPrice = foodItems.stream().mapToDouble(foodItem -> foodItem.price()).sum();
        return totalPrice;
    }

    public double discount() {
        return discount;
    }
}
```

### Еда:

Код для класса `Burger`:

```java
package org.trishinfotech.builder.food.burger;

import org.trishinfotech.builder.meal.FoodItem;
import org.trishinfotech.builder.packing.Packing;
import org.trishinfotech.builder.packing.Wrap;

public abstract class Burger implements FoodItem {

    @Override
    public Packing packing() {
        return new Wrap();
    }

}
```

Код для класса `ChickenBurger`:

```java
package org.trishinfotech.builder.food.burger;

public class ChickenBurger extends Burger {

    @Override
    public String name() {
        return "Chicken Burger";
    }

    @Override
    public int calories() {
        return 300;
    }

    @Override
    public double price() {
        return 4.5d;
    }

}
```

Код для класса `VegBurger` (веганский бургер):

```java
package org.trishinfotech.builder.food.burger;

public class VegBurger extends Burger {

    @Override
    public String name() {
        return "Veg Burger";
    }

    @Override
    public int calories() {
        return 180;
    }

    @Override
    public double price() {
        return 2.7d;
    }

}
```

Код для класса `Nuggets`:

```java
package org.trishinfotech.builder.food.nuggets;

import org.trishinfotech.builder.meal.FoodItem;
import org.trishinfotech.builder.packing.Container;
import org.trishinfotech.builder.packing.Packing;

public abstract class Nuggets implements FoodItem {

    @Override
    public Packing packing() {
        return new Container();
    }

}
```

 Код для класса `CheeseNuggets`:

```java
package org.trishinfotech.builder.food.nuggets;

public class CheeseNuggets extends Nuggets {

    @Override
    public String name() {
        return "Cheese Nuggets";
    }

    @Override
    public int calories() {
        return 330;
    }

    @Override
    public double price() {
        return 3.8d;
    }

}
```

Код для класса `ChickenNuggets`:

```java
package org.trishinfotech.builder.food.nuggets;

public class ChickenNuggets extends Nuggets {

    @Override
    public String name() {
        return "Chicken Nuggets";
    }

    @Override
    public int calories() {
        return 450;
    }

    @Override
    public double price() {
        return 5.0d;
    }

}
```

### Напитки:

Напитки бывают разных размеров. Итак, вот код перечисления `BeverageSize`:

```java
package org.trishinfotech.builder.beverages;

public enum BeverageSize {
    XS("Extra Small", 110), S("Small", 150), M("Medium", 210), L("Large", 290);
    private String name;
    private int calories;

    BeverageSize(String name, int calories) {
        this.name = name;
        this.calories = calories;
    }
  
    public String getName() {
        return name;
    }

    public int getCalories() {
        return calories;
    }

}
```

Код для класса `Drink`:

```java
package org.trishinfotech.builder.beverages;

import org.trishinfotech.builder.meal.FoodItem;

public abstract class Drink implements FoodItem {

    protected BeverageSize size;

    public Drink(BeverageSize size) {
        super();
        this.size = size;
        if (this.size == null) {
            this.size = BeverageSize.M;
        }
    }

    public BeverageSize getSize() {
        return size;
    }

    public String drinkDetails() {
        return " (" + size + ")";
    }
}
```

 Код для класса `ColdDrink`:

```java
package org.trishinfotech.builder.beverages.cold;

import org.trishinfotech.builder.beverages.BeverageSize;
import org.trishinfotech.builder.beverages.Drink;
import org.trishinfotech.builder.packing.Bottle;
import org.trishinfotech.builder.packing.Packing;

public abstract class ColdDrink extends Drink {

    public ColdDrink(BeverageSize size) {
        super(size);
    }

    @Override public Packing packing() {
        return new Bottle();
    }
}
```

 Код для класса `CocaCola`:

```java
package org.trishinfotech.builder.beverages.cold;

import org.trishinfotech.builder.beverages.BeverageSize;

public class CocaCola extends ColdDrink {

    public CocaCola(BeverageSize size) {
        super(size);
    }

    @Override
    public String name() {
        return "Coca-Cola" + drinkDetails();
    }

    @Override
    public int calories() {
        if (size != null) {
            switch (size) {
            case XS:
                return 110;
            case S:
                return 150;
            case M:
                return 210;
            case L:
                return 290;
            default:
                break;
            }
        }
        return 0;
    }

    @Override
    public double price() {
        if (size != null) {
            switch (size) {
            case XS:
                return 0.80d;
            case S:
                return 1.0d;
            case M:
                return 1.5d;
            case L:
                return 2.0d;
            default:
                break;
            }
        }
        return 0.0d;
    }

}
```

Код для класса `Pepsi`:

```java
package org.trishinfotech.builder.beverages.cold;

import org.trishinfotech.builder.beverages.BeverageSize;

public class Pepsi extends ColdDrink {

    public Pepsi(BeverageSize size) {
        super(size);
    }

    @Override public String name() {
        return "Pepsi" + drinkDetails();
    }

    @Override public int calories() {
        if (size != null) {
            switch (size) {
                case S:
                    return 160;
                case M:
                    return 220;
                case L:
                    return 300;
                default:
                    break;
            }
        }
        return 0;
    }

    @Override public double price() {
        if (size != null) {
            switch (size) {
                case S:
                    return 1.2d;
                case M:
                    return 2.2d;
                case L:
                    return 2.7d;
                default:
                    break;
            }
        }
        return 0.0d;
    }

}
```

 Код для класса `HotDrink`:

```java
package org.trishinfotech.builder.beverages.hot;

import org.trishinfotech.builder.beverages.BeverageSize;
import org.trishinfotech.builder.beverages.Drink;
import org.trishinfotech.builder.packing.Packing;
import org.trishinfotech.builder.packing.SipperMug;

public abstract class HotDrink extends Drink {

    public HotDrink(BeverageSize size) {
        super(size);
    }
    
    @Override public Packing packing() {
        return new SipperMug();
    }
}
```

Код для класса `Cuppuccinno`:

```java
package org.trishinfotech.builder.beverages.hot;

import org.trishinfotech.builder.beverages.BeverageSize;

public class Cappuccino extends HotDrink {

    public Cappuccino(BeverageSize size) {
        super(size);
    }

    @Override public String name() {
        return "Cappuccino" + drinkDetails();
    }
  
    @Override public int calories() {
        if (size != null) {
            switch (size) {
                case S:
                    return 120;
                case M:
                    return 160;
                case L:
                    return 210;
                default:
                break;
            }
        }
        return 0;
    }

    @Override public double price() {
        if (size != null) {
            switch (size) {
                case S:
                    return 1.0d;
                case M:
                    return 1.4d;
                case L:
                    return 1.8d;
                default:
                break;
            }
        }
        return 0.0d;
    }

}
```

Код для класса `HotChocolate`:

```java
package org.trishinfotech.builder.beverages.hot;

import org.trishinfotech.builder.beverages.BeverageSize;

public class HotChocolate extends HotDrink {

    public HotChocolate(BeverageSize size) {
        super(size);
    }

    @Override public String name() {
        return "Hot Chocolate" + drinkDetails();
    }
  
    @Override public int calories() {
        if (size != null) {
            switch (size) {
                case S:
                    return 370;
                case M:
                    return 450;
                case L:
                    return 560;
                default:
                    break;
            }
        }
        return 0;
    }
    
    @Override public double price() {
        if (size != null) {
            switch (size) {
                case S:
                    return 1.6d;
                case M:
                    return 2.3d;
                case L:
                    return 3.0d;
                default:
                    break;
            }
          
       }
        return 0.0d;
    }

}
```

### Упаковка:

Код интерфейса `Packing`:

```java
package org.trishinfotech.builder.packing;

public interface Packing {

    public String pack();

    public double packingPrice();
}
```

Код для класса `Bottle`:

```java
package org.trishinfotech.builder.packing;

public class Bottle implements Packing {

    @Override
    public String pack() {
        return "Bottle";
    }

    @Override
    public double packingPrice() {
        return 0.75d;
    }

}
```

Код для класса `Container`:

```java
package org.trishinfotech.builder.packing;

public class Container implements Packing {

    @Override
    public String pack() {
        return "Container";
    }

    @Override
    public double packingPrice() {
        return 1.25d;
    }

}
```

Код для класса `MultiPack`. Упаковка `MutiPack` служит вспомогательной упаковкой для еды, когда мы используем разные упаковки для разных продуктов.

```java
package org.trishinfotech.builder.packing;

public class MultiPack implements Packing {

    private double packingPrice;


    public MultiPack(double packingPrice) {
        super();
        this.packingPrice = packingPrice;
    }
  
    @Override
    public String pack() {
        return "Multi-Pack";
    }

    @Override
    public double packingPrice() {
        return packingPrice;
    }

}
```

Код для класса `SipperMug`:

```java
package org.trishinfotech.builder.packing;

public class SipperMug implements Packing {

    @Override
    public String pack() {
        return "Sipper Mug";
    }

    @Override
    public double packingPrice() {
        return 1.6d;
    }

}
```

Код для класса `Wrap`:

```java
package org.trishinfotech.builder.packing;

public class Wrap implements Packing {

    @Override
    public String pack() {
        return "Wrap";
    }

    @Override
    public double packingPrice() {
        return 0.40d;
    }

}
```

Код служебного класса `BillPrinter`, который я написал для печати детализированного счета.

```java
package org.trishinfotech.builder.util;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.concurrent.atomic.DoubleAdder;

import org.trishinfotech.builder.Order;
import org.trishinfotech.builder.OrderService;
import org.trishinfotech.builder.meal.Meal;
import org.trishinfotech.builder.packing.Packing;

public class BillPrinter {

    static DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss");

    public static void printItemisedBill(Order order) {
        OrderService service = order.getService();
        System.out.printf("%60s\n", "Food Court");
        System.out.println("=================================================================================================================");
        System.out.printf("Service: %10s (%2.2f Tax)                                                         Customer Name: %-20s\n", service.getName(), service.getTax(), order.getCustomerName());
        System.out.println("-----------------------------------------------------------------------------------------------------------------");
        System.out.printf("%25s | %10s | %10s | %10s | %15s | %10s | %10s\n", "Food Item", "Calories", "Packing", "Price", "Packing Price", "Discount %", "Total Price");
        System.out.println("-----------------------------------------------------------------------------------------------------------------");
        DoubleAdder itemTotalPrice = new DoubleAdder();
        order.getFoodItems().stream().forEach(item -> {
            String name = item.name();
            int calories = item.calories();
            Packing packing = item.packing();
            double price = item.price();
            double packingPrice = packing.packingPrice();
            double discount = item instanceof Meal? ((Meal)item).discount() : 0.0d;
            double totalItemPrice = calculateTotalItemPrice(price, packingPrice, discount);
            System.out.printf("%25s | %10d | %10s | %10.2f | %15.2f | %10.2f | %10.2f\n", name, calories, packing.pack(), price, packing.packingPrice(), discount, totalItemPrice);
            itemTotalPrice.add(totalItemPrice);
        });
        System.out.println("=================================================================================================================");
        double billTotal = itemTotalPrice.doubleValue();
        billTotal = applyTaxes(billTotal, service);
        System.out.printf("Date: %-30s %66s %.2f\n", dtf.format(LocalDateTime.now()), "Total Bill (incl. taxes):", billTotal);
        System.out.println("Enjoy your meal!\n\n\n\n");
    }

    private static double applyTaxes(double billTotal, OrderService service) {
        return billTotal + (billTotal * service.getTax())/100;
    }

    private static double calculateTotalItemPrice(double price, double packingPrice, double discount) {
        if (discount > 0.0d) {
            price = price - (price * discount)/100;
        }
        return price + packingPrice;
    }
}
```

Почти все готово. Пришло время написать наш иммутабельный класс `Order`:

```java
package org.trishinfotech.builder;

import java.util.ArrayList;
import java.util.List;
import java.util.Objects;
import java.util.stream.Collectors;

import org.trishinfotech.builder.meal.FoodItem;

public class Order {

    private List<FoodItem> foodItems = new ArrayList<FoodItem>();

    private String customerName;
    private OrderService service;

    public Order(OrderService service, List<FoodItem> foodItems, String customerName) {
        super();
        if (Objects.isNull(service)) {
            throw new IllegalArgumentException(
                    "Meal can't be order without selecting service 'Take Away' or 'Eat Here'");
        }
        if (Objects.isNull(foodItems) || foodItems.stream().filter(Objects::nonNull).collect(Collectors.toList()).isEmpty()) {
            throw new IllegalArgumentException(
                    "Meal can't be order without any food item");
        }
        this.service = service;
        this.foodItems = new ArrayList<FoodItem>(foodItems);
        this.customerName = customerName;
        if (this.customerName == null) {
            this.customerName = "NO NAME";
        }
    }

    public List<FoodItem> getFoodItems() {
        return foodItems;
    }

    public String getCustomerName() {
        return customerName;
    }

    public OrderService getService() {
        return service;
    }

}
```

А вот код для `OrderBuilder`, который конструирует объект `Order`.

```java
package org.trishinfotech.builder;

import java.util.ArrayList;
import java.util.List;

import org.trishinfotech.builder.beverages.BeverageSize;
import org.trishinfotech.builder.beverages.cold.CocaCola;
import org.trishinfotech.builder.beverages.cold.Pepsi;
import org.trishinfotech.builder.food.burger.ChickenBurger;
import org.trishinfotech.builder.food.burger.VegBurger;
import org.trishinfotech.builder.food.nuggets.CheeseNuggets;
import org.trishinfotech.builder.food.nuggets.ChickenNuggets;
import org.trishinfotech.builder.meal.FoodItem;
import org.trishinfotech.builder.meal.Meal;

public class OrderBuilder {

    protected static final double HAPPY_MENU_DISCOUNT = 5.0d;

    private String customerName;

    private OrderService service = OrderService.TAKE_AWAY;

    private List<FoodItem> items = new ArrayList<FoodItem>();

    public OrderBuilder() {
        super();
    }
  
    // Сеттеры для каждого поля в целевом объекте. В этом примере это Order.
    // Возвращаемым типом у нас будет сам Builder (например, OrderBuilder), чтобы сделать возможным цепной вызов сеттеров.
    public OrderBuilder name(String customerName) {
        this.customerName = customerName;
        return this;
    }
  
    public OrderBuilder service(OrderService service) {
        if (service != null) {
            this.service = service;
        }
        return this;
    }

    public OrderBuilder item(FoodItem item) {
        items.add(item);
        return this;
    }

    // Комбо предложения 
    public OrderBuilder vegNuggetsHappyMeal() {
        List<FoodItem> foodItems = new ArrayList<FoodItem>();
        foodItems.add(new CheeseNuggets());
        foodItems.add(new Pepsi(BeverageSize.S));
        Meal meal = new Meal("Veg Nuggets Happy Meal", foodItems, HAPPY_MENU_DISCOUNT);
        return item(meal);
    }

    public OrderBuilder chickenNuggetsHappyMeal() {
        List<FoodItem> foodItems = new ArrayList<FoodItem>();
        foodItems.add(new ChickenNuggets());
        foodItems.add(new CocaCola(BeverageSize.S));
        Meal meal = new Meal("Chicken Nuggets Happy Meal", foodItems, HAPPY_MENU_DISCOUNT);
        return item(meal);
    }

    public OrderBuilder vegBurgerHappyMeal() {
        List<FoodItem> foodItems = new ArrayList<FoodItem>();
        foodItems.add(new VegBurger());
        foodItems.add(new Pepsi(BeverageSize.S));
        Meal meal = new Meal("Veg Burger Happy Meal", foodItems, HAPPY_MENU_DISCOUNT);
        return item(meal);
    }

    public OrderBuilder chickenBurgerHappyMeal() {
        List<FoodItem> foodItems = new ArrayList<FoodItem>();
        foodItems.add(new ChickenBurger());
        foodItems.add(new CocaCola(BeverageSize.S));
        Meal meal = new Meal("Chicken Burger Happy Meal", foodItems, HAPPY_MENU_DISCOUNT);
        return item(meal);
    }

    public Order build() {
        Order order = new Order(service, items, customerName);
        if (!validateOrder()) {
            System.out.println("Sorry! Order can't be placed without service type (Take Away/Eat Here) and any food item.");
            return null;
        }
        return order;
    }

    private boolean validateOrder() {
        return (service != null) && !items.isEmpty();
    }
}
```

Готово! Теперь пришло время написать `Main` для выполнения и тестирования результат:

```java
package org.trishinfotech.builder;

import org.trishinfotech.builder.beverages.BeverageSize;
import org.trishinfotech.builder.beverages.cold.CocaCola;
import org.trishinfotech.builder.beverages.cold.Pepsi;
import org.trishinfotech.builder.beverages.hot.HotChocolate;
import org.trishinfotech.builder.food.burger.ChickenBurger;
import org.trishinfotech.builder.food.nuggets.CheeseNuggets;
import org.trishinfotech.builder.food.nuggets.ChickenNuggets;
import org.trishinfotech.builder.util.BillPrinter;

public class Main {

    public static void main(String[] args) {
        OrderBuilder builder1 = new OrderBuilder();
        // you can see the use of chained calls of setters here. No statement terminator
        // till we set all the values of the object
        Order meal1 = builder1.name("Brijesh").service(OrderService.TAKE_AWAY).item(new ChickenBurger())
                .item(new Pepsi(BeverageSize.M)).vegNuggetsHappyMeal().build();
        BillPrinter.printItemisedBill(meal1);

        OrderBuilder builder2 = new OrderBuilder();
        Order meal2 = builder2.name("Micheal").service(OrderService.EAT_HERE).item(new ChickenNuggets())
                .item(new CheeseNuggets()).item(new CocaCola(BeverageSize.L)).chickenBurgerHappyMeal()
                .item(new HotChocolate(BeverageSize.M)).vegBurgerHappyMeal().build();
        BillPrinter.printItemisedBill(meal2);
    }

}
```

 А вот и _результат_ работы программы:

```java

                                                  Food Court
=================================================================================================================
Service:  Take Away (2.00 Tax)                                                         Customer Name: Brijesh             
-----------------------------------------------------------------------------------------------------------------
                Food Item |   Calories |    Packing |      Price |   Packing Price | Discount % | Total Price
-----------------------------------------------------------------------------------------------------------------
           Chicken Burger |        300 |       Wrap |       4.50 |            0.40 |       0.00 |       4.90
                Pepsi (M) |        220 |     Bottle |       2.20 |            0.75 |       0.00 |       2.95
   Veg Nuggets Happy Meal |        490 | Multi-Pack |       5.00 |            2.00 |       5.00 |       6.75
=================================================================================================================
Date: 2020/10/09 20:02:38                                                     Total Bill (incl. taxes): 14.89
Enjoy your meal!

                                                  Food Court
=================================================================================================================
Service:   Eat Here (5.50 Tax)                                                         Customer Name: Micheal             
-----------------------------------------------------------------------------------------------------------------
                Food Item |   Calories |    Packing |      Price |   Packing Price | Discount % | Total Price
-----------------------------------------------------------------------------------------------------------------
          Chicken Nuggets |        450 |  Container |       5.00 |            1.25 |       0.00 |       6.25
           Cheese Nuggets |        330 |  Container |       3.80 |            1.25 |       0.00 |       5.05
            Coca-Cola (L) |        290 |     Bottle |       2.00 |            0.75 |       0.00 |       2.75
Chicken Burger Happy Meal |        450 | Multi-Pack |       5.50 |            1.15 |       5.00 |       6.38
        Hot Chocolate (M) |        450 | Sipper Mug |       2.30 |            1.60 |       0.00 |       3.90
    Veg Burger Happy Meal |        340 | Multi-Pack |       3.90 |            1.15 |       5.00 |       4.86
=================================================================================================================
Date: 2020/10/09 20:02:38                                                     Total Bill (incl. taxes): 30.78
Enjoy your meal!
```

Ну вот и все! Я надеюсь, что этот урок помог освоить паттерн Builder.