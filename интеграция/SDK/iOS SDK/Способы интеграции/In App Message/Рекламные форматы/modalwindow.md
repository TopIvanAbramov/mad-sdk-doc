---
label: Modal Window
---

![|300x200](./inapp-preview.png){height="100"}

## 1. Описание

Modal Window - это формат рекламы, который представляет собой диалоговое окно, которое появляется поверх текущего экрана пользователя. 


## 2. Кастомизация UI

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


## Пример интеграции In-App рекламы

Пример интеграции In-App рекламы можно посмотреть в [репозитории на GitHub](https://github.com/magnit-tech/mads-ios-sdk).
