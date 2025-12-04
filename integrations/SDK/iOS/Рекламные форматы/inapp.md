# InApp

In‑App реклама — это всплывающие окна внутри приложения, где можно разместить изображение, кнопку с диплинком или копирование промокода. Такой формат помогает привлекать внимание пользователей и повышать конверсии прямо в интерфейсе приложения поверх любого экрана.


# 1. Создаем загрузчик рекламы

```swift
import MadsSDK

private let loader = InAppAdLoader()
private var loadedInAppAd: InAppAd?
```


# 2. Подписываемся на состояние загрузчика

Проставляем делег у загрузчика:


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

# 4. Подписываемся на состояния inApp рекламы

Проставляем делег у рекламого объекта:

```swift
inAppAd.delegate = self
self.inAppAd = inAppAd
```

Реализуем протокол:

```swift
extension ViewController: InAppAdDelegate {
    func inAppAd(_ ad: InAppAd, didEmit event: InAppAd.Event) {
        switch event {
        case .adShown:
            break
        case .adFailedToShow:
            break
        case let .adDeeplinkClicked(uRL):
            break
        case let .adPromocodeClicked(promocode)
            break
        case let .error(error):
           break
        case .adDismissed:
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
