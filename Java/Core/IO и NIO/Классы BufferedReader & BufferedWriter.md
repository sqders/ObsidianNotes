# Определение
При работе этих классов происходит буферизация потока, что ускоряет работу с потоком. Загружает часть файла, затем воспроизводя его, пока оставшаяся часть продолжает загружаться, эффективность увеличивается из-за того, что мы меньшее количество раз обращаемся к файлу. Более эффективны, чем [[Классы FileWriter & FileReader]], принимают в конструкторе именно эти классы
## BufferedReader
![[Pasted image 20230610101615.png]]
## BufferedWriter
![[Pasted image 20230610101610.png]]
# Особенности и использование
# Методы
1.`read() / write()`
2. `readLine() / writeLine()`
# Примеры
![[Pasted image 20230610102025.png]]
Копирует содержимое файла **test2** в **test3**