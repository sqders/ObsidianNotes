# Определение
**Класс `CountDownLatch` потокобезопасен.**
Дословно с английского - *защёлка обратного отсчёта*. Это класс, который необходим для обратного отсчёта, который осуществляется методом `.countDown()`, количество вызовов метода для возврата `await()` указывается в конструкторе. Имеет метод **`await()`**, который останавливает выполнение текущего потока на месте его вызова и ждёт "открытия защёлки", т.е. когда количество вызванных `.countDown()` станет равно числу, переданному в конструкторе. После продолжает выполнение потока.
# Зачем необходим?
Для выполнение всего спектра заранее известных операций, допустим, при регистрации пользователя, нам необходимо:
1) Принять данные с формы
2) Провалидировать данные с формы
3) Преобразовать данные в объекты
4) Загрузить данные в БД
Данные операции могут занять много времени и при синхронном подходе могут занять много времени => мы выделяем их в отдельных поток и заводим `CountDownLatch` со счётчиком операций = 3 и только после первых трех пунктов, может быть выполнен 4 - конечный. Тем самым, мы добиваемся четкого выполнения всех необходимых инструкций перед финальным действием.
# Пример
![[Pasted image 20230604111737.png]]
Устанавливаем отсчёт на 3 операции, которые будут исполняться в 3х потоках. Когда будут выполнены все 3 операции, главный поток сможет продолжить своё выполнение.

#Многопоточность #потокобезопасный 