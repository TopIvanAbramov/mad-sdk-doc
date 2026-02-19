---
label: Общая информация
order: 1
---

![|400x400](./inline.png){height="200"}

InLine реклама — это встраиваемые рекламные блоки внутри контента приложения. Поддерживаемые типы: **Stories**, **Banner**, **Clickout**.

Возвращаемый тип рекламного блока регулируется на стороне админки. Для мобильного приложения встраивание идентично для разных форматов.

ToDo: в процессе разработки

<!-- 
## 1. Инициализация загрузчика рекламы

```swift
import MadsSDK

private let loader = InlineAdLoader()
private var loadedInlineAd: InlineAd?
```


## 2. Загрузка рекламы

Перед началом загрузки рекламы необходимо подписаться на состояние загрузчика. Для этого проставляем делегат:

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

При загрузке рекламы необходимо передавать padId, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые padId (ToDo)


```swift
loader.load(
    padId: 1,
    targetings: [
        "some_targeting": "value"
    ]
)
```

## 3. Таргетирование рекламы

Таргетинги — это дополнительная информация о пользователе и контексте показа, которую приложение передаёт в MadSDK вместе с запросом рекламы. Каждый таргетинг описывается парой «ключ–значение»: например, пол пользователя, возрастная группа, тип тарифа, экран или раздел приложения, категория отображаемого контента и т.п.

Эти параметры используются рекламной системой для более точного подбора объявлений: ad‑сервер сопоставляет присланные таргетинги с настройками рекламных кампаний и отбирает только те, которые подходят под указанные условия. Чем точнее и стабильнее передаваемые таргетинги, тем более релевантную рекламу получают пользователи и тем выше эффективность монетизации.

Необходимо передавать словарь таргетингов в метод загрузки рекламы в метод `Mads#load`:


```swift
let targetings: [String: String] = ["gender": "female"]
 
loader.load(
    padId: 1, 
    targetings: targetings // таргетинги
)
```

## 4. Показ рекламы

Для показа загруженной рекламы необходимо передавать текущий UIViewcontroller, поверх которого покажется попап в метод `Mads#showInAppAd`

```swift
MadsSDK.showInAppAd(
  inAppAd,    // объявление, загруженное на шаге 3
  inVC: self  // UIViewcontroller, поверх которого покажется попап
)
```

## 4. Показ рекламы

Для показа загруженной рекламы необходимо передать текущий UIViewController, в контексте которого будет отображаться реклама в метод `Mads#showInLineAd`:

```swift
let inlineView = MadsSDK.showInLineAd(in: self)
self.view.addSubview(inlineView)
```

## 5. Реакция на события с рекламой

Интегрирующему приложению необходимо обрабатывать нажатия на диплинки, кнопки и тп. Для этого нужно подписаться на соответствующий протокол.

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
``` -->