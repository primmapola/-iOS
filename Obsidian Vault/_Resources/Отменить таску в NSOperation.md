Чтобы отменить задачу в `NSOperation`, можно использовать метод `cancel`. Этот метод помечает операцию как отмененную. Важно отметить, что вызов `cancel` не прекращает выполнение операции, если она уже запущена; он лишь устанавливает флаг `isCancelled` в `true`. Это значит, что код внутри операции должен периодически проверять этот флаг и корректно реагировать на его изменение, прекращая выполнение операции.

Вот как можно отменить операцию:

```swift
let operation = BlockOperation {
    for i in 1...10 {
        guard !operation.isCancelled else {
            print("Операция отменена")
            return
        }
        print(i)
        // Имитация длительной задачи
        Thread.sleep(forTimeInterval: 1)
    }
}

let operationQueue = OperationQueue()
operationQueue.addOperation(operation)

// Отмена операции через 2 секунды
DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    operation.cancel()
}
```

В этом примере создается `BlockOperation`, который в цикле выполняет задачу, имитирующую длительную операцию. Внутри цикла происходит проверка `operation.isCancelled`. Если операция отменена (например, в результате вызова `operation.cancel()`), выполнение операции прекращается. Отмена операции инициируется через 2 секунды после её запуска с помощью `DispatchQueue.main.asyncAfter`.

Таким образом, чтобы корректно обрабатывать отмену операции, необходимо регулярно проверять свойство `isCancelled` внутри тела операции и прерывать её выполнение, если это свойство становится `true`.