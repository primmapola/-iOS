#iOS_Platform
### Что такое Generic

<mark class="hltr-yellow">Generic код</mark> - дает возможность создавать функции и типы данных, которые могут работать с любым типом.

Например, такие типы как `Array`, `Set` и `Dictionary` используют дженерики для хранения элементов.

<mark class="hltr-red">Так же в Swift есть `Any` и `AnyObject` - они тоже могут расширить функционал, но при этом отследить соответствие типов до компиляции не получится -> это делает код ненадежным.</mark>

### Пример

Допустим нам надо создать массивы для String и Int и вывести их содержимое
```Swift
let numbers: [Int] = [1,2,3,4]
let strings: [Strings] = ["asd","aaa"]

func printNumbers(nums: [Int]) {
	print(nums)
}

func printStrings(strs: [Strings]) {
	print(strs)
}
```

Функции отличаются только параметрами входа -> можно использовать Generic для избежания дублирования.

Generic function
```Swift
func print<T>(array: [T]) {
	print(array.map {$0})
}
```


### Generic типы
```Swift
var genericType: [T] = [] // Пустой массив различных элементов
```

