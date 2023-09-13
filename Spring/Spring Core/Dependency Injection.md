## Что такое Spring Framework?

#### Краткий ответ:

По сути Spring Framework представляет собой просто [контейнер внедрения зависимостей](https://en.wikipedia.org/wiki/Dependency_injection), с несколькими удобными слоями (например: доступ к базе данных, прокси, аспектно-ориентированное программирование, RPC, веб-инфраструктура MVC). Это все позволяет вам быстрее и удобнее создавать Java-приложения.

### Что такое зависимость ? Зачем нам Spring Framework?

Представьте, что вы пишете Java класс, который позволяет вам получить доступ к таблице пользователей в вашей базе данных. Вы бы назвали эти классы DAO (объект доступа к данным) или репозитории. Итак, вы собираетесь написать класс UserDAO.

```java
public class UserDao {

    public User findById(Integer id) {
        // execute a sql query to find the user
    }
}
```

Ваш класс UserDAO имеет только один метод, который позволяет вам находить пользователей в вашей таблице базы данных по их идентификаторам.

1. Вопрос в том, откуда ваш UserDao получает свою информацию об источнике данных? Очевидно, что DAO **_зависит от_** действительного источника данных для запуска этих SQL-запросов.
### Внедрение зависимостей с помощью new()

Наивным решением было бы просто создать новый источник данных с помощью конструктора, каждый раз, когда он вам нужен.

##### Итак, для подключения к базе данных MySQL ваш UserDAO может выглядеть так:

```java
import com.mysql.cj.jdbc.MysqlDataSource;

public class UserDao {

    public User findById(Integer id) {
        MysqlDataSource dataSource = new MysqlDataSource(); // (1)
        dataSource.setURL("jdbc:mysql://localhost:3306/myDatabase");
        dataSource.setUser("root");
        dataSource.setPassword("s3cr3t");

        try (Connection connection = dataSource.getConnection()) { // (2)
             PreparedStatement selectStatement = connection.prepareStatement("select * from users where id =  ?");
             // execute the statement..convert the raw jdbc resultset to a user
             return user;
        }
    }
}
```
##### Итоги
1. Мы хотим подключиться к базе данных MySQL, для чего мы используем MysqlDataSource и задали непосредственно в коде url/username/password для облегчения чтения кода.
2. Мы используем наш недавно созданный источник данных для запроса.

##### Это работает, но давайте посмотрим, что произойдет, когда мы расширим наш класс UserDao другим методом, findByFirstName.

К сожалению, этому методу также нужен источник данных для работы. Мы можем добавить этот новый метод к нашему UserDAO и применить некоторые рефакторинги, введя метод **newDataSource**.

```java
import com.mysql.cj.jdbc.MysqlDataSource;

public class UserDao {

    public User findById(Integer id) {
        try (Connection connection = newDataSource().getConnection()) { // (1)
               PreparedStatement selectStatement = connection.prepareStatement("select * from users where id =  ?");
               // TODO execute the select , handle exceptions, return the user
        }
    }

    public User findByFirstName(String firstName) {
        try (Connection connection = newDataSource().getConnection()) { // (2)
               PreparedStatement selectStatement = connection.prepareStatement("select * from users where first_name =  ?");
               // TODO execute the select ,  handle exceptions, return the user
        }
    }

    public DataSource newDataSource() {
        MysqlDataSource dataSource = new MysqlDataSource(); // (3)
        dataSource.setUser("root");
        dataSource.setPassword("s3cr3t");
        dataSource.setURL("jdbc:mysql://localhost:3306/myDatabase");
        return dataSource;
    }
}
```
##### Итоги
1. findById был переписан для использования нового метода newDataSource().
2. findByFirstName был добавлен и также использует новый метод newDataSource().
3. Это наш недавно извлеченный метод, способный создавать новые источники данных.

Этот подход работает, но имеет два недостатка:

1. Что произойдет, если мы хотим создать новый класс ProductDAO, который также выполняет операторы SQL? Ваш ProductDAO также будет иметь зависимость DataSource, которая теперь доступна только в вашем классе UserDAO. Затем у вас будет другой подобный метод или извлечен вспомогательный класс, содержащий ваш DataSource.
2. Мы создаем совершенно новый источник данных для каждого запроса SQL. Учтите, что DataSource открывает реальное сокет-соединение от вашей Java-программы к вашей базе данных. Это занимает время и довольно дорого. Было бы намного лучше, если бы мы открыли только **один** источник данных и использовали его повторно, вместо того, чтобы открывать и закрывать их тонны. Одним из способов сделать это может быть сохранение источника данных в закрытом поле в нашем UserDao, чтобы его можно было повторно использовать между методами, но это не помогает при дублировании между несколькими DAO.
### Инверсия управления (IoC, Inversion of Control)

Давайте сделаем еще один шаг вперед.

Было бы неплохо, если бы вам в классе UserDAO вообще не приходилось беспокоиться о **поиске зависимостей**? Вместо того, чтобы активно вызывать Application.INSTANCE.dataSource(), ваш UserDAO мог бы (как-то) кричать, что он ему нужен, но больше не контролирует, когда / как / откуда он его получает?

Это то, что называется **_инверсией управления_** (Inversion of Control).

##### Давайте посмотрим, как может выглядеть наш класс UserDAO, с новым конструктором.

```java
import javax.sql.DataSource;

public class UserDao {

    private DataSource dataSource;

    private UserDao(DataSource dataSource) { // (1)
        this.dataSource = dataSource;
    }

    public User findById(Integer id) {
        try (Connection connection = dataSource.getConnection()) { // (2)
               PreparedStatement selectStatement = connection.prepareStatement("select * from users where id =  ?");
               // TODO execute the select etc.
        }
    }

    public User findByFirstName(String firstName) {
        try (Connection connection = dataSource.getConnection()) { // (2)
               PreparedStatement selectStatement = connection.prepareStatement("select * from users where first_name =  ?");
               // TODO execute the select etc.
        }
    }
}
```
##### Итоги
1. Всякий раз, когда вызывающий создает новый UserDao через свой конструктор, вызывающий также должен передать действительный источник данных.
2. Методы findByX будут просто использовать этот источник данных.

С точки зрения **UserDao** это выглядит намного лучше. **Он больше не знает ни о классе приложения, ни о том, как создавать сами источники данных.** **Он только объявляет миру, что «если вы хотите создать (то есть использовать) меня, вам нужно дать мне источник данных»**.

Но представьте, что вы хотите запустить свое приложение. Если раньше вы могли вызывать «new UserService()», то теперь вам нужно обязательно вызвать новый UserDao(dataSource).

```java
public class MyApplication {

    public static void main(String[] args) {
        UserDao userDao = new UserDao(Application.INSTANCE.dataSource());
        User user1 = userDao.findById(1);
        User user2 = userDao.findById(2);
        // etc ...
    }
}
```

##### Контейнеры для внедрения зависимостей

Следовательно, **проблема в том, что вы, как программист, все еще создаете UserDAO с помощью их конструктора** и, таким образом, устанавливаете зависимость DataSource вручную.

Разве не было бы хорошо, если бы **кто-то** знал, что **ваш UserDAO имеет зависимость от конструктора DataSource, и знал, как ее создать?** А затем волшебным образом **сконструирует** для вас **оба** объекта: работающий DataSource и работающий UserDao?

**Этот кто-то является контейнером внедрения зависимостей и является именно тем, что представляет собой среда Spring.**

## Контейнер Spring IOC / Dependency Injection

Как уже упоминалось в самом начале, **Spring Framework по своей сути является контейнером внедрения зависимостей**, который управляет написанными вами классами и их зависимостями для вас (см. Предыдущий раздел). Давайте выясним, как это происходит.