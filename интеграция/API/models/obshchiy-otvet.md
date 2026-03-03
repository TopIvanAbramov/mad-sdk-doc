---
label: Общий ответ
order: 10
icon: file-code
---

# Общий ответ

Общая структура ответа API для всех рекламных форматов.

## AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | Рекламный формат: `modalwindow`, `stories`, `multiformat`, `clickout` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация (зависит от формата) |
| `items` | [Creative] | Массив креативов |
| `statistics` | [Statistic] | Пиксели для всего блока |

Структура `displayOptions` и `Creative` зависит от `templateGroupName`. См. [Рекламные форматы](reklamnye-formaty/index.md).
