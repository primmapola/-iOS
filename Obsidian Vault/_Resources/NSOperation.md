`NSOperation` и `NSOperationQueue` — это мощные абстракции в iOS, предоставляемые Foundation framework, которые позволяют управлять параллельными или последовательными задачами в многопоточной среде. Вот основные моменты, касающиеся `NSOperation`:

1. **Абстракция задачи:** `NSOperation` — это абстрактный класс, который представляет собой одиночную задачу. Чтобы использовать его, обычно создается подкласс `NSOperation` или используются его конкретные подклассы, такие как `BlockOperation` или `InvocationOperation`.

2. **Выполнение операции:** Операция может быть выполнена синхронно или асинхронно. Вызов метода `start()` начинает выполнение операции немедленно и в текущем потоке, что чаще всего не желательно. Вместо этого, операции обычно добавляются в `NSOperationQueue`, который управляет выполнением операций на разных потоках.

3. **Управление зависимостями:** Операции могут быть настроены таким образом, чтобы одна операция не начиналась до тех пор, пока одна или несколько других операций не будут завершены. Это позволяет создавать сложные зависимости между задачами.

4. **Отмена операции:** `NSOperation` поддерживает отмену, что позволяет прервать выполнение операции, если это ещё возможно.

5. **Состояние операции:** У `NSOperation` есть несколько свойств, позволяющих отслеживать состояние выполнения, такие как `isExecuting`, `isFinished` и `isCancelled`.

6. **Приоритеты:** Операции могут иметь различные приоритеты выполнения. Это не гарантирует немедленное выполнение операции с высоким приоритетом, но указывает планировщику `NSOperationQueue` предпочтение при выборе следующей операции для запуска.

Пример использования `NSOperation` и `NSOperationQueue`:

```swift
let operationQueue = OperationQueue()

let operation1 = BlockOperation {
    // Код первой задачи
}

let operation2 = BlockOperation {
    // Код второй задачи
}

operation2.addDependency(operation1) // operation2 не начнется, пока не завершится operation1

operationQueue.addOperation(operation1)
operationQueue.addOperation(operation2)
```

В этом примере создаются две операции, где `operation2` зависит от `operation1`. Обе операции добавляются в очередь, что позволяет управлять их выполнением асинхронно.