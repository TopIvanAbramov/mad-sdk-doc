# InApp

InApp реклама предназначена для показа попапов по тригеру


# 1. Создаем загрузчик рекламы

```swift
private let loader = InAppAdLoader()
private var loadedInAppAd: InAppAd?
```


# 2. Подписываемся на состояние загрузчика


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
    padId: "",
    targetings: [
        "some_targeting": "value"
    ]
)
```

# 4. Сохраняем загруженную рекламу и подписываемся на изменения состояния

```swift
inAppAd.delegate = self
self.inAppAd = inAppAd
```

Реализуем протокол:

```swift
extension ViewController: InAppAdDelegate {
    func inAppAd(_ ad: InAppAd, didEmit event: InAppAd.Event) {
        switch event {
        case .visible:
            break
        case let .deeplinkButtonClicked(uRL):
            break
        case let .promocodeButtonClicked(promocode)
            break
        case let .error(error):
           break
        case .closed:
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
