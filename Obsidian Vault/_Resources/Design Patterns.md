Шаблоны проектирования — это стандартные решения общих проблем, возникающих при проектировании программного обеспечения. Они не являются готовыми к использованию кодами, а скорее представляют собой описания или шаблоны для решения задачи в определенных условиях. Шаблоны помогают разработчикам строить гибкие и масштабируемые системы, уменьшая связанность компонентов и увеличивая их переиспользуемость.

Шаблоны проектирования обычно делятся на три основные категории:

### 1. Порождающие (Creational Patterns)
Эти шаблоны обеспечивают гибкость при создании объектов, скрывая точные классы создаваемых объектов и способ их создания.

- **Abstract Factory (Абстрактная Фабрика)**: Предоставляет интерфейс для создания семейств связанных или зависимых объектов без указания их конкретных классов.
- **Builder (Строитель)**: Позволяет создавать сложные объекты пошагово.
- **Factory Method (Фабричный Метод)**: Определяет интерфейс для создания объекта, но позволяет подклассам изменять тип создаваемых объектов.
- **Prototype (Прототип)**: Позволяет копировать объекты без привязки к их конкретным классам.
- **Singleton (Одиночка)**: Гарантирует, что класс имеет только один экземпляр, и предоставляет глобальную точку доступа к этому экземпляру.

### 2. Структурные (Structural Patterns)
Эти шаблоны описывают способы построения объектов и классов в более крупные структуры, сохраняя при этом гибкость и эффективность структур.

- **Adapter (Адаптер)**: Позволяет объектам с несовместимыми интерфейсами работать вместе.
- **Bridge (Мост)**: Разделяет абстракцию и реализацию так, чтобы они могли изменяться независимо.
- **Composite (Компоновщик)**: Позволяет клиентам обращаться к отдельным объектам и к их композициям одинаково.
- **Decorator (Декоратор)**: Динамически добавляет новые обязанности объектам без изменения их реализации.
- **Facade (Фасад)**: Предоставляет упрощенный интерфейс к сложной системе классов, библиотеке или фреймворку.
- **Flyweight (Приспособленец)**: Позволяет уменьшить затраты на создание большого количества похожих объектов.
- **Proxy (Заместитель)**: Предоставляет объект-заместитель, контролирующий доступ к другому объекту.

### 3. Поведенческие (Behavioral Patterns)
Эти шаблоны относятся к взаимодействиям и обязанностям между объектами, помогая уменьшить связанность и делая взаимодействие между объектами более гибким.

- **Chain of Responsibility (Цепочка Обязанностей)**: Позволяет передавать запросы последовательно по цепочке обработчиков.
- **Command (Команда)**: Превращает запросы в объекты, позволяя передавать их как аргументы, а также поддерживать операции отмены.
- **Interpreter (Интерпретатор)**: Определяет грамматическое представление для языка и интерпретатор для его обработки.
- **Iterator (Итератор)**: Предоставляет способ последовательного доступа к элементам коллекции без раскрытия ее внутреннего представления.
- **Mediator (Посредник)**: Определяет объект, который инкапсулирует взаимодействие множества объектов.
- **Memento (Снимок)**: Позволяет сохранять и восстанавливать предыдущее состояние объекта, не раскрывая его внутренних деталей.
- **Observer (Наблюдатель)**: Позволяет объектам оповещать другие объекты об изменениях в своем состоянии.
- **State (Состояние)**: Позволяет объектам изменять свое поведение в зависимости от своего состояния.
- **Strategy (Стратегия)**: Определяет семейство алгоритмов, инкапсулирует каждый из них и обеспечивает их взаимозаменяемость.
- **Template Method (Шаблонный Метод)**: Определяет скелет алгоритма в методе, оставляя определение точных шагов алгоритма подклассам.
- **Visitor (Посетитель)**: Позволяет добавлять новые операции к объектам без изменения их классов.

Каждый из этих шаблонов имеет свое применение в зависимости от конкретных задач и контекста разработки. Они помогают разработчикам строить более чистые, понятные и масштабируемые архитектуры