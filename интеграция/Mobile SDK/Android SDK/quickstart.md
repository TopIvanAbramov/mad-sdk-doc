---
label: Быстрый старт
order: 100
icon: rocket
---

# Быстрый старт — Android

Это руководство поможет установить MADS SDK и показать первое рекламное объявление в Android-приложении.

## 1. Добавление зависимостей

Перед началом интеграции необходимо добавить зависимость от SDK в нужные модули проекта:

```kotlin
repositories {
    mavenCentral()
}

dependencies {
    implementation("ru.magnit.mads.mobile:android-sdk:0.0.1")
}
```

## 2. Инициализация SDK

Инициализацию SDK необходимо выполнить после старта приложения, вызвав метод `Mads.init(...)`:

```kotlin
import ru.tander.mads.Mads

class DemoApplication : Application() {

    override fun onCreate() {
        Mads.init(this)
    }
}
```

## 3. Идентификация пользователя

Когда пользователь входит в систему, важно связать его действия, совершенные анонимно, с его постоянным профилем. Для этого после авторизации необходимо передать внутренний `userId`:

```kotlin
import ru.tander.mads.Mads

fun onUserIdUpdated(userId: String) {
    Mads.userId = userId
}
```

## Что дальше?

- [In App Message (ModalWindow)](<Рекламные форматы/In App Message/modal-window.md>) — показ всплывающей рекламы
- [InLine (Multiformat)](<Рекламные форматы/InLine/multiformat.md>) — встраивание рекламных блоков в контент
- [Отладка](debugging.md) — тестирование интеграции
