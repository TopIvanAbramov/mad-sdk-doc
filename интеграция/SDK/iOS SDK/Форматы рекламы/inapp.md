---
label: InApp
order: 1
---

# InApp

![|300x200](./inapp-preview.png){height="100"}

In‑App реклама — это всплывающие окна внутри приложения, где можно разместить изображение, кнопку с диплинком или копирование промокода. Такой формат помогает привлекать внимание пользователей и повышать конверсии прямо в интерфейсе приложения поверх любого экрана.


# 1. Инициализация загрузчика рекламы

Загрузчик рекламы `InAppAdLoader` позволяет предзагружать рекламу прежде, чем взаимодействие пользователя с приложением будет прервано показом рекламы.

Чтобы инициализировать загрузчик рекламы необходимо вызвать метод:

```swift
import MadsSDK

private let loader = InAppAdLoader()
private var loadedInAppAd: InAppAd?
```


# 2. Загрузка рекламы

Перед началом загрузки рекламы необходимо подписаться на состояние загрузчика. Для этого проставляем делегат:


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


При загрузке рекламы необходимо передавать padId, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые padId. Смотрите подробнее в [Отладка интеграции](#6-отладка-интеграции)

Для старта загрузки рекламы необходимо вызвать метод `InAppAdLoader#load`:


```swift
loader.load(
    padId: 1,                     // идентификатор места показа рекламы
    targetings: [                 // параметры таргетингов.
        "some_targeting": "value"
    ],
    isDebugCreativeEnabled: false // true - если нужно загрузить отладочный рекламный креатив.
)
```

# 3. Таргетирование рекламы

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

# 4. Показ рекламы

Для показа загруженной рекламы необходимо передавать текущий UIViewcontroller, поверх которого покажется попап в метод `Mads#showInAppAd`

```swift
MadsSDK.showInAppAd(
  inAppAd,    // объявление, загруженное на шаге 3
  inVC: self  // UIViewcontroller, поверх которого покажется попап
)
```

# 5. Реакция на события с рекламой

Интегрирующему приложению необходимо обрабатывать нажатия на диплинки, кнопки и тп. Для этого нужно подписаться на соответствующий протокол.

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

# 6. Отладка интеграции

Для отладки интеграции In-App рекламы можно использовать отладочный рекламный креатив:

```swift
loader.load(
    padId: 1,                    // отладочный padId 
    isDebugCreativeEnabled: true // включаем загрузку тестового креатива
)
```


# 7. Кастомизация UI

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


# Пример интеграции In-App рекламы

Пример интеграции In-App рекламы можно посмотреть в [репозитории на GitHub](https://github.com/magnit-tech/mads-ios-sdk).
