#Aрхитектуры

Singleton — это порождающий паттерн проектирования, который гарантирует, что у класса есть только один экземпляр, и предоставляет к нему глобальную точку доступа.

### Singleton вне ARC (Manual Reference Counting, MRC):

При использовании Manual Reference Counting (которое было актуально в ранних версиях Objective-C до внедрения ARC в iOS 5), создание синглтона требовало управления памятью на более низком уровне:

```objc
+ (instancetype)sharedInstance {
    static dispatch_once_t onceToken;
    static MySingleton *sharedInstance = nil;

    dispatch_once(&onceToken, ^{
        sharedInstance = [[super allocWithZone:NULL] init];
    });

    return sharedInstance;
}

+ (id)allocWithZone:(struct _NSZone *)zone {
    return [self sharedInstance];
}

- (id)copyWithZone:(NSZone *)zone {
    return self;
}
```

В коде выше используется `dispatch_once` для обеспечения того, что инициализация экземпляра происходит единожды.

### Singleton с использованием ARC:

С внедрением ARC управление памятью стало проще, но синглтон по-прежнему использует `dispatch_once` для того, чтобы гарантировать единственность экземпляра:

```swift
class MySingleton {
    static let sharedInstance = MySingleton()

    private init() {
        // Закрытый инициализатор, чтобы предотвратить создание больше одного экземпляра
    }
}
```

В этом подходе для Swift используется статическая константа, которая инициализируется один раз. Конструктор класса (`init`) сделан приватным, чтобы предотвратить создание дополнительных экземпляров.

Обратите внимание, что с внедрением ARC вам больше не нужно явно управлять памятью, как это было в случае с MRC. ARC автоматически следит за ссылками и освобождает память, когда она больше не используется.