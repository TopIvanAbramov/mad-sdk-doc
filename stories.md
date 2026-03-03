struct AdResponse {
    let padId: Int,
    let templateGroupName: "stories",
    let templateName: String,
    let displayOptions: DisplayOptions, // опции, описывающие UI конфигруацию для отображения items в контейнере, а не самих кретивов в ним.
    let items: [Creative],
    let statistics: [Statistic]         // пиксели для всего блока сторис
}


DisplayOptions:

общие UI конфиги для всех сторис

struct DisplayOptions {
   let defaultSlideDuration: Int          // дефолтная длительность показа слайда в секундах
   let coverBorderColor: String           // цвет обложки не просмотренной обложки
   let coverBorderViewedColor: String     // цвет просмотренной обложки
   let coverCornerRadius: Int             // скругление обложки и рамки
   let size: AspectRatio                  // cоотношение сторон для обложки без текста
   let interItemSpacing: Int              // расстояние между элементами обложек
}


Модель обложек со слайдами:

struct Creative {
    let id: String                      // уникальный идентификатор рекламного объявления
    let integrationType: "sdk",         // подтип креатива, описывает способ интеграции: enum("sdk", "native")
    let contentType: "image",           // возможно в будущем появится гиф обложка как у InAppStory
    let content: Image,
    let title: String,                  // заголовок под обложкой
    let statistics: [Statistic],        // пиксели для обложки
    let slides: [StorySlide]
}


Модель слайда:

struct StorySlide<Image/Video> {
    let id: Int                        // порядковый номер (при шаблонизации - номер итерации по slides)
    // UPD: согласовать с платформой хранения идентификатора в отдельной роли креатива под слайд стори при заведении в кабинете (заполнение паттерна). При ротации нужно учитывать просмотр / непросмотр для формирование isViewed. Синк: Роберт Нагуманов x Саша Абаев
    let isViewed: Bool                 // просмотрен ли слайд
    let contentType: String            // enum("image", "video")
    let content: Content<Image/Video>  // контент (изображение или видео)
    let button: Button?                // опциональная кнопка действия (макс. 1 на слайд)
    let markingInfo: MarkingInfo?      // опциональная маркировка рекламы,
    let statistics: [Statistic],       // пиксели для слайда сторис
}