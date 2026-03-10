---
label: Multiformat
order: 10
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
| `templateGroupName` | String | `multiformat` |
| `templateName` | String | Имя шаблона, реализующего UI отображение |
| `displayOptions` | DisplayOptions | UI-конфигурация для баннерного блока |
| `items` | [Creative] | Массив креативов (слайдов) |
| `statistics` | [Statistic] | Пиксели для всего баннерного блока |

### DisplayOptions

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

###  Creative (слайд)

| Поле | Тип | Описание |
|------|-----|----------|
| `id` | String | Уникальный идентификатор объявления |
| `integrationType` | String | "sdk", "native" |
| `statistics` | [Statistic] | Пиксели для слайда |
| `contentType` | String | "image" |
| `content` | Image / Video | Контент слайда |
| `markingInfo` | MarkingInfo | Маркировка |
| `link` | Link | Ссылка в слайде |

## 3. Пример полного ответа

```json
{
   "padId":2,
   "templateGroupName":"multiformat",
   "templateName": "carousel_541x226_static",
   "displayOptions":{
      "size":{
         "width": 541,
         "height": 226
      },
      "useAutoScroll": true,
      "autoScrollTimeout": 4000,
      "useHeader": false,
      "cornerRadius": 25,
      "earsWidth": 20,
      "interItemSpacing": 20,
      "format": "carousel"
   },
   "items":[
      {
         "id": "1",
         "integrationType": "sdk",
         "contentType": "image",
         "content": {
            "url": "https://picsum.photos/541/226?random=1",
            "size": {
               "width":541,
               "height":226
            }
         },
         "link": {
            "type": "deeplink",
            "url": "magnit://web?url=https://google.com"
		 },
         "markingInfo":{
            "labelText":"реклама",
            "orgInfo":"ООО «СВ Ритейл» г. Москва. ОГРН: 118774670977, erid: 2RanymQ9iGh",
            "url":"https://kontragent.magnit.ru/",
            "buttonText":"Подробнее о рекламе в Магните",
            "description":"Помогаем с продвижением товаров и услуг - о вашем бизнесе узнают миллионы покупателей Магнита",
            "displayOptions":{
               "backgroundColor":"#FFFFFF66",
               "textColor":"#000000"
            }
         },
         "statistics":[
            {
               "url": "https://example.com/pixels/impression?id=root",
               "type": "impression",
              "viewablePercent":50
            },
            {
               "url": "https://example.com/pixels/click?id=root",
               "type": "click"
            }
         ]
      },
      {
         "id":"2",
         "integrationType":"sdk",
         "contentType":"image",
         "content":{
            "url":"https://picsum.photos/541/226?random=2",
            "size":{
               "width":541,
               "height":226
            }
         },
         "link": {
            "type": "deeplink",
            "url": "magnit://web?url=https://google.com"
		 },
         "markingInfo":{
            "labelText":"реклама",
            "orgInfo":"ООО «СВ Ритейл» г. Москва. ОГРН: 118774670977, erid: 2RanymQ9iGh",
            "url":"https://kontragent.magnit.ru/",
            "buttonText":"Подробнее о рекламе в Магните",
            "description":"Помогаем с продвижением товаров и услуг - о вашем бизнесе узнают миллионы покупателей Магнита",
            "displayOptions":{
               "backgroundColor":"#FFFFFF66",
               "textColor":"#000000"
            }
         },
         "statistics":[
            {
               "url": "https://example.com/pixels/impression?id=root",
               "type": "impression",
              "viewablePercent":50
            },
            {
               "url": "https://example.com/pixels/click?id=root",
               "type": "click"
            }
         ]
      },
      {
         "id":"3",
         "integrationType":"sdk",
         "contentType":"image",
         "content":{
            "url":"https://picsum.photos/541/226?random=3",
            "size":{
               "width":541,
               "height":226
            }
         },
         "link": {
            "type": "deeplink",
            "url": "magnit://web?url=https://google.com"
		 },
         "markingInfo":{
            "labelText":"реклама",
            "orgInfo":"ООО «СВ Ритейл» г. Москва. ОГРН: 118774670977, erid: 2RanymQ9iGh",
            "url":"https://kontragent.magnit.ru/",
            "buttonText":"Подробнее о рекламе в Магните",
            "description":"Помогаем с продвижением товаров и услуг - о вашем бизнесе узнают миллионы покупателей Магнита",
            "displayOptions":{
               "backgroundColor":"#FFFFFF66",
               "textColor":"#000000"
            }
         },
         "statistics":[
            {
               "url": "https://example.com/pixels/impression?id=root",
               "type": "impression",
              "viewablePercent":50
            },
            {
               "url": "https://example.com/pixels/click?id=root",
               "type": "click"
            }
         ]
      },
      {
         "id":"4",
         "integrationType":"sdk",
         "contentType":"image",
         "content":{
            "url":"https://picsum.photos/541/226?random=4",
            "size":{
               "width":541,
               "height":226
            }
         },
         "link": {
            "type": "deeplink",
            "url": "magnit://web?url=https://google.com"
		 },
         "statistics":[
            {
               "url": "https://example.com/pixels/impression?id=root",
               "type": "impression",
              "viewablePercent":50
            },
            {
               "url": "https://example.com/pixels/click?id=root",
               "type": "click"
            }
         ]
      },
      {
         "id": "5",
         "integrationType": "sdk",
         "contentType": "image",
         "content":{
            "url":"https://picsum.photos/541/226?random=5",
            "size":{
               "width":541,
               "height":226
            }
         },
         "link": {
            "type": "deeplink",
            "url": "magnit://web?url=https://google.com"
		 },
         "statistics":[
            {
               "url": "https://example.com/pixels/impression?id=root",
               "type": "impression",
              "viewablePercent":50
            },
            {
               "url": "https://example.com/pixels/click?id=root",
               "type": "click"
            }
         ]
      }
   ],
   "statistics":[
       {
          "url":"https://example.com/pixels/impression?id=root",
          "type": "impression",
          "viewablePercent":50
       },
       {
          "url":"https://example.com/pixels/click?id=root",
          "type":"click"
       }
   ]
}
```