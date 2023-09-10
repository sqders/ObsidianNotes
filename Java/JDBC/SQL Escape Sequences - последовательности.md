# Определение
Java поддерживает специальные экранирующие символы, собственно, символ `\`, а также для работы с БД через `JDBC` поддерживает и другие интересные "помечающие" знаки и обороты, они называются **`Escape Sequences`**
[SQL sequences JDBC - Oracle docs](https://docs.oracle.com/cd/E13157_01/wlevs/docs30/jdbc_drivers/sqlescape.html):
JDBC defines escape sequences that contain the standard syntax for the following language features:

- Date, time, and timestamp literals
- Scalar functions such as numeric, string, and data type conversion functions
- Outer joins
- Escape characters for wildcards used in LIKE clauses
- Procedure calls

The escape sequence used by JDBC is:

   {extension}

The escape sequence is recognized and parsed by the Type 4 JDBC drivers, which replace the escape sequences with data store-specific grammar.
# Использование
## Date, Time, Timestamp
![[Pasted image 20230623100059.png]]
## Scalar Functions
![[Pasted image 20230623100115.png]]
## Outer joins
![[Pasted image 20230623100137.png]]
## LIKE for Wildcards
![[Pasted image 20230623100159.png]]
## [[Хранимые процедуры (Procedure Call)|Procedure Call - Хранимые процедуры]]
Это уже занесённые в БД процедуры, которые мы можем вызывать и исполнять. **Полезно**.
![[Pasted image 20230623100216.png]]