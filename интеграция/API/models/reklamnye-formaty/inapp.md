---
label: InApp
order: 10
icon: browser
---

# InApp (Modal Window)

Формат `templateGroupName: "modalwindow"`. Диалоговое окно поверх экрана.

## AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | Формат: `modalwindow` (в будущем: `fulscreen`) |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для модалок |
| `items` | [Creative] | Массив креативов (модалок) |
| `statistics` | [Statistic] | Пиксели для всего блока |

## DisplayOptions

Общие UI-конфиги для всех модалок.

| Поле | Тип | Описание |
|------|-----|----------|
| `modalWindowCornerRadius` | Int | Радиус скругления верха модалки |
| `backgroundColor` | String | Цвет заднего фона |

## Creative (модалка)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | String | Уникальный идентификатор объявления |
| `integrationType` | String | Способ интеграции: "sdk", "native" |
| `statistics` | [Statistic] | Пиксели для модалки |
| `contentType` | String | "image" (видео — в будущем) |
| `content` | Image / Video | Контент креатива |
| `markingInfo` | MarkingInfo | Маркировка |
| `title` | String? | Заголовок |
| `subtitle` | String? | Подзаголовок |
| `button` | Button? | Кнопка (опционально) |
