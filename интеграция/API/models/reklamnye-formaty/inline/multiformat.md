---
label: Multiformat
order: 10
icon: image
---

## AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | `multiformat` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для баннерного блока |
| `items` | [Creative] | Массив креативов (слайдов) |
| `statistics` | [Statistic] | Пиксели для всего баннерного блока |

## DisplayOptions

Общие UI-конфиги для баннерного блока.

| Поле | Тип | Описание |
|------|-----|----------|
| `size` | AspectRatio | Соотношение сторон слайдов |
| `useAutoScroll` | Bool | Включено ли автоперелистывание |
| `autoScrollTimeout` | Int? | Время между автоперелистываниями (сек) |
| `useHeader` | Bool | Использовать хедер над креативами |
| `cornerRadius` | Int | Радиус скругления слайдов |
| `earsWidth` | Int | Ширина ушей (видимых частей боковых баннеров) |
| `interItemSpacing` | Int | Отступы между слайдами |
| `format` | String | `carousel` |

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
