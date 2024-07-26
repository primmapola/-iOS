#iOS_Platform
В Swift `String` - это структура, которая предоставляет множество методов для работы со строками. Также `String` поддерживает множество протоколов, что делает работу со строками гибкой и мощной. Давайте рассмотрим основные методы и протоколы, связанные со `String`:

### Основные методы `String`:

1. **Инициализация**:
    ```swift
    let stringA = String() // пустая строка
    let stringB = String(repeating: "a", count: 3) // "aaa"
    ```

2. **Добавление/удаление символов**:
    ```swift
    var hello = "Hello"
    hello.append(", World!") // "Hello, World!"
    ```

3. **Подстроки**:
    ```swift
    let greeting = "Hello, World!"
    let index = greeting.firstIndex(of: ",")!
    let substring = greeting[..<index] // "Hello"
    ```

4. **Манипуляции с регистром**:
    ```swift
    let lowercased = greeting.lowercased() // "hello, world!"
    let uppercased = greeting.uppercased() // "HELLO, WORLD!"
    ```

5. **Работа с префиксами/суффиксами**:
    ```swift
    let hasPrefix = greeting.hasPrefix("Hello") // true
    let hasSuffix = greeting.hasSuffix("World!") // true
    ```

6. **Разделение строки**:
    ```swift
    let words = greeting.split(separator: " ") // ["Hello,", "World!"]
    ```

7. **Поиск**:
    ```swift
    let containsWorld = greeting.contains("World") // true
    ```

### Протоколы, которые поддерживает `String`:

1. **Comparable**: Этот протокол позволяет сравнивать строки с помощью операторов `<`, `>`, `<=`, `>=`.

2. **Equatable**: Благодаря этому протоколу можно сравнивать строки на равенство с помощью оператора `==`.

3. **Collection**: Строки могут быть проитерированы как коллекции символов.
    ```swift
    for char in "Hello" {
        print(char)
    }
    ```

4. **CustomStringConvertible**: Это позволяет преобразовать объекты в строку, особенно полезно для вывода информации для отладки.

5. **ExpressibleByStringLiteral**: Позволяет создавать строки с помощью литеральной нотации.

6. **LosslessStringConvertible**: Позволяет преобразовать объекты в строку и обратно без потерь.

Это только обзор основных методов и протоколов, связанных со `String` в Swift. Полный список методов и протоколов, а также дополнительная информация, доступна в официальной документации Apple по Swift.