[[Operation and OperationQueue]]
#многопоточность

`Operation` и `OperationQueue` в iOS представляют собой альтернативный подход к управлению многопоточностью и асинхронными задачами в сравнении с GCD (Grand Central Dispatch). Эти классы являются частью фреймворка Foundation и предлагают более высокоуровневый и объектно-ориентированный способ для работы с асинхронными операциями.

### Operation

`Operation` – это абстрактный класс, который представляет собой одноразовую задачу, которую можно выполнить. Он предоставляет возможности управления зависимостями, приоритетами, состоянием выполнения и отмены. Чтобы использовать `Operation`, обычно создается подкласс `Operation` или используется один из его встроенных подклассов, таких как `BlockOperation`.

### OperationQueue

`OperationQueue` управляет выполнением одной или нескольких операций (`Operation`). Очередь может работать как в последовательном, так и в параллельном режиме, и она берет на себя всю работу по распределению задач по разным потокам. `OperationQueue` также позволяет легко управлять приоритетами и зависимостями между операциями.

### Примеры:

#### 1. Создание простой операции:

```swift
let operation = BlockOperation {
    // Код, который нужно выполнить
    print("Операция выполнена")
}

// Добавление операции в очередь для асинхронного выполнения
OperationQueue().addOperation(operation)
```

#### 2. Установка зависимостей между операциями:

```swift
let operation1 = BlockOperation {
    print("Операция 1 выполнена")
}

let operation2 = BlockOperation {
    print("Операция 2 выполнена")
}

// Устанавливаем, что operation2 не начнется, пока не завершится operation1
operation2.addDependency(operation1)

let queue = OperationQueue()
queue.addOperations([operation1, operation2], waitUntilFinished: false)
```

#### 3. Отмена операции:

```swift
let operation = BlockOperation {
    for i in 1...5 {
        if operation.isCancelled {
            break
        }
        print("Итерация \(i)")
        sleep(1) // Имитация длительной задачи
    }
}

let queue = OperationQueue()
queue.addOperation(operation)

// Отмена операции через 2 секунды
DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    operation.cancel()
}
```

### Заключение:

`Operation` и `OperationQueue` предлагают мощный и гибкий способ управления асинхронными задачами с возможностью управления зависимостями, приоритетами и состоянием выполнения. Эти классы являются отличным выбором, когда необходим более сложный и управляемый подход к многопоточности, по сравнению с более низкоуровневым GCD.