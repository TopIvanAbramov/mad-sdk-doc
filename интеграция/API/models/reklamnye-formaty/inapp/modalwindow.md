---
label: ModalWindow
order: 10
icon: browser
---

## 1. Запрос рекламного формата

```bash
curl -X GET "[API_BASE_URL]/v1/ads/get?padId={YOUR_PAD_ID}&userId={USER_ID}"
```

## 2. Ответ 

### AdResponse

| Поле | Тип | Описание |
|------|-----|----------|
| `padId` | Int | Id рекламного плейсмента |
| `templateGroupName` | String | `modalwindow` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для модалок |
| `items` | [Creative] | Массив креативов (модалок) |
| `statistics` | [Statistic] | Пиксели для всего блока |

### DisplayOptions

Общие UI-конфиги для всех модалок.

| Поле | Тип | Описание |
|------|-----|----------|
| `modalWindowCornerRadius` | Int | Радиус скругления верха модалки |
| `backgroundColor` | String | Цвет заднего фона модалки |

###  Creative (модалка)

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
| `button` | Button? | Кнопка |

## 3. Пример полного ответа

```json
{
  "padId": 2,
  "templateGroupName": "modalwindow",
  "templateName": "modal_1000x700_static",
  "displayOptions": {
    "modalWindowCornerRadius": 25,
    "backgroundColor": "#FFFFFF"
  },
  "items": [
    {
      "id": "1",
      "integrationType": "sdk",
      "statistics": [
        {
          "url": "https://example.com/pixels/impression?id=banner_1",
          "type": "impression"
        }
      ],
      "contentType": "image",
      "content": {
        "url": "https://picsum.photos/541/226?random=1",
        "size": {
          "width": 1000,
          "height": 700
        }
      },
      "title": "Круассан со скидкой 20%",
      "subtitle": "заказывайте свежую выпечку каждый день. Доставка от 20 минут",
      "button": {
        "type": "cta",
        "subType": "rich",
        "text": "купить",
        "model": {
          "link": {
            "type": "external",
            "url": "magnit.ru"
          }
        },
        "displayOptions": {
          "cornerRadius": 15,
          "textColor": "#FF0000",
          "backgroundColor": "#000000"
        }
      },
      "markingInfo": {
        "labelText": "реклама",
        "orgInfo": "ООО «СВ Ритейл» г. Москва. ОГРН: 118774670977, erid: 2RanymQ9iGh",
        "url": "https://kontragent.magnit.ru/",
        "buttonText": "Подробнее о рекламе в Магните",
        "description": "Помогаем с продвижением товаров и услуг - о вашем бизнесе узнают миллионы покупателей Магнита",
        "displayOptions": {
          "backgroundColor": "#FFFFFF66",
          "textColor": "#000000"
        }
      }
    }
  ],
  "statistics": [
    {
      "url": "https://example.com/pixels/impression?id=root",
      "type": "impression"
    }
  ]
}
```