# In-App реклама

In-App - формат рекламы, встраиваемый в контент приложения во время естественных пауз, таких как окончание выполнения целевого действия.

## Шаг 0. Интеграция рекламного SDK.

Выполните интеграцию рекламного SDK согласно [инструкции](integration.md).

## Шаг 1. Инициализация загрузчика рекламы.

Загрузчик рекламы `InAppAdLoader` позволяет предзагружать рекламу прежде, чем взаимодействие пользователя с приложением будет прервано показом рекламы.

Чтобы инициализировать загрузчик рекламы необходимо вызвать метод `Mads#inAppAdLoader`:
```kotlin
import ru.tander.mads.Mads

val inAppAdLoader = Mads.inAppAdLoader()
```

## Шаг 2. Настройка параметров загружаемой рекламы.

Для загрузки рекламы необходимо инициализировать `InAppAdRequest`:
```kotlin
import ru.tander.mads.inapp.loading.InAppAdRequest

val inAppAdRequest = InAppAdRequest(
    padId = TODO("Идентификатор места показа рекламы"),
    debugCreative = false, // true - если нужно загрузить отладочный рекламный креатив.
    otherParams = emptyMap(), // дополнительные параметры для загрузки рекламы.
)
```

## Шаг 3. Загрузка рекламы.

Для старта загрузки рекламы необходимо вызвать метод `InAppAdLoader#load`:
```kotlin
inAppAdLoader.load(
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
inAppAdLoader.clear()
```

После вызова `InAppAdLoader#clear` загрузчик можно использовать повторно, если это необходимо.

## Шаг 4. Показ рекламы.

Для показа загруженной рекламы необходимо вызвать метод `Mads#showInAppAd`, передав ему загруженное рекламное объявление в качестве аргумента:
```kotlin
import ru.tander.mads.Mads

Mads.showInAppAd(
    activity = this,
    ad = inAppAd, // объявление, загруженное на шаге 3
    tag = TODO("Произвольная строка, идентифицирующая место показа рекламы внутри приложения."),
    colorsLight = InAppAdColors.light(), // цветовая схема рекламного объявления, которая будет использоваться, если на устройстве установлена светлая тема.
    colorsDark = InAppAdColors.dark(), // цветовая схема рекламного объявления, которая будет использоваться, если на устройстве установлена тёмная тема.
)
```

## Шаг 5 (опционально). Реакция на события показа рекламы.

Если интегрирующему приложению нужно следить за событиями показа рекламы - приложение может вызвать метод `Mads#subscribeToInAppAdShowingEvents`, передав ему в качестве аргумента соответствующий колбэк:
```kotlin
import ru.tander.mads.Mads
import ru.tander.mads.inapp.showing.events.InAppAdShowingEventsCallback
import ru.tander.mads.inapp.showing.events.InAppAdShowingEventsSubscription

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
