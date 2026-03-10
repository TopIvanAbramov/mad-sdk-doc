---
label: Быстрый старт
order: 100
icon: rocket
---

# Быстрый старт — Android

Это руководство поможет установить MADS SDK и показать первое рекламное объявление в Android-приложении.

## 1. Добавление зависимостей

Временно SDK поставляется в виде архива с локальным Maven-репозиторием, который нужно разархивировать в любой удобной директории (например внутри директории проекта по пути `./maven/mads`).

Затем SDK можно подключать в любой модуль проекта:
```kotlin
repositories {
    maven {
        // адрес директории с разархивированным Maven-репозиторием
        url = uri("$rootDir/maven/mads")
    }
}

dependencies {
    implementation("ru.tander.mads:integration:0.0.1")
}
```

> [!NOTE]
> Позже SDK будет поставляться через общедоступный Maven-репозиторий.

## 2. Инициализация SDK

Инициализацию SDK необходимо выполнить после старта приложения, вызвав метод `Mads#init`:

```kotlin
import ru.tander.mads.Mads

class DemoApplication : Application() {

    override fun onCreate() {
        Mads.init(
            context = this,
            openLinkHandler = ::openLink,
        )
    }

    private fun openLink(link: String) {
        Intent(Intent.ACTION_VIEW, link.toUri())
            .addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
            .let(::startActivity)
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
