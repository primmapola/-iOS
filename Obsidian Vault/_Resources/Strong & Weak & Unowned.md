#многопоточность
ARC, или Automatic Reference Counting, является механизмом управления памятью в Swift и Objective-C. Он автоматически отслеживает и управляет памятью, используемой объектами вашего приложения. Когда объект больше не используется (то есть к нему нет активных ссылок), ARC освобождает память, выделенную для этого объекта, делая её доступной для других целей.

### Ключевые Понятия ARC:

1. **Strong Reference**: Это обычная ссылка, которая увеличивает счетчик ссылок на объект. Пока существует хотя бы одна сильная (strong) ссылка, объект не будет уничтожен ARC. Большинство ссылок в Swift являются сильными по умолчанию.

2. **Weak Reference**: Слабая ссылка позволяет ссылаться на объект, не увеличивая его счетчик ссылок. Это означает, что объект может быть уничтожен, даже если на него есть слабые ссылки. Слабые ссылки автоматически обнуляются ARC, когда объект, на который они указывают, удаляется. Это полезно для предотвращения ситуаций с циклическими ссылками. Слабые ссылки обычно используются для опциональных свойств или когда ссылка может быть `nil`.

3. **Unowned Reference**: Неуправляемая ссылка похожа на слабую в том, что она не увеличивает счетчик ссылок объекта. Однако в отличие от слабой ссылки, неуправляемая ссылка не обнуляется автоматически, когда объект удаляется. Это означает, что доступ к неуправляемой ссылке после уничтожения объекта приведет к ошибке времени выполнения. Неуправляемые ссылки используются, когда вы уверены, что ссылка всегда будет указывать на объект в течение её срока жизни.

### Примеры:

- **Strong Reference**: Используется по умолчанию для большинства свойств объекта.
  ```swift
  class MyClass {
      var property: AnotherClass // strong reference by default
  }
  ```

- **Weak Reference**: Используется, когда необходимо избежать циклических ссылок.
  ```swift
  class Element {
      weak var parent: Container? // weak reference to avoid retain cycle
  }
  ```

- **Unowned Reference**: Используется, когда уверены в существовании объекта.
  ```swift
  class Element {
      unowned var parent: Container // unowned reference
  }
  ```

### Важные Замечания:

- Циклические ссылки могут возникнуть, когда два объекта сильно ссылается друг на друга. Это приводит к тому, что ARC не может освободить память, так как счетчик ссылок не уменьшается до нуля.
- Использование слабых и неуправляемых ссылок помогает разрешать циклические ссылки и управлять памятью эффективно.

ARC значительно упрощает управление памятью в Swift, но понимание различных типов ссылок и их правильное использование остаётся важным аспектом разработки на Swift.