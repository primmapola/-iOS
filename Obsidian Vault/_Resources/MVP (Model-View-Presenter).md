[[MVP (Model-View-Presenter)]]
#Aрхитектуры 

MVP (Model-View-Presenter) и MVC (Model-View-Controller) являются архитектурными паттернами, которые помогают организовать код в приложении, делая его более чистым, модульным и тестируемым. Оба этих паттерна имеют свои преимущества и недостатки, особенно в контексте разработки на iOS.

1. **Структура:**

   - **MVP:** В MVP, Model представляет собой данные приложения, View отображает эти данные, а Presenter является связующим звеном между Model и View. Presenter отвечает за обработку всей бизнес-логики и обновление View.
   - **MVC:** В MVC, Controller выполняет роль связующего звена между Model и View. ***Однако, в отличие от MVP, View может иметь прямой доступ к Model, что может создать проблемы с зависимостями.***

2. **Преимущества MVP:**

   - **Тестируемость:** MVP упрощает тестирование, так как Presenter не имеет прямого доступа к UI, что облегчает написание юнит-тестов.
   - **Разделение ответственности:** MVP обеспечивает четкое разделение ответственности между слоями, что упрощает понимание и поддержку кода.
   - **Повторное использование кода:** Код в Presenter может быть повторно использован, так как он не зависит от UI.

3. **Недостатки MVP:**

   - **Сложность:** MVP может увеличить сложность проекта из-за необходимости создавать дополнительные классы Presenter.
   - **Больше кода:** Может потребовать больше кода по сравнению с MVC, особенно в больших проектах.

4. **Различия с MVC:**

   - **Разделение ответственности:** В MVP бизнес-логика полностью отделена от UI, в то время как в MVC Controller может иметь прямой доступ к View и Model.
   - **Тестируемость:** MVP упрощает написание тестов за счет более четкого разделения ответственности и отсутствия прямого доступа к UI в Presenter.

Разработчики iOS могут выбирать между MVP и MVC в зависимости от требований проекта и личных предпочтений. MVP может быть предпочтительным для проектов, где важна высокая тестируемость и чистота кода, в то время как MVC может быть проще и быстрее для разработки в небольших проектах или прототипах.

Рассмотрим простой пример создания экрана с использованием архитектурного паттерна MVP в iOS:

### 1. Model:
Создадим структуру `User`, которая будет представлять пользователя в нашем приложении.

```swift
struct User {
    var name: String
    var age: Int
}
```

### 2. View:
Создадим протокол `UserViewProtocol`, который будет определять методы для обновления представления.

```swift
protocol UserViewProtocol: AnyObject {
    func setName(_ name: String)
    func setAge(_ age: String)
}
```

Также создадим класс `UserViewController`, который будет реализовывать этот протокол:

```swift
class UserViewController: UIViewController, UserViewProtocol {
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var ageLabel: UILabel!
    
    var presenter: UserPresenterProtocol!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        presenter.viewDidLoad()
    }
    
    func setName(_ name: String) {
        nameLabel.text = name
    }
    
    func setAge(_ age: String) {
        ageLabel.text = age
    }
}
```

### 3. Presenter:
Создадим протокол `UserPresenterProtocol`, который будет определять методы для взаимодействия с представлением и моделью.

```swift
protocol UserPresenterProtocol: AnyObject {
    func viewDidLoad()
}
```

Также создадим класс `UserPresenter`, который будет реализовывать этот протокол:

```swift
class UserPresenter: UserPresenterProtocol {
    weak var view: UserViewProtocol?
    var user: User
    
    init(view: UserViewProtocol, user: User) {
        self.view = view
        self.user = user
    }
    
    func viewDidLoad() {
        view?.setName(user.name)
        view?.setAge("\(user.age)")
    }
}
```

Теперь, когда `UserViewController` загружается, `presenter` уведомляется об этом и обновляет `UserViewController` с информацией из модели `User`. Это позволяет нам отделить бизнес-логику от UI-логики, что делает код более чистым и тестируемым.