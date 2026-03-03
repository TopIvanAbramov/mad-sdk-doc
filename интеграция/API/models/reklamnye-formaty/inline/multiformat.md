---
label: Multiformat
order: 10
icon: image
---

# Multiformat

Формат `templateGroupName: "multiformat"`. Карусель баннеров.

## AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | Формат: `multiformat` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для баннерного блока |
| `items` | [Creative] | Массив креативов (слайдов) |
| `statistics` | [Statistic] | Пиксели для всего баннерного блока |

## DisplayOptions

Общие UI-конфиги для баннерного блока.

| Поле | Тип | Описание |
|------|-----|----------|
| `size` | AspectRatio | Соотношение сторон слайдов |
| `useAutoScroll` | Bool | Автоперелистывание |
| `autoScrollTimeout` | Int? | Время между автоперелистыванием (сек) |
| `useHeader` | Bool | Использовать хедер над креативами |
| `cornerRadius` | Int | Радиус скругления слайдов |
| `earsWidth` | Int | Ширина ушей (видимых частей боковых баннеров) |
| `interItemSpacing` | Int | Отступы между слайдами |
| `format` | String | "carousel", "1x1", "2x2", "2x1", "3x3" |

## Creative (слайд)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | String | Уникальный идентификатор объявления |
| `integrationType` | String | "sdk", "native" |
| `statistics` | [Statistic] | Пиксели для слайда |
| `contentType` | String | "image" |
| `content` | Image / Video | Контент слайда |
| `markingInfo` | MarkingInfo | Маркировка |
| `link` | Link | Ссылка в слайде |
