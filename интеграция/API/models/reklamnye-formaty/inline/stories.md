---
label: Stories
order: 20
icon: image
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
| `templateGroupName` | String | `stories` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для обложек |
| `items` | [Creative] | Массив креативов (обложек со слайдами) |
| `statistics` | [Statistic] | Пиксели для всего блока сторис |

### DisplayOptions

Общие UI-конфиги для отображения обложек.

| Поле | Тип | Описание |
|------|-----|----------|
| `defaultSlideDuration` | Int | Длительность показа слайда (сек) |
| `coverBorderColor` | String | Цвет не просмотренной обложки |
| `coverBorderViewedColor` | String | Цвет просмотренной обложки |
| `coverCornerRadius` | Int | Скругление обложки и рамки |
| `size` | AspectRatio | Соотношение сторон обложки |
| `interItemSpacing` | Int | Расстояние между обложками |

### Creative (обложка)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | String | Уникальный идентификатор объявления |
| `integrationType` | String | "sdk", "native" |
| `contentType` | String | "image", "video" |
| `content` | Image | Контент обложки |
| `title` | String | Заголовок под обложкой |
| `statistics` | [Statistic] | Пиксели для обложки |
| `slides` | [StorySlide] | Массив слайдов |

### StorySlide (слайд)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | Int | Порядковый номер слайда |
| `isViewed` | Bool | Просмотрен ли слайд |
| `contentType` | String | "image", "video" |
| `content` | Image / Video | Контент слайда |
| `button` | Button? | Кнопка действия (макс. 1 на слайд) |
| `markingInfo` | MarkingInfo? | Маркировка |
| `statistics` | [Statistic] | Пиксели для слайда |

## 3. Пример полного ответа

```json
{
  "padId": 1,
  "templateGroupName": "stories",
  "templateName": "stories",
  "displayOptions": {
    "defaultSlideDuration": 5000
  },
  "items": [
    {
      "id": "1",
      "integrationType": "sdk",
      "contentType": "image",
      "content": {
        "url": "https://picsum.photos/360/360?random=1",
        "size": {
          "width": 360,
          "height": 360
        }
      },
      "title": "Title 1",
      "statistics": [
        {
          "url": "https://example.com/pixels/impression?id=cover_1",
          "type": "impression"
        }
      ],
      "slides": [
        {
          "id": 1,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=1_1",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_1_slide_1",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_1_slide_1",
              "type": "click"
            }
          ]
        },
        {
          "id": 2,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=1_2",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_1_slide_2",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_1_slide_2",
              "type": "click"
            }
          ]
        }
      ]
    },
    {
      "id": "2",
      "integrationType": "sdk",
      "contentType": "image",
      "content": {
        "url": "https://picsum.photos/360/360?random=2",
        "size": {
          "width": 360,
          "height": 360
        }
      },
      "title": "Title 2",
      "statistics": [
        {
          "url": "https://example.com/pixels/impression?id=cover_2",
          "type": "impression"
        }
      ],
      "slides": [
        {
          "id": 1,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=2_1",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_2_slide_1",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_2_slide_1",
              "type": "click"
            }
          ]
        },
        {
          "id": 2,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=2_2",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_2_slide_2",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_2_slide_2",
              "type": "click"
            }
          ]
        }
      ]
    },
    {
      "id": "3",
      "integrationType": "sdk",
      "contentType": "image",
      "content": {
        "url": "https://picsum.photos/360/360?random=3",
        "size": {
          "width": 360,
          "height": 360
        }
      },
      "title": "Title 3",
      "statistics": [
        {
          "url": "https://example.com/pixels/impression?id=cover_3",
          "type": "impression"
        }
      ],
      "slides": [
        {
          "id": 1,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=3_1",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_3_slide_1",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_3_slide_1",
              "type": "click"
            }
          ]
        },
        {
          "id": 2,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=3_2",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_3_slide_2",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_3_slide_2",
              "type": "click"
            }
          ]
        }
      ]
    },
    {
      "id": "4",
      "integrationType": "sdk",
      "contentType": "image",
      "content": {
        "url": "https://picsum.photos/360/360?random=4",
        "size": {
          "width": 360,
          "height": 360
        }
      },
      "title": "Title 4",
      "statistics": [
        {
          "url": "https://example.com/pixels/impression?id=cover_4",
          "type": "impression"
        }
      ],
      "slides": [
        {
          "id": 1,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=4_1",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_4_slide_1",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_4_slide_1",
              "type": "click"
            }
          ]
        },
        {
          "id": 2,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=4_2",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_4_slide_2",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_4_slide_2",
              "type": "click"
            }
          ]
        }
      ]
    },
    {
      "id": "5",
      "integrationType": "sdk",
      "contentType": "image",
      "content": {
        "url": "https://picsum.photos/360/360?random=5",
        "size": {
          "width": 360,
          "height": 360
        }
      },
      "title": "Title 5",
      "statistics": [
        {
          "url": "https://example.com/pixels/impression?id=cover_5",
          "type": "impression"
        }
      ],
      "slides": [
        {
          "id": 1,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=5_1",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_5_slide_1",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_5_slide_1",
              "type": "click"
            }
          ]
        },
        {
          "id": 2,
          "isViewed": false,
          "contentType": "image",
          "content": {
            "url": "https://picsum.photos/360/640?random=5_2",
            "size": {
              "width": 360,
              "height": 640
            }
          },
          "button": {
            "type": "cta",
            "subType": "rich",
            "text": "Visit Magnit",
            "textColor": "#FFFFFF",
            "backgroundColor": "#FF0000",
            "model": {
              "link": {
                "type": "Deeplink",
                "url": "www.magnit.ru"
              }
            }
          },
          "markingInfo": null,
          "statistics": [
            {
              "url": "https://example.com/pixels/impression?id=cover_5_slide_2",
              "type": "impression"
            },
            {
              "url": "https://example.com/pixels/click?id=cover_5_slide_2",
              "type": "click"
            }
          ]
        }
      ]
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