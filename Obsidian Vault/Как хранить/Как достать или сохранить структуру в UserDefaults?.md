Надо что бы структура конформаила Codable - ее можно преобразовать в JSON и обратно, но тк в Foundation нет типа JSON, то структура будет типа Data или NSData?
```Swift
struct MyStruct: Codable {
    var property1: String
    var property2: Int
    // Добавь другие свойства здесь
}

// Сохранение структуры
let myStruct = MyStruct(property1: "value1", property2: 2)
let userDefaults = UserDefaults.standard

if let encoded = try? JSONEncoder().encode(myStruct) {
    userDefaults.set(encoded, forKey: "myStructKey")
}

// Получение структуры
if let savedData = userDefaults.data(forKey: "myStructKey"),
   let loadedStruct = try? JSONDecoder().decode(MyStruct.self, from: savedData) {
    // теперь loadedStruct содержит твою структуру
}

```
