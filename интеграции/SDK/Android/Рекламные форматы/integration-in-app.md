# InApp

![|300x200](./inapp-preview.png){height="100"}

In‑App реклама — это всплывающие окна внутри приложения, где можно разместить изображение, кнопку с диплинком или копирование промокода. Такой формат помогает привлекать внимание пользователей и повышать конверсии прямо в интерфейсе приложения поверх любого экрана.


# 1. Инициализация загрузчика рекламы

Загрузчик рекламы `InAppAdLoader` позволяет предзагружать рекламу прежде, чем взаимодействие пользователя с приложением будет прервано показом рекламы.

Чтобы инициализировать загрузчик рекламы необходимо вызвать метод:
```kotlin
import ru.tander.mads.Mads

val loader = Mads.inAppAdLoader()
```

# 2. Загрузка рекламы

При загрузке рекламы необходимо передавать padId, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые padId. Смотрите подробнее в [Отладка интеграции](#6-отладка-интеграции)


```kotlin
import ru.tander.mads.inapp.loading.InAppAdRequest

val inAppAdRequest = InAppAdRequest(
    padId = 1,             // идентификатор места показа рекламы
    debugCreative = false, // true - если нужно загрузить отладочный рекламный креатив.
    targetings = emptyMap(), // параметры таргетингов.
)
```

Для старта загрузки рекламы необходимо вызвать метод `InAppAdLoader#load`:
```kotlin
loader.load(
    adRequest = inAppAdRequest,
    onSuccess = { inAppAd ->
        // реклама загружена
    },
    onFailure = { failure ->
        // произошла ошибка загрузки рекламы
    },
)
```

Один `InAppAdLoader` может параллельно загружать несколько рекламных объявлений.

Для отмены всех загрузок, запущенных одним и тем же `InAppAdLoader`, достаточно вызвать метод `InAppAdLoader#clear`:

```kotlin
loader.clear()
```

После вызова `InAppAdLoader#clear` загрузчик можно использовать повторно, если это необходимо.

# 3. Таргетирование рекламы

Для более точного таргетирования рекламы нужно передавать параметры в словаре targetings. Названия ключей и возможные значения должны совпадать между платформами.

Необходимо передавать словарь ключей в метод загрузки рекламы:


```kotlin
val targetings: Map<String, String> = mapOf(
    "gender" to "female"
)

val inAppAdRequest = InAppAdRequest(
    padId = 1,               
    targetings = targetings, // таргетинги
)


loader.load(
    adRequest = inAppAdRequest,
    onSuccess = { inAppAd ->
        // реклама загружена
    },
    onFailure = { failure ->
        // произошла ошибка загрузки рекламы
    },
)
```

# 4. Показ рекламы

Для показа загруженной рекламы необходимо вызвать метод `Mads#showInAppAd`, передав ему загруженное рекламное объявление в качестве аргумента:
```kotlin
import ru.tander.mads.Mads

Mads.showInAppAd(
    activity = this,
    ad = inAppAd, // объявление, загруженное на шаге 3
    tag = TODO("Произвольная строка, идентифицирующая место показа рекламы внутри приложения."),
    colors = InAppAdColors(), // цветовая схема рекламного объявления
)
```

# 5. Реакция на события с рекламой

Если интегрирующему приложению нужно следить за событиями показа рекламы - приложение может вызвать метод `Mads#subscribeToInAppAdShowingEvents`, передав ему в качестве аргумента соответствующий колбэк:
```kotlin
import ru.tander.mads.Mads
import ru.tander.mads.inapp.showing.events.InAppAdShowingEventsCallback

val inAppAdShowingEventsCallback = object : InAppAdShowingEventsCallback {

    // вызывается при показе объявления
    fun onAdShown() { ... }

    // вызывается при ошибке показа объявления
    fun onAdFailedToShow(failure: Throwable) { ... }

    // вызывается при скрытии объявления
    fun onAdDismissed() { ... }

    // вызывается при нажатии пользователем на кнопку, открывающую диплинк
    fun onAdDeeplinkButtonClicked(deeplink: String) { ... }

    // вызывается при нажатии пользователем на кнопку, копирующую промокод
    fun onAdCopyPromoCodeButtonClicked(promoCode: String) { ... }
}

val inAppAdShowingEventsSubscription = Mads.subscribeToInAppAdShowingEvents(
    tag = TODO("Произвольная строка, идентифицирующая место показа рекламы внутри приложения (см. шаг 4)."),
    callback = inAppAdShowingEventsCallback,
)
```

Для отмены подписки на событиям показа рекламы достаточно вызвать метод `InAppAdShowingEventsSubscription#cancel`:
```kotlin
inAppAdShowingEventsSubscription.cancel()
```


# 6. Отладка интеграции

Для отладки интеграции In-App рекламы можно использовать отладочный рекламный креатив:
```kotlin
import ru.tander.mads.inapp.loading.InAppAdRequest

val inAppAdRequest = InAppAdRequest(
    padId = "1",          // отладочный padId 
    debugCreative = true, // включаем загрузку тестового креатива
)
```


# 7. Кастомизация UI

Для кастомизации возможно поменять цвета кнопок и скругления. В будущем появится возможность указать кастомный шрифт.

| Цвет                        | Описание                                 |
|-----------------------------|------------------------------------------|
| backgroundColor             | цвет фона диалога с рекламой             |
| primaryButtonColor          | цвет фона для кнопки с диплинком         |
| primaryButtonContentColor   | цвет контента на кнопке с диплинком      |
| secondaryButtonColor        | цвет фона для кнопки с промокодом        |
| secondaryButtonContentColor | цвет контента на кнопке с промокодом     |
| loaderColor                 | цвет прогресс-бара на диалоге с рекламой |
| cornerRadius                | скругления кнопок (в процессе реализации)|

Чтобы применить кастомизацию необходимо передать передать конфиг в метод показа InApp рекламы:

```kotlin
Mads.showInAppAd(
    activity = this,
    ad = inAppAd,
    tag = TODO("Произвольная строка, идентифицирующая место показа рекламы внутри приложения."),
    colors = InAppAdColors(), // UI конфиг
)
```

# Пример интеграции In-App рекламы

Пример интеграции In-App рекламы можно посмотреть в [репозитории на GitHub](https://github.com/magnit-tech/mads-android-sdk).
