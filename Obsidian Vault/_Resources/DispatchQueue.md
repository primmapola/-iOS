[[GCD (Grand Central Dispatch)]]
#многопоточность
`DispatchQueue` в Swift — это часть GCD (Grand Central Dispatch), которая используется для асинхронной и синхронной отправки задач в серийные или параллельные очереди. GCD — это мощный инструмент для многопоточности, который позволяет упростить работу с асинхронными задачами и параллельным выполнением кода.

### Основные Концепции

- **Очереди (Queues)**: Очереди могут быть серийными или параллельными. В серийных очередях задачи выполняются одна за другой, в то время как в параллельных очередях задачи могут выполняться одновременно.

- **Серийная Очередь (Serial Queue)**: Выполняет одну задачу за раз в порядке их добавления. Пример серийной очереди — главная очередь (main queue), которая используется для обновления пользовательского интерфейса.

- **Параллельная Очередь (Concurrent Queue)**: Позволяет выполнять несколько задач одновременно. GCD управляет количеством одновременно выполняемых задач на основе доступных системных ресурсов.

- **Синхронная (Sync) и Асинхронная (Async) отправка**: Синхронная отправка задач блокирует текущий поток до завершения задачи, в то время как асинхронная отправка позволяет текущему потоку продолжать выполнение без ожидания завершения задачи.

### Основная Очередь (Main Queue)

Главная очередь (`DispatchQueue.main`) — это серийная очередь, которая используется для всех обновлений пользовательского интерфейса. Поскольку пользовательский интерфейс должен быть консистентным, все обновления UI должны выполняться в этой очереди.

```swift
DispatchQueue.main.async {
    // Обновление UI
}
```

### Глобальные Параллельные Очереди (Global Concurrent Queues)

Swift предоставляет глобальные параллельные очереди с различными уровнями качества обслуживания (Quality of Service, QoS):

- User Interactive
- User Initiated
- Utility
- Background

```swift
DispatchQueue.global(qos: .background).async {
    // Фоновая задача
}
```

### Создание Собственных Очередей

Вы также можете создавать свои собственные серийные или параллельные очереди:

```swift
let serialQueue = DispatchQueue(label: "com.example.serialQueue")
let concurrentQueue = DispatchQueue(label: "com.example.concurrentQueue", attributes: .concurrent)

serialQueue.async {
    // Задача для серийной очереди
}

concurrentQueue.async {
    // Задача для параллельной очереди
}
```

### Синхронная и Асинхронная Отправка

- **Асинхронная (async)**: Добавляет задачу в очередь и возвращает управление немедленно, позволяя коду после `async` выполняться без ожидания завершения задачи.

- **Синхронная (sync)**: Добавляет задачу в очередь и ждет ее завершения, прежде чем продолжить выполнение следующего кода.

```swift
concurrentQueue.async {
    // Асинхронная задача
}

serialQueue.sync {
    // Синхронная задача
}
```

### Блокировки и Взаимные Блокировки (Deadlocks)

Важно остерегаться взаимных блокировок, особенно при использовании синхронных вызовов. Например, вызов `sync` на главной очереди из главного потока приведет к взаимной блокировке, так как главная очередь будет ждать завершения задачи, которая не может начаться, потому что главный поток уже занят.

### Отложенное Выполнение

С помощью `DispatchQueue` можно также реализовать отложенное выполнение задач:

```swift
let delay = DispatchTimeInterval.seconds(2)
DispatchQueue.main.asyncAfter(deadline: .now() + delay) {
    // Задача, которая выполнится через 2 секунды
}
```

### Заключение

`DispatchQueue` в Swift предлагает мощный и гибкий способ управления многопоточностью. Он позволяет улучшить производительность приложений, разгрузив основной поток от тяжелых задач и эффективно используя системные ресурсы для параллельного выполнения задач.