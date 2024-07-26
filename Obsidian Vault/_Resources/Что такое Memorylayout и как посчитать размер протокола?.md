`MemoryLayout` - `enum`, с помощью которого можно узнать информацию о размере, выравнивании и шаге какого-либо типа данных:

```kotlin
@frozen
public enum MemoryLayout<T> {
  /// The contiguous memory footprint of `T`, in bytes.
  ///
  /// A type's size does not include any dynamically allocated or out of line
  /// storage. In particular, `MemoryLayout<T>.size`, when `T` is a class
  /// type, is the same regardless of how many stored properties `T` has.
  ///
  /// When allocating memory for multiple instances of `T` using an unsafe
  /// pointer, use a multiple of the type's stride instead of its size.
  @_transparent
  public static var size: Int {
    return Int(Builtin.sizeof(T.self))
  }

  /// The number of bytes from the start of one instance of `T` to the start of
  /// the next when stored in contiguous memory or in an `Array<T>`.
  ///
  /// This is the same as the number of bytes moved when an `UnsafePointer<T>`
  /// instance is incremented. `T` may have a lower minimal alignment that
  /// trades runtime performance for space efficiency. This value is always
  /// positive.
  @_transparent
  public static var stride: Int {
    return Int(Builtin.strideof(T.self))
  }

  /// The default memory alignment of `T`, in bytes.
  ///
  /// Use the `alignment` property for a type when allocating memory using an
  /// unsafe pointer. This value is always positive.
  @_transparent
  public static var alignment: Int {
    return Int(Builtin.alignof(T.self))
  }
}
```

Рассмотрим пустой протокол `P`:

```swift
protocol P { }
print(MemoryLayout<P>.size) // 40
```

Обычный экзистенциальный контейнер по умолчанию содержит:

- Буфер на три машинных слова для хранимого значения. Имеем на x64: 8 + 8 + 8 = 24
- Ссылка на Метатип — ещё 8
- Ссылка на массив таблиц `Protocol Witness Table` - снова 8 Итого: 24 + 8 + 8 = 40

```scss
print(MemoryLayout<Sendable>.size) // 32
```

`Sendable` это маркер протокол, а значит он не содержит ссылки на массив таблиц Protocol Witness Table. Итого: 40 - 8 = 32

```scss
print(MemoryLayout<AnyObject>.size) // 8
```

`AnyObject` - это специальный вид экзистенциального контейнера `Class Existential Containers`:

- Содержит указатель на память в хипе - это 8
- Нет ссылки на массив таблиц Protocol Witness Table Итого: 8

```scss
print(MemoryLayout<Actor>.size) // 16
```

`Actor` наследуется от `AnyObject`, а значит Class Existential Containers:

- Содержит указатель на память в куче - это 8
- Есть ссылка на массив таблиц Protocol Witness Table - ещё 8  
    Итого: 8 + 8 = 16