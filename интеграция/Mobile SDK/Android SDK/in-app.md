---
label: In App Message
order: 90
icon: browser
---

# In App Message — Android

In App Message — это всплывающие окна внутри приложения. Подробнее о формате — в разделе [Рекламные форматы](../../../concepts/ad-formats.md).

## 1. Инициализация загрузчика

Загрузчик рекламы `InAppAdLoader` позволяет предзагружать рекламу прежде, чем взаимодействие пользователя с приложением будет прервано показом рекламы.

```kotlin
import ru.tander.mads.Mads

val loader = Mads.inAppAdLoader()
```

## 2. Загрузка рекламы

При загрузке рекламы необходимо передавать `padId`, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые `padId` (см. [Отладка](debugging.md)).

```kotlin
import ru.tander.mads.inapp.loading.InAppAdRequest

val inAppAdRequest = InAppAdRequest(
    padId = 1,             // идентификатор места показа рекламы
    debugCreative = false, // true — если нужно загрузить отладочный рекламный креатив
    targetings = emptyMap(), // параметры таргетингов
)
```

Для старта загрузки вызовите метод `InAppAdLoader#load`:

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

### Параметры запроса

| Параметр | Тип | Обязательный | Описание |
|----------|-----|:---:|----------|
| `padId` | `Int` | ✅ | Id места размещения |
| `targetings` | `Map<String, String>` | ✅ | Словарь таргетингов для персонализации рекламы |
| `debugCreative` | `Boolean` | ❌ | Загрузка дебаг-креативов (по умолчанию: `false`) |

> [!TIP]
> Один `InAppAdLoader` может параллельно загружать несколько рекламных объявлений. Для отмены всех загрузок вызовите `loader.clear()`. После этого загрузчик можно использовать повторно.

## 3. Таргетирование

Передайте словарь таргетингов при загрузке рекламы. Подробнее о таргетингах — в разделе [Таргетирование](../../../concepts/targeting.md).

```kotlin
val targetings: Map<String, String> = mapOf(
    "gender" to "female"
)

val inAppAdRequest = InAppAdRequest(
    padId = 1,               
    targetings = targetings,
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

## 4. Показ рекламы

Для показа загруженной рекламы вызовите метод `Mads#showInAppAd`:

```kotlin
import ru.tander.mads.Mads

Mads.showInAppAd(
    activity = this,
    ad = inAppAd,
    tag = "main_screen_promo",
    colors = InAppAdColors(),
)
```

### Параметры показа

| Параметр | Тип | Обязательный | Описание |
|----------|-----|:---:|----------|
| `activity` | `Activity` | ✅ | Activity, внутри которой покажется рекламное объявление |
| `ad` | `InAppAd` | ✅ | Загруженное рекламное объявление |
| `tag` | `String` | ✅ | Уникальный тег места размещения. Нужен для сохранения работы коллбеков при смене конфигурации. Принцип аналогичен [Activity Result](https://developer.android.com/training/basics/intents/result) |
| `colors` | `InAppAdColors` | ❌ | Цветовая схема рекламного объявления (по умолчанию: стандартная) |

## 5. Кастомизация UI (Modal Window)

Для кастомизации внешнего вида модального окна можно задать цвета и скругления через `InAppAdColors`:

| Свойство | Описание |
|----------|----------|
| `backgroundColor` | Цвет фона диалога с рекламой |
| `primaryButtonColor` | Цвет фона кнопки с диплинком |
| `primaryButtonContentColor` | Цвет контента на кнопке с диплинком |
| `secondaryButtonColor` | Цвет фона кнопки с промокодом |
| `secondaryButtonContentColor` | Цвет контента на кнопке с промокодом |
| `loaderColor` | Цвет прогресс-бара |
| `cornerRadius` | Скругления кнопок *(в процессе реализации)* |

```kotlin
Mads.showInAppAd(
    activity = this,
    ad = inAppAd,
    tag = "main_screen_promo",
    colors = InAppAdColors(), // UI конфиг
)
```

## 6. Реакция на события

Если приложению нужно отслеживать события показа рекламы, вызовите `Mads#subscribeToInAppAdShowingEvents`:

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

    // вызывается при нажатии на кнопку диплинка
    fun onAdDeeplinkButtonClicked(deeplink: String) { ... }

    // вызывается при нажатии на кнопку промокода
    fun onAdCopyPromoCodeButtonClicked(promoCode: String) { ... }
}

val subscription = Mads.subscribeToInAppAdShowingEvents(
    tag = "main_screen_promo",
    callback = inAppAdShowingEventsCallback,
)
```

Для отмены подписки:
```kotlin
subscription.cancel()
```

## Пример

Полный пример интеграции In App рекламы — в [репозитории на GitHub](https://github.com/magnit-tech/mads-android-sdk).
