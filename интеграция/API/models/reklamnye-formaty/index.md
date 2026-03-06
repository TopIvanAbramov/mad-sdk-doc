---
label: Рекламные форматы
order: 30
icon: image
---

# Рекламные форматы

Модели ответа API для каждого рекламного формата соответствуют аналогичным в MadSDK. Подробное описание [рекламных форматов](../../../../concepts/ad-formats.md).

## Как определить рекламный формат по ответу API?

Значение `templateGroupName` в ответе определяет формат и структуру данных

| templateGroupName | Описание | Модели данных
|----------|----------|----------|
| `modalwindow` | Диалоговое окно поверх экрана | [Model](inapp/modalwindow.md) |
| `multiformat` |  Карусель баннеров | [Model](inline/multiformat.md) |
| `stories` | Сторисы | [Model](inline/stories.md) |