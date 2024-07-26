Для увеличения области нажатия UIButton программно в Swift можно воспользоваться следующим подходом:

1. Создайте индивидуально настроенный класс UIButton и переопределите метод `pointInside(_:with:)`, чтобы определить область, в которой кнопка будет реагировать на нажатие.
   
```swift
class CustomButton: UIButton {
    var increasedTouchArea: CGFloat = 20 // Увеличение области нажатия
    
    override func point(inside point: CGPoint, with event: UIEvent?) -> Bool {
        let increasedBounds = bounds.insetBy(dx: -increasedTouchArea, dy: -increasedTouchArea)
        return increasedBounds.contains(point)
    }
}
```

2. В вашем контроллере установите значение `increasedTouchArea` для кнопки, чтобы увеличить область нажатия.

```swift
let customButton = CustomButton()
customButton.increasedTouchArea = 20 // Установка увеличения области нажатия
```

Этот подход позволит увеличить область нажатия для кнопки, что может быть полезно, если кнопка мала или требуется увеличить точность нажатия[4].