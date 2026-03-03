---
label: In App Message
order: 90
icon: browser
---

# In App Message — iOS

In App Message — это всплывающие окна внутри приложения. Подробнее о формате — в разделе [Рекламные форматы](../concepts/ad-formats.md).

## 1. Инициализация загрузчика

Загрузчик рекламы `InAppAdLoader` позволяет предзагружать рекламу прежде, чем взаимодействие пользователя с приложением будет прервано показом рекламы.

```swift
import MadsSDK

private let loader = InAppAdLoader()
private var loadedInAppAd: InAppAd?
```

## 2. Загрузка рекламы

Подпишитесь на состояние загрузчика через делегат:

```swift
loader.delegate = self
```

Реализуйте протокол:

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

При загрузке рекламы необходимо передавать `padId`, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые `padId` (см. [Отладка](debugging.md)).

Вызовите метод `InAppAdLoader#load`:

```swift
loader.load(
    padId: 1,
    targetings: [
        "some_targeting": "value"
    ],
    isDebugCreativeEnabled: false
)
```

### Параметры запроса

| Параметр | Тип | Обязательный | Описание |
|----------|-----|:---:|----------|
| `padId` | `Int` | ✅ | Id места размещения |
| `targetings` | `[String: String]` | ✅ | Словарь таргетингов для персонализации рекламы |
| `isDebugCreativeEnabled` | `Bool` | ❌ | Загрузка дебаг-креативов (по умолчанию: `false`) |

## 3. Таргетирование

Передайте словарь таргетингов при загрузке рекламы. Подробнее о таргетингах — в разделе [Таргетирование](../concepts/targeting.md).

```swift
let targetings: [String: String] = ["gender": "female"]
 
loader.load(
    padId: 1, 
    targetings: targetings
)
```

## 4. Показ рекламы

Для показа загруженной рекламы передайте текущий `UIViewController`:

```swift
MadsSDK.showInAppAd(
  inAppAd,
  inVC: self
)
```

### Параметры показа

| Параметр | Тип | Обязательный | Описание |
|----------|-----|:---:|----------|
| `_ ad` | `InAppAd` | ✅ | Загруженное рекламное объявление |
| `viewController` | `UIViewController` | ✅ | UIViewController, поверх которого покажется модальное окно |
| `uiConfig` | `UIConfiguration` | ❌ | UI конфигурация для кастомизации (по умолчанию: `.default`) |

## 5. Кастомизация UI (Modal Window)

Для кастомизации внешнего вида модального окна задайте параметры через `UIConfiguration`:

| Параметр | Тип | Описание |
|----------|-----|----------|
| `backgroundColor` | `Color` | Цвет фона диалога с рекламой |
| `primaryColor` | `Color` | Цвет кнопки с диплинком |
| `primaryButtonContentColor` | `Color` | Цвет контента на кнопке с диплинком |
| `secondaryColor` | `Color` | Цвет кнопки с промокодом |
| `secondaryButtonContentColor` | `Color` | Цвет контента на кнопке с промокодом |
| `loaderColor` | `Color` | Цвет прогресс-бара |
| `cornerRadius` | `CGFloat` | Скругления кнопок |

Цвет можно указать через hex-строку (`#ff0000`) или `UIColor`:

```swift
public enum Color {
    case hex(String)
    case uiColor(UIColor)
}
```

Пример кастомизации:

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

## 6. Реакция на события

Подпишитесь на события рекламного объекта через делегат:

```swift
inAppAd.delegate = self
self.inAppAd = inAppAd
```

Реализуйте протокол:

```swift
extension ViewController: InAppAdDelegate {
    func inAppAd(_ ad: InAppAd, didEmit event: InAppAd.Event) {
        switch event {
        case .shown:
            break
        case .failedToShow:
            break
        case let .deeplinkClicked(uRL):
            break // необходимо обработать открытие диплинка
        case let .promocodeClicked(promocode)
            break
        case .dismissed:
            break
        }
    }
}
```

## Пример

Полный пример интеграции In App рекламы — в [репозитории на GitHub](https://github.com/magnit-tech/mads-ios-sdk).
