---
label: Переиспользуемые модели
order: 20
icon: database
---

# Переиспользуемые модели

Общие типы данных, используемые в разных рекламных форматах.

## MarkingInfo

Информация о маркировке рекламы.

| Поле | Тип | Описание |
|------|-----|----------|
| `labelText` | String | Текст элемента: "Реклама", "Соц. реклама" |
| `orgInfo` | String | Информация о рекламодателе, текст от ООО до erid |
| `url` | String | Ссылка в кнопке |
| `buttonText` | String | Текст на кнопке в маркировочной модалке |
| `description` | String | Текст над информацией о рекламодателе |
| `displayOptions` | MarkingDisplayOptions | UI-настройки лейбла |

## MarkingDisplayOptions

| Поле | Тип | Описание |
|------|-----|----------|
| `backgroundColor` | String | Цвет фона для лейбла |
| `textColor` | String | Цвет текста |

## Statistic

Статистический пиксель для отслеживания событий.

| Поле | Тип | Описание |
|------|-----|----------|
| `url` | String | URL пикселя |
| `type` | String | Тип пикселя |
| `value` | Int? | Время в секундах |
| `pvalue` | Int? | Время в процентах от duration |
| `viewablePercent` | Int? | Процент вхождения в область видимости |

## AspectRatio

| Поле | Тип | Описание |
|------|-----|----------|
| `width` | Int | Ширина |
| `height` | Int | Высота |

## Image

| Поле | Тип | Описание |
|------|-----|----------|
| `url` | String | URL изображения |
| `size` | AspectRatio | Размеры |

## Video

| Поле | Тип | Описание |
|------|-----|----------|
| `url` | String | URL видео |
| `size` | AspectRatio | Размеры |
| `bitrate` | Int | Битрейт |
| `format` | String | Формат (mp4) |
| `duration` | Int | Длительность в миллисекундах |

## Button

| Поле | Тип | Описание |
|------|-----|----------|
| `type` | String | "cta", "customAction" и др. |
| `subType` | String | "rich", "promocodeCopy" и др. |
| `text` | String | Текст кнопки (макс. 30 символов) |
| `model` | ButtonContent | LinkButton или PromocodeButton |
| `displayOptions` | ButtonDisplayOptions | UI-настройки |

## ButtonDisplayOptions

| Поле | Тип | Описание |
|------|-----|----------|
| `textColor` | String | Цвет текста в HEX |
| `backgroundColor` | String | Цвет фона в HEX |
| `cornerRadius` | Int | Скругление кнопки |

## ButtonContent (LinkButton)

| Поле | Тип | Описание |
|------|-----|----------|
| `link` | Link | Ссылка |

## ButtonContent (PromocodeButton)

| Поле | Тип | Описание |
|------|-----|----------|
| `promocode` | String | Промокод |
| `onCopyText` | String | Текст кнопки после копирования |
| `alertText` | String? | Текст алерта |

## Link

| Поле | Тип | Описание |
|------|-----|----------|
| `type` | String | "Deeplink" или "External" |
| `url` | String | URL |
