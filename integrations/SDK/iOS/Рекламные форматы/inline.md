---
label: Inline реклама
order: 2
---

# Inline

Inline реклама — это встраиваемые рекламные блоки внутри контента приложения. Поддерживаемые типы: **Stories**, **Banner**, **Clickout**.


# 1. Создаем загрузчик рекламы

```swift
import MadsSDK

private let loader = InlineAdLoader()
private var loadedInlineAd: InlineAd?
```


# 2. Подписываемся на состояние загрузчика

Проставляем делегат у загрузчика:

```swift
loader.delegate = self
```

Реализуем протокол:

```swift
extension ViewController: InlineAdLoaderDelegate {
    func inlineAdLoader(_ loader: any InlineAdLoaderProtocol, didChangeState state: InlineAdLoader.State) {
        switch state {
        case let .loaded(inlineAd):
            break
        case let .failed(inlineAdLoadError):
            break
        }
    }
}
```

# 3. Загружаем рекламу

```swift
loader.load(
    padId: 1,
    targetings: [
        "some_targeting": "value"
    ]
)
```

# 4. Подписываемся на состояния Inline рекламы

Проставляем делегат у рекламного объекта:

```swift
inlineAd.delegate = self
self.loadedInlineAd = inlineAd
```

Реализуем протокол:

```swift
extension ViewController: InlineAdDelegate {
    func inlineAd(_ ad: InlineAd, didEmit event: InlineAd.Event) {
        switch event {
        case .shown:
            break
        case .failedToShow:
            break
        case let .deeplinkClicked(url):
            break
        case let .promocodeClicked(promocode):
            break
        case .dismissed:
            break
        }
    }
}
```

# 5. Отображаем рекламу

Необходимо передать текущий UIViewController, в контексте которого будет отображаться реклама:

```swift
let inlineView = inlineAd.render(in: self)
self.view.addSubview(inlineView)
```

# Кастомизация

Для кастомизации возможно поменять цвета кнопок и скругления. В будущем появится возможность указать кастомный шрифт.

| Название параметра | Значение | Описание |
|----------|----------|----------|
| primaryColor | Color | цвет для кнопки с диплинком |
| secondaryColor | Color| цвет для кнопки с промокодом |
| cornerRadius | CGFloat | скругления кнопок|


Цвет можно указать через hex строку (#ff0000) или UIColor

```swift
public enum Color {
    case hex(String)
    case uiColor(UIColor)
}
```

Чтобы применить кастомизацию необходимо передать конфиг в метод рендеринга Inline рекламы:

```swift
let uiConfig = UIConfiguration(
    primaryColor: .uiColor(UIColor.red),
    secondaryColor: .uiColor(UIColor.grey),
    cornerRadius: 15
)

let inlineView = inlineAd.render(in: self, uiConfig: uiConfig)
self.view.addSubview(inlineView)
```
