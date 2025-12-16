---
label: InApp
order: 1
---

# InApp

![|300x200](./inapp-preview.png){height="100"}

In‑App реклама — это всплывающие окна внутри приложения, где можно разместить изображение, кнопку с диплинком или копирование промокода. Такой формат помогает привлекать внимание пользователей и повышать конверсии прямо в интерфейсе приложения поверх любого экрана.


# 1. Создаем загрузчик рекламы

```swift
import MadsSDK

private let loader = InAppAdLoader()
private var loadedInAppAd: InAppAd?
```


# 2. Подписываемся на состояние загрузчика

Проставляем делегат у загрузчика:


```swift
loader.delegate = self
```

Реализуем протокол:

```swift
extension ViewController: InAppAdLoaderDelegate {
    func inAppAdLoader(_ loader: any InAppAdLoaderProtocol, didChangeState state: InAppAdLoader.State) {
        switch state {
        case let .loaded(inAppAd):
            break
        case let .failed(inAppAdLoadError):
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

# 4. Подписываемся на состояния InApp рекламы

Проставляем делегат у рекламого объекта:

```swift
inAppAd.delegate = self
self.inAppAd = inAppAd
```

Реализуем протокол:

```swift
extension ViewController: InAppAdDelegate {
    func inAppAd(_ ad: InAppAd, didEmit event: InAppAd.Event) {
        switch event {
        case .shown:
            break
        case .failedToShow:
            break
        case let .deeplinkClicked(uRL):
            break
        case let .promocodeClicked(promocode)
            break
        case .dismissed:
            break
        }
    }
}
```

# 5. Показываем рекламу

Необходимо передавать текущий UIViewcontroller, поверх которого покажется попап

```swift
MadsSDK.showInAppAd(inAppAd, inVC: self)
```

# Кастомизация

Для кастомизации возможно поменять цвета кнопок и скругления. В будущем появится возможность указать кастомный шрифт.

| Название параметра | Значение | Описание |
|----------|----------|----------|
| backgroundColor | Color | цвет фона диалога с рекламой
| primaryColor | Color | цвет для кнопки с диплинком |
| primaryButtonContentColor | Color | цвет контента на кнопке с диплинком |
| secondaryColor | Color| цвет для кнопки с промокодом |
| secondaryButtonContentColor | Color | цвет контента на кнопке с промокодом |
| loaderColor | Color | цвет прогресс-бара на диалоге с рекламой |
| cornerRadius | CGFloat | скругления кнопок|


Цвет можно указать через hex строку (#ff0000) или UIColor

```swift
    public enum Color {
        case hex(String)
        case uiColor(UIColor)
    }
```

Чтобы применить кастомизацию необходимо передать передать конфиг в метод показа InApp рекламы:

```swift
let uiConfig = UIConfiguration(
    primaryColor: .uiColor(UIColor.red),
    secondaryColor: .uiColor(UIColor.grey),
    cornerRadius: 15
)

MadsSDK.showInAppAd(
    inAppAd,
    inVC: self,
    uiConfig: uiConfig
)
```
