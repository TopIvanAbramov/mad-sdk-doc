---
label: ModalWindow
order: 90
---

# ModalWindow

In App Message — это всплывающие окна внутри приложения. Подробнее о формате — в разделе [Рекламные форматы](../../../../../concepts/ad-formats.md).

## 1. Загрузка рекламы

При загрузке рекламы необходимо передавать `padId`, который можно получить в рекламной админке либо от менеджера. Для отладки можно использовать тестовые `padId` (см. [Отладка](../../debugging.md)).

+++ Delegate

Создайте загрузчик и подпишитесь через делегат:

```swift
import MadsSDK

private let loader = InAppAdLoader()
```

```swift
loader.delegate = self
```

Вызовите `loader.load(...)`:

```swift
let request = InAppAdRequest(
    padId: "1",
    targetings: ["some_targeting": "value"],
    isDebugCreativeEnabled: false
)

loader.load(request)
```

Отмена загрузки:

```swift
let cancellable = loader.load(request)

// Отмена в любой момент
cancellable.cancel()
```

+++ Async

Для загрузки вызовите `MadsSDK.inApp.load(...)`:

```swift
import MadsSDK

let request = InAppAdRequest(
    padId: "1",
    targetings: ["some_targeting": "value"],
    isDebugCreativeEnabled: false
)

let response = await MadsSDK.inApp.load(request)
```

Простановка таймаута загрузки рекламы:

```swift
let task = Task {
    await MadsSDK.inApp.load(request)
}

Task {
    try? await Task.sleep(for: .seconds(10))
    task.cancel()
}
```

Отмена загрузки:

```swift
let task = Task {
    await MadsSDK.inApp.load(request)
}

// Отмена в любой момент
task.cancel()
```

+++

### Параметры запроса

| Параметр | Тип | Обязательный | Описание |
|----------|-----|:---:|----------|
| `padId` | `String` | ✅ | Id места размещения |
| `targetings` | `[String: String]` | ✅ | Словарь таргетингов для персонализации рекламы |
| `isDebugCreativeEnabled` | `Bool` | ❌ | Загрузка дебаг-креативов (по умолчанию: `false`) |

> [!TIP]
> Параллельно можно загружать несколько рекламных объявлений. Для Delegate — создайте несколько экземпляров `InAppAdLoader`. Для Async — запустите загрузку в отдельных `Task`, для отмены вызовите `task.cancel()`.

## 2. Таргетирование

Передайте словарь таргетингов при загрузке рекламы. Подробнее о таргетингах — в разделе [Таргетирование](../../../../../concepts/targeting.md).

+++ Delegate
```swift
let targetings: [String: String] = ["gender": "female"]

loader.load(
    InAppAdRequest(
        padId: "1",
        targetings: targetings
    )
)
```
+++ Async
```swift
let targetings: [String: String] = ["gender": "female"]

let request = InAppAdRequest(
    padId: "1",
    targetings: targetings
)

let response = await MadsSDK.inApp.load(request)
```
+++

## 3. Обработка результата запроса и показ загруженной рекламы

Обработайте результат загрузки рекламы:

+++ Delegate
```swift
extension ViewController: InAppAdLoaderDelegate {
    func inAppAdLoader(
        _ loader: InAppAdLoaderProtocol,
        didReceive response: InAppAdLoader.Response
    ) {
        switch response {
        case let .success(inAppAd, slot):
            // Для показа можно передать UIViewController (опционально)
            MadsSDK.showInAppAd(inAppAd, inVC: self)
        case let .failure(error, slot):
            // Загрузка завершилась с ошибкой
            break
        case let .noContent(slot):
            // Реклама не была подобрана
            break
        }
    }
}
```
+++ Async
```swift
let response = await MadsSDK.inApp.load(request)

switch response {
case let .success(inAppAd, slot):
    // Для показа можно передать UIViewController (опционально)
    MadsSDK.showInAppAd(inAppAd, inVC: self)
case let .failure(error, slot):
    // Загрузка завершилась с ошибкой
    break
case let .noContent(slot):
    // Реклама не была подобрана
    break
}
```
+++

### Возможные состояния

| Тип значения | Параметры | Описание |
|--------------|-----------|----------|
| `Success` | `inAppAd` — загруженная реклама, `slot` — информация о размещении | Реклама загружена успешно. Отобразить вызвав `MadsSDK.showInAppAd(...)`. |
| `Failure` | `error` — причина ошибки, `slot` — информация о размещении | Загрузка рекламы завершилась ошибкой. |
| `NoContent` | `slot` — информация о размещении | Запрос завершился без ошибок, но реклама не была подобрана. |

### Возможные ошибки загрузки (InAppAdLoadError)

| Тип значения | Описание |
|--------------|----------|
| `network` | Ошибка сети при загрузке рекламы |
| `sdkNotInitialized` | SDK не инициализирован |
| `adLoad` | Не удалось загрузить рекламу |
| `cancelled` | Загрузка рекламы отменена |

> [!TIP]
> Параметр `viewController` в `MadsSDK.showInAppAd(_:inVC:)` является опциональным. Если не указан — SDK определяет хост-контроллер автоматически.

## 4. Кастомизация UI (Modal Window)

> [!TIP]
> Кастомизация удалена из настроек и вынесена в ответ сервера. Кастомизировать рекламу можно будет в админ панели.

## 5. Реакция на действия пользователя

SDK не реагирует на действия пользователя на рекламном объявлении (такие как "нажатие на кнопку" и т.д.). Реакцию на эти действия необходимо реализовать на стороне интегрирующего приложения:

+++ Delegate

Подпишитесь на события рекламного объекта через делегат:

```swift
inAppAd.delegate = self
```

Реализуйте протокол:

```swift
extension ViewController: InAppAdDelegate {
    func inAppAd(_ ad: InAppAd, didEmit action: InAppAd.Action) {
        switch action {
        case let .onUrlClicked(info, url):
            openUrl(url) // открытие ссылки
        case let .onPromocodeCopy(info, promocode):
            applyPromocode(promocode) // применение промокода
        }
    }

    func inAppAd(_ ad: InAppAd, didEmit event: InAppAd.Event) {
        // см. секцию «6. Реакция на события показа рекламы»
    }
}
```

+++ Async
```swift
for await action in inAppAd.actions {
    switch action {
    case let .onUrlClicked(info, url):
        openUrl(url) // открытие ссылки
    case let .onPromocodeCopy(info, promocode):
        applyPromocode(promocode) // применение промокода
    }
}
```
+++

### Возможные значения InAppAd.Action

| Тип значения | Параметры | Описание |
|--------------|-----------|---------|
| `InAppAd.Action.onUrlClicked` | `info` — информация о рекламном объявлении<br>`url` — ссылка для перехода | Нажатие на кнопку перехода по ссылке |
| `InAppAd.Action.onPromocodeCopy` | `info` — информация о рекламном объявлении<br>`promocode` — промокод для копирования | Нажатие на кнопку копирования промокода |

## 6. Реакция на события показа рекламы

При необходимости приложение может отслеживать события показа рекламы:

+++ Delegate
```swift
func inAppAd(_ ad: InAppAd, didEmit event: InAppAd.Event) {
    switch event {
    case let .onCreativeView(info):
        break // объявление показано
    case let .onCreativeFailedToShow(info):
        break // не удалось показать объявление
    case let .onCreativeDismissed(info, type):
        break // объявление скрыто
    }
}
```
+++ Async
```swift
for await event in inAppAd.events {
    switch event {
    case let .onCreativeView(info):
        break // объявление показано
    case let .onCreativeFailedToShow(info):
        break // не удалось показать объявление
    case let .onCreativeDismissed(info, type):
        break // объявление скрыто
    }
}
```
+++

### Возможные значения InAppAd.Event

| Тип значения | Параметры | Описание |
|--------------|-----------|---------|
| `InAppAd.Event.onCreativeView` | `info` — информация о рекламном объявлении | Показ рекламного объявления |
| `InAppAd.Event.onCreativeFailedToShow` | `info` — информация о рекламном объявлении | Не удалось показать объявление |
| `InAppAd.Event.onCreativeDismissed` | `info` — информация о рекламном объявлении<br>`type` — способ закрытия | Скрытие рекламного объявления |

### Возможные значения DismissType

| Тип значения | Описание |
|--------------|----------|
| `close` | Тап по кнопке закрытия (крестик) |
| `closeSwipe` | Свайп вниз |
| `closeOutsideClick` | Тап вне модалки (на overlay) |

> [!TIP]
> Actions (действия пользователя) обязательны к обработке на интегрирующей стороне. Events (события показа) — информационные и необязательные.

## Пример

Полный пример интеграции In App рекламы — в [репозитории на GitHub](https://github.com/magnit-tech/mads-ios-sdk).
