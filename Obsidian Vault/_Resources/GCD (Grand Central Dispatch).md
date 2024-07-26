[[GCD (Grand Central Dispatch)]]
#многопоточность
GCD (Grand Central Dispatch) — это технология многозадачности, используемая в операционных системах Apple (iOS, macOS и др.). Она предоставляет простой и мощный способ управления асинхронными задачами и многопоточностью.

GCD работает с задачами, которые вы хотите выполнить асинхронно или параллельно, и помещает их в очереди. Основные виды очередей:

- **Основная (main) очередь**: Выполняет задачи в главном потоке, обычно используется для обновления UI.

- **Глобальные (global) очереди**: Системные фоновые очереди, которые могут выполнять задачи параллельно. Разделены на несколько уровней приоритета.

- **Приватные (custom) очереди**: Пользовательские очереди, которые вы создаете. Могут быть последовательными (serial) или параллельными (concurrent).

### Примеры использования GCD:

#### 1. Выполнение задачи асинхронно на фоновом потоке:

```swift
DispatchQueue.global(qos: .background).async {
    // Длительная задача, например, загрузка данных
    let result = fetchData()

    // Возврат в главную очередь для обновления UI
    DispatchQueue.main.async {
        // Обновление UI
        updateUI(with: result)
    }
}
```

#### 2. Задержка выполнения задачи:

```swift
let delayInSeconds = 3.0
DispatchQueue.main.asyncAfter(deadline: .now() + delayInSeconds) {
    // Код, который будет выполнен с задержкой
    print("Задача выполнена с задержкой")
}
```

#### 3. Создание и использование приватной последовательной очереди:

```swift
let serialQueue = DispatchQueue(label: "com.example.mySerialQueue")
serialQueue.async {
    // Выполнение первой задачи
    performFirstTask()
}

serialQueue.async {
    // Выполнение второй задачи, начнется только после завершения первой
    performSecondTask()
}
```

#### 4. Использование группы задач (DispatchGroup) для синхронизации:

```swift
let dispatchGroup = DispatchGroup()

dispatchGroup.enter()
loadFirstResource {
    // Когда первый ресурс загружен
    dispatchGroup.leave()
}

dispatchGroup.enter()
loadSecondResource {
    // Когда второй ресурс загружен
    dispatchGroup.leave()
}

dispatchGroup.notify(queue: DispatchQueue.main) {
    // Код здесь будет выполнен после загрузки обоих ресурсов
    combineResources()
}
```

GCD — мощный инструмент для управления асинхронностью и параллелизмом в приложениях Apple. Он помогает избежать блокировки главного потока, повышает производительность приложения и облегчает работу с многопоточностью.

### [Многопоточность 2023](https://habr.com/ru/articles/742502) 