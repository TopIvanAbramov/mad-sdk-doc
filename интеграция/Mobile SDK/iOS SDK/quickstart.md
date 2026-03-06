---
label: Быстрый старт
order: 100
icon: rocket
---

# Быстрый старт — iOS

Это руководство поможет установить MADS SDK и показать первое рекламное объявление в iOS-приложении.

## 1. Добавление зависимостей

В Xcode проекте добавьте пакет: **File → Add Package Dependencies**.

Укажите URL репозитория с Swift Package. Рекомендуем зафиксировать определённую версию библиотеки:

```
https://github.com/magnit-tech/mads-ios-sdk
```

Убедитесь, что MadSDK прилинковано к таргету вашего приложения.

## 2. Инициализация SDK

Инициализацию SDK лучше всего выполнить в классе `SceneDelegate` или `AppDelegate`:

```swift
import MadSDK

MadsSDK.initialize()
```

## 3. Идентификация пользователя

Когда пользователь входит в систему, важно связать его действия, совершенные анонимно, с его постоянным профилем. Для этого после авторизации передайте внутренний `userId`:

```swift
MadsSDK.userId = "sample-user-id"
```

## 4. Конфигурация SDK

| Настройка | Описание |
|-----------|----------|
| `MadsSDK.isDebugLogsEnabled = true` | Включает дебаг-логи в консоль |
| `MadsSDK.isDebugCreativeEnabled = true` | Включает загрузку дебаг-креатива |

## Что дальше?

- [In App Message (ModalWindow)](Рекламные форматы/In App Message/modal-window.md) — показ всплывающей рекламы
- [InLine (Multiformat)](Рекламные форматы/InLine/multiformat.md) — встраивание рекламных блоков в контент
- [Отладка](debugging.md) — тестирование интеграции
