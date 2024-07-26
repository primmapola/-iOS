В Swift, утверждения передачи управления (Control Transfer Statements) позволяют изменять порядок выполнения кода в программе, прерывая ход обычного выполнения и передавая управление другим частям кода. Вот пять основных утверждений передачи управления в Swift и описание того, как их использовать:

1. **Break**
   - **Использование**: Прерывает выполнение текущего цикла (`for`, `while`) или `switch`-выражения.
   - **Пример**:
     ```swift
     for number in 1...10 {
         if number == 5 {
             break  // Прекращает цикл, когда number достигает 5
         }
         print(number)
     }
     ```

2. **Continue**
   - **Использование**: Пропускает текущую итерацию цикла и переходит к следующей итерации.
   - **Пример**:
     ```swift
     for number in 1...10 {
         if number % 2 == 0 {
             continue  // Пропускает четные числа
         }
         print(number)  // Печатает только нечетные числа
     }
     ```

3. **Fallthrough**
   - **Использование**: В `switch`-выражениях, `fallthrough` используется для продолжения выполнения следующего `case` блока без проверки его условия.
   - **Пример**:
     ```swift
     let integerToDescribe = 5
     var description = "Число \(integerToDescribe) является"
     switch integerToDescribe {
     case 2, 3, 5, 7, 11, 13, 17, 19:
         description += " простым и"
         fallthrough
     default:
         description += " целым."
     }
     print(description)  // "Число 5 является простым и целым."
     ```

4. **Return**
   - **Использование**: Завершает выполнение текущей функции и возвращает управление в место, откуда она была вызвана, при необходимости передавая значение.
   - **Пример**:
     ```swift
     func maybeDouble(_ value: Int, shouldDouble: Bool) -> Int {
         if shouldDouble {
             return value * 2
         }
         return value
     }
     print(maybeDouble(4, shouldDouble: true))  // Выведет 8
     ```

5. **Throw**
   - **Использование**: Используется для сигнализации о том, что в функции произошла ошибка, которая должна быть перехвачена и обработана с помощью механизма обработки ошибок.
   - **Пример**:
     ```swift
     enum PrinterError: Error {
         case outOfPaper
         case noToner
         case onFire
     }

     func printPage(_ text: String) throws {
         let hasPaper = false
         guard hasPaper else {
             throw PrinterError.outOfPaper
         }
         print(text)
     }
     
     do {
         try printPage("Hello, world!")
     } catch PrinterError.outOfPaper {
         print("Ошибка: закончилась бумага.")
     } catch {
         print("Неизвестная ошибка.")
     }
     ```

Эти утверждения передачи управления являются фундаментальными для управления потоком выполнения в Swift, позволяя создавать более гибкий и мощный код.