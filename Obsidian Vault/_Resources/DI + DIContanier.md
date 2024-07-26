#Aрхитектуры 

Внедрение зависимостей — это стиль настройки объекта, при котором поля объекта задаются внешней сущностью. Другими словами, объекты настраиваются внешними объектами. DI — это альтернатива самонастройке объектов.

В Swift Dependency Injection (DI) можно реализовать разными способами. Рассмотрим основные методы DI и как их можно применить в Swift, а также поговорим о DI контейнерах.

### 1. Конструктор (Constructor Injection)

Это наиболее часто используемый способ внедрения зависимостей, когда зависимости передаются через конструктор класса.

```swift
protocol DatabaseServiceProtocol {
    func fetchData()
}

class DatabaseService: DatabaseServiceProtocol {
    func fetchData() {
        // Реализация получения данных
    }
}

class DataManager {
    private let databaseService: DatabaseServiceProtocol

    init(databaseService: DatabaseServiceProtocol) {
        self.databaseService = databaseService
    }

    func loadData() {
        databaseService.fetchData()
    }
}

let databaseService = DatabaseService()
let dataManager = DataManager(databaseService: databaseService)
```

### 2. Сеттер (Setter Injection)

Зависимости передаются через сеттеры или другие методы.

```swift
class Logger {
    func log(message: String) {
        // Логирование сообщения
    }
}

class Application {
    var logger: Logger?

    func setLogger(logger: Logger) {
        self.logger = logger
    }

    func performAction() {
        logger?.log(message: "Action performed")
    }
}

let logger = Logger()
let app = Application()
app.setLogger(logger: logger)
```

### 3. Интерфейс (Interface Injection)

Объект реализует специальный интерфейс, который позволяет внедрить зависимости.

```swift
protocol NetworkServiceInjectable {
    func inject(networkService: NetworkService)
}

class NetworkService {}

class ViewController: NetworkServiceInjectable {
    private var networkService: NetworkService?

    func inject(networkService: NetworkService) {
        self.networkService = networkService
    }

    func fetchData() {
        // Использование networkService для получения данных
    }
}

let viewController = ViewController()
let networkService = NetworkService()
viewController.inject(networkService: networkService)
```

### DI Контейнер

DI контейнер — это объект, который заботится о создании и управлении зависимостями. В Swift можно создать простой DI контейнер, используя словарь или специальный класс, который управляет созданием и предоставлением зависимостей.

```swift
class DIContainer {
    private var services = [String: Any]()

    func register<T>(service: T) {
        let key = String(describing: T.self)
        services[key] = service
    }

    func resolve<T>() -> T? {
        let key = String(describing: T.self)
        return services[key] as? T
    }
}

let container = DIContainer()
container.register(service: DatabaseService() as DatabaseServiceProtocol)

let databaseService = container.resolve() as DatabaseServiceProtocol?
```

Это простой пример контейнера зависимостей. В более сложных проектах можно использовать сторонние библиотеки для DI, такие как Swinject или Typhoon, которые предлагают более продвинутые функции и удобный синтаксис для управления зависимостями.

Конечно, давайте рассмотрим простой пример использования Dependency Injection (DI) и DI контейнера на Swift.

### Шаг 1: Определение Зависимостей

Допустим, у нас есть протокол `NetworkingService`, который описывает функциональность для выполнения сетевых запросов:

```swift
protocol NetworkingService {
    func fetchData(completion: @escaping (Data?) -> Void)
}
```

И у нас есть конкретная реализация этого протокола:

```swift
class RealNetworkingService: NetworkingService {
    func fetchData(completion: @escaping (Data?) -> Void) {
        // Код для выполнения сетевых запросов
        // Предположим, что мы получили данные и вызываем completion
        completion(Data())
    }
}
```

### Шаг 2: Регистрация с Контейнером

Для DI контейнера можно использовать сторонние библиотеки или просто создать свой собственный примитивный контейнер. Давайте определим простой DI контейнер:

```swift
class DIContainer {
    static let shared = DIContainer()

    private var services = [String: Any]()

    func register<T>(service: T) {
        let key = String(describing: T.self)
        services[key] = service
    }

    func resolve<T>() -> T? {
        let key = String(describing: T.self)
        return services[key] as? T
    }
}
```

Теперь регистрируем наш `NetworkingService`:

```swift
DIContainer.shared.register(service: RealNetworkingService() as NetworkingService)
```

### Шаг 3: Использование в Коде

Допустим, у нас есть класс `DataLoader`, который зависит от `NetworkingService`:

```swift
class DataLoader {
    private let networkingService: NetworkingService

    init(networkingService: NetworkingService) {
        self.networkingService = networkingService
    }

    func loadData() {
        networkingService.fetchData { data in
            // Обработка полученных данных
        }
    }
}
```

Теперь мы можем создать экземпляр `DataLoader`, используя DI контейнер для получения зависимости:

```swift
if let networkingService = DIContainer.shared.resolve() as NetworkingService? {
    let dataLoader = DataLoader(networkingService: networkingService)
    dataLoader.loadData()
}
```

### Шаг 4: Гибкость и Масштабируемость

Если позже мы захотим изменить реализацию `NetworkingService` (например, использовать мок для тестирования), нам достаточно будет просто зарегистрировать новую реализацию в DI контейнере. Весь остальной код останется неизменным:

```swift
class MockNetworkingService: NetworkingService {
    func fetchData(completion: @escaping (Data?) -> Void) {
        // Имитация получения данных
        completion(Data())
    }
}

// Перерегистрация сервиса с новой реализацией
DIContainer.shared.register(service: MockNetworkingService() as NetworkingService)
```

### Заключение

В этом примере мы создали простой DI контейнер и использовали его для управления зависимостями в нашем Swift-приложении. Это упрощает тестирование, обеспечивает гибкость и делает код более модульным и удобным для поддержки.