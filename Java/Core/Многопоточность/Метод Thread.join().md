# Определение
Данный метод говорит текущему потоку, что он должен **дождаться завершения указанного потока** перед тем, как продолжить свою работу. Переводит процесс в состояние *WAITING*.
## Почему join()?
![[Pasted image 20230629063637.png]]
На картинке в программе был создан новый поток, main-поток раздвоился и далее мы подали команду **join()** - main-поток дождался выполнения 2 процесса и, получив ответ, процессы "склеились" вместе в main-процесс.
# Зачем необходим
Для ожидания полного выполнения работы другого потока, перед продолжением работы текущего
# Пример
![[Pasted image 20230603085654.png]]
В данном примере **основной поток `main`** ожидает, когда завершат свою работу 1 и 2 поток и только потом выводит `counter`.