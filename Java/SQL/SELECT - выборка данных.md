# Определение
**SELECT** - выбирает данные из таблицы и возвращает их 

Шаблон:
Зеленый цвет - **обязательные**
Черные - **необязательные**
Красные - **доп. информация**.`|{a,b,c}` - список значений через ИЛИ
![[Pasted image 20230625154638.png]]
# Концепции
1. **PROJECTION** - выбор столбцов из таблицы, указывается после ключевого слова **SELECT**. [[DISTINCT - уникальность данных|DISTINCT]]
2. **SELECTION** - выбор строк из таблицы. [[Where - выбор строк|WHERE]]
3. **JOINING** - объединение таблиц
# Expressions in select list - выражение в SELECT
## Expressions для нестроковых: \* / - + ()
Синтаксис:
**SELECT column, expression FROM table**
### Пример
**SELECT salary \* 2 FROM employees;**
**SELECT salary \* 2 + employee_id FROM employees;**
**SELECT salary, salary\*2 FROM employees**
## Expressions для строковых: ||
**(и не только, если тип не строковый - он будет преобразован)**
### Пример
**SELECT 'Работник: '||employee_id||' зарабатывает '||salary FROM employees**
# Alias - псевдоним
**Alias - альтернативное имя для столбца или целого выражения**
Для улучшения читабельности лучше перед **Alias** ставить ключевое слово **as**
## Синтаксис
**SELECT col as Alias, col1 Alias1, col2 "my Alias" FROM table**
Если необходимо Alias с пробелами - необходимо заключить его в кавычки, но лучше юзать нижнее подчеркивание - Count_of_days
## Пример:
SELECT 'My name is '||first_name **name** FROM employees;
Здесь Alias **name** назовёт результирующую таблицу именем **name**
SELECT first_name **name**, last_name **"Фамилия человека"**, employee_salary **salary** FROM employees;
Название колонок результирующей таблицы будут соответственно **name, Фамилия человека, salary**
# Примеры
**Все столбцы указываются с помощью '\*'**
**SELECT * FROM table**
**SELECT country_name, region_id FROM countries / SELECT COUNTRY_NAME FROM COUNTRIES** - демонстрация **PROJECTION**
