---
label: Отладка
order: 70
icon: bug
---

# Отладка интеграции — Android

## Тестовый рекламный креатив

Для отладки интеграции можно использовать отладочный рекламный креатив. Передайте `debugCreative = true` при создании запроса:

```kotlin
import ru.tander.mads.inapp.loading.InAppAdRequest

val inAppAdRequest = InAppAdRequest(
    padId = 1,            // отладочный padId
    debugCreative = true, // включаем загрузку тестового креатива
)
```

> [!TIP]
> Тестовый креатив позволяет проверить корректность интеграции без реальных рекламных кампаний.

## Пример

Полный пример интеграции — в [репозитории на GitHub](https://github.com/magnit-tech/mads-android-sdk).
