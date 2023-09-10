# Определение
**BLOB** - Binary large objects, **CLOB** - character large objects
Это просто большие объекты, которые мы сохраняем в БД
# Использование
Чтобы загрузить файл в БД, нужно загрузить его содержимое в Blob объект Java с помощью [[Streams введение|Stream-ов]] или [[Введение в Buffer & Channel|Buffer и Channel]] 
Если это изображение, можно юзать статический класс `ImageIO`:
![[Pasted image 20230623095143.png]]