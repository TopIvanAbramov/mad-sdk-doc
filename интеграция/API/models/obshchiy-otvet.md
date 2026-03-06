---
label: Общий ответ
order: 130
icon: file-code
---

# Общий ответ

Общая структура ответа API для всех рекламных форматов одинаковая. Отличаются только `templateGroupName`, набор пикселей и модели Creative

## AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | Формат: `modalwindow`, `fulscreen`, `multiformat`, `stories`, `clickout` — см. [Рекламные форматы](reklamnye-formaty/index.md) |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация (зависит от формата) |
| `items` | [Creative] | Массив креативов: разный для каждого формата |
| `statistics` | [Statistic] | Пиксели для всего блока |

Структура `displayOptions` и `Creative` зависит от `templateGroupName`. См. [Рекламные форматы](reklamnye-formaty/index.md).
