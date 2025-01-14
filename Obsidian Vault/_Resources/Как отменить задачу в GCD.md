Grand Central Dispatch (GCD) в iOS используется для управления параллельными задачами в многопоточной среде. Отмена задачи в GCD напрямую не поддерживается, так как отправленная на исполнение задача не может быть прервана. Однако, вы можете использовать `DispatchWorkItem` для создания отменяемой задачи.

`DispatchWorkItem` позволяет вам инкапсулировать задачу и управлять её выполнением. Вы можете проверять, была ли задача отменена, и соответственно реагировать в коде выполнения. Вот как это можно сделать:

### Создание и отмена `DispatchWorkItem`

1. **Создание `DispatchWorkItem`**:

```swift
var workItem: DispatchWorkItem?

workItem = DispatchWorkItem {
    // Проверка, не была ли задача отменена
    guard !workItem.isCancelled else {
        print("Задача была отменена")
        return
    }

    // Делаем что-то длительное и интенсивное
    print("Выполнение задачи")
}

// Проверка на nil, т.к. workItem опциональный
if let workItem = workItem {
    DispatchQueue.global().async(execute: workItem)
}
```

2. **Отмена `DispatchWorkItem`**:

Вы можете отменить `workItem`, вызвав метод `cancel()`:

```swift
workItem?.cancel()
```

### Важные моменты

- Отмена `DispatchWorkItem` не останавливает уже выполняющуюся задачу. Она устанавливает флаг `isCancelled` в `true`, и ваш код внутри `DispatchWorkItem` должен регулярно проверять этот флаг, чтобы корректно отреагировать на отмену.
- Это эффективный способ управлять выполнением задач, которые могут быть отменены, особенно когда задачи выполняются асинхронно и могут занимать значительное время.

Использование `DispatchWorkItem` дает вам гибкость в управлении асинхронными задачами, позволяя эффективно реагировать на изменения условий выполнения и требования к отмене задач.