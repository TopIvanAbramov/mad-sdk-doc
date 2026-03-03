---
label: Stories
order: 20
icon: image
---

# Stories

Формат `templateGroupName: "stories"`. Вертикальные карточки со слайдами.

## AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | Формат: `stories` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для обложек |
| `items` | [Creative] | Массив креативов (обложек со слайдами) |
| `statistics` | [Statistic] | Пиксели для всего блока сторис |

## DisplayOptions

Общие UI-конфиги для отображения обложек.

| Поле | Тип | Описание |
|------|-----|----------|
| `defaultSlideDuration` | Int | Длительность показа слайда (сек) |
| `coverBorderColor` | String | Цвет обложки не просмотренной |
| `coverBorderViewedColor` | String | Цвет просмотренной обложки |
| `coverCornerRadius` | Int | Скругление обложки и рамки |
| `size` | AspectRatio | Соотношение сторон обложки |
| `interItemSpacing` | Int | Расстояние между обложками |

## Creative (обложка)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | String | Уникальный идентификатор объявления |
| `integrationType` | String | "sdk", "native" |
| `contentType` | String | "image", "video" |
| `content` | Image | Контент обложки |
| `title` | String | Заголовок под обложкой |
| `statistics` | [Statistic] | Пиксели для обложки |
| `slides` | [StorySlide] | Массив слайдов |

## StorySlide (слайд)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | Int | Порядковый номер слайда |
| `isViewed` | Bool | Просмотрен ли слайд |
| `contentType` | String | "image", "video" |
| `content` | Image / Video | Контент слайда |
| `button` | Button? | Кнопка действия (макс. 1 на слайд) |
| `markingInfo` | MarkingInfo? | Маркировка |
| `statistics` | [Statistic] | Пиксели для слайда |
