---
label: ModalWindow
order: 90
---

# ModalWindow

In App Message — это всплывающие окна внутри приложения. Подробнее о формате — в разделе [Рекламные форматы](../../../../../concepts/ad-formats.md).

## 1. Загрузка рекламы

При загрузке рекламы необходимо передавать `padId`, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые `padId` (см. [Отладка](../../debugging.md)).

```kotlin
import ru.tander.mads.inapp.loading.InAppAdRequest

val inAppAdRequest = InAppAdRequest(
    padId = "1",             // идентификатор места показа рекламы
    debugCreative = false, // true — если нужно загрузить отладочный рекламный креатив
    targetings = emptyMap(), // параметры таргетингов
)
```

Для загрузки вызовите suspend-функцию `Mads.inApp.load(...)`:

```kotlin
import ru.tander.mads.Mads

val inAppAdResponse = Mads.inApp.load(inAppAdRequest)
```

Простановка таймаутов загрузки рекламы:
```kotlin
withTimeout(10_000L) {
    Mads.inApp.load(...)
}
```

Отмена загрузки рекламы:
```kotlin
val scope = CoroutineScope(Dispatchers.IO + SupervisorJob())

val job = scope.launch {
    try {
        Mads.inApp.load(...)  // загрузка рекламы
    } catch (e: CancellationException) {
        println("Load cancelled")
        // Cleanup if needed
    }
}

// Cancel anytime:
job.cancel()  // Отменяет загрузку рекламы
```

### Параметры запроса

| Параметр | Тип | Обязательный | Описание |
|----------|-----|:---:|----------|
| `padId` | `String` | ✅ | Id места размещения |
| `targetings` | `Map<String, String>` | ✅ | Словарь таргетингов для персонализации рекламы |
| `debugCreative` | `Boolean` | ❌ | Загрузка дебаг-креативов (по умолчанию: `false`) |

> [!TIP]
> Параллельно можно загружать несколько рекламных объявлений. Для отмены загрузки достаточно остановить работу `CoroutineScope`, в контексте которого была вызвана suspend-функция `Mads.inApp.load(...)`.

## 2. Таргетирование

Передайте словарь таргетингов при загрузке рекламы. Подробнее о таргетингах — в разделе [Таргетирование](../../../../../concepts/targeting.md).

```kotlin
val targetings: Map<String, String> = mapOf(
    "gender" to "female"
)

val inAppAdRequest = InAppAdRequest(
    padId = 1,               
    targetings = targetings,
)

val inAppAdResponse = Mads.inApp.load(inAppAdRequest)
```

## 3. Обработка результата запроса и показ загруженной рекламы

Обработайте результат загрузки рекламы:

```kotlin
import ru.tander.mads.inapp.loading.InAppAdResponse

when (inAppAdResponse) {
    is InAppAdResponse.Success -> {
        // Для показа загруженной рекламы передайте текущую `Activity` 
        inAppAdResponse.content.show(activity = ...)
    }
    is InAppAdResponse.NoContent -> {
        // Запрос за рекламой завершился без ошибок, но реклама не была подобрана.
    }
    is InAppAdResponse.Failure -> {
        // Запрос за рекламой завершился с ошибой.
    }
}
```

### Возможные состояния

| Тип значения                | Параметры                                   | Описание                                                                                    |
|-----------------------------|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `Success`   | `content` - контент для отображения рекламы | Реклама загружена успешно. Отобразить загруженную рекламу можно вызвав `content.show(...)`. |
| `Failure` | `reason` - причина ошибки загрузки рекламы                                          | Загрузка рекламы завершилась ошибкой.                     |
| `NoContent`   |  - | Запрос за рекламой завершился без ошибок, но реклама не была подобрана.    |

## 4. Кастомизация UI (Modal Window)

> [!TIP]
> Кастомизация удалена из настроек и вынесена в ответ сервера. Кастомизировать рекламу можно будет в админ панели.


## 5. Реакция на действия пользователя

SDK не реагирует на действия пользвателя на рекламном объявлении (такие как "нажатие на кнопку" и т.д.). Реакцию на эти действия необходимо реализовать на стороне интегрирующего приложения:

```kotlin
import ru.tander.mads.inapp.showing.InAppAdShowingAction

when (inAppAdResponse) {

    is InAppAdResponse.Success -> {

        inAppAdResponse.content.actions
            .onEach(::handleInAppAdShowingAction)
            .launchIn(...)

        inAppAdResponse.content.show(activity = ...)
    }
    ...
}

fun handleInAppAdShowingAction(action: InAppAdShowingAction) {
    when (action) {
        is InAppAdShowingAction.OnUrlClicked -> {
            openUrl(action.type, action.url) // открытие ссылки при нажатии на кнопку на рекламном объявлении
        }
        is InAppAdShowingAction.OnPromocodeCopy -> {
            applyPromoCode(action.promocode) // применение промокода при нажатии на кнопку на рекламном объявлении
        }
    }
}
```

### Возможные значения InAppAdShowingAction

| Тип значения                           | Параметры                                                                                                                                                        | Описание                                |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `InAppAdShowingAction.OnUrlClicked`    | `adInfo` - инфомация о рекламном объявлении<br>`type` - тип ссылки (напр. "web", "deeplink" и т.д.)<br>`url` - ссылка, по которой необходимо осуществить переход | Нажатие на кнопку перехода по ссылке    |
| `InAppAdShowingAction.OnPromocodeCopy` | `adInfo` - инфомация о рекламном объявлении<br>`promocode` - промокод, который необходимо скопировать                                                            | Нажатие на кнопку копирования промокода |

## 6. Реакция на события показа рекламы

При необходимости приложение может отслеживать события показа рекламы, используя `InAppAdContent.events`:

```kotlin
import ru.tander.mads.inapp.showing.InAppAdShowingEvent

when (inAppAdResponse) {

    is InAppAdResponse.Success -> {

        inAppAdResponse.content.events
            .onEach(::handleInAppAdShowingEvents)
            .launchIn(...)

        inAppAdResponse.content.show(activity = ...)
    }
    ...
}

fun handleInAppAdShowingEvent(event: InAppAdShowingEvent) {
    when (action) {
        is InAppAdShowingEvent.OnCreativeView -> {
            // объявление показано
        }
        is InAppAdShowingEvent.OnCreativeDismissed -> {
            // объявление скрыто
        }
    }
}
```

### Возможные значения InAppAdShowingEvent

| Тип значения                              | Параметры                                   | Описание                      |
|-------------------------------------------|---------------------------------------------|-------------------------------|
| `InAppAdShowingEvent.OnCreativeView`      | `adInfo` - инфомация о рекламном объявлении | Показ рекламного объявления   |
| `InAppAdShowingEvent.OnCreativeDismissed` | `adInfo` - инфомация о рекламном объявлении | Скрытие рекламного объявления |

## Пример

Полный пример интеграции In App рекламы — в [репозитории на GitHub](https://github.com/magnit-tech/mads-android-sdk).
