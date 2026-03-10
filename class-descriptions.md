Общие переиспользуемые модели данных 
Информация о маркировке
struct MarkingInfo {
    let labelText: String                       // текст элемента, ведущего на маркировку: enum("Реклама", "Соц. реклама", ..)
    let orgInfo: String                         // информация о рекламодателе, весь текст от ООО до значения erid
    let url: String                             // ссылка в кнопке
    let buttonText: String                      // Текст на кнопке показывающейся в маркировочной модалке
    let description: String                     // Текст над информацией о рекламодателе: "Помогаем с продвижением ... покупателей"
    let displayOptions: MarkingDisplayOptions   // UI настройки для лейбла маркировки
}
 
struct MarkingDisplayOptions {
   let backgroundColor: String  // цвет заднего фона для лейбла "реклама"
   let textColor: String        // цвет текста
}
Массив статистических пикселей
struct Statistic {
    let url: String,
    let type: String, // тип пикселя из таблицы 
    let value: Int?, //[OPT] описывает время в секундах; может приходить для типов пикселей, имеющих дополнительную параметризацию: viewableImpression, playbackReached, playbackOVVFalse, playbackOVVTrue.
    let pvalue: Int?, //[OPT] описывает время в процентах от duration; может приходить для типов пикселей, имеющих дополнительную параметризацию: viewableImpression, playbackReached, playbackOVVFalse, playbackOVVTrue.
    let viewablePercent: Int? //[OPT] описывает в процентах вхождение в область видимости; может приходить для типов пикселей, имеющих дополнительную параметризацию: impression, viewableImpression, playbackReached, playbackOVVFalse, playbackOVVTrue.
}
AspectRatio (смотрим соотношение сторон)
AspectRatio {
   let width: Int,
   let height: Int
}
Картинка
struct Image {
    let url: String, 
    let size: AspectRatio 
}
Видео
struct Video {
    let url: String,
    let size: AspectRatio,
    let bitrate: Int,
    let format: String // enum("mp4")
    let duration: Int  // длина в миллисекундах
}
Кнопка
struct Button {
    let type: String,               // тип кнопки, несёт бизнес смысл(предназначение). Значения: enum("cta", "customAction", ..)
    let subType: String             // уточняет тип, описывает его конечные UI и функционал. Значние: enum("rich", "promocodeCopy", ..).  Комментарий: rich - уточняет внешний вид UI (богаты внешний вид с икнокой, текстом, анимацией, и кнопкой внутри) и его поведение для type=CTA.
    let text: String,              // текст кнопки (макс. 30 символов)
    let model: ButtonContent       // контент в зависимости от типа
    let displayOptions: ButtonDisplayOptions
}
ButtonDisplayOptions

// UIConfig для настройки кнопок
struct ButtonDisplayOptions {
   let textColor: String          // цвет текста в HEX формате (#RRGGBB)
   let backgroundColor: String    // цвет фона в HEX формате (#RRGGBB)
   let cornerRadius: Int          // скругление кнопки
}


Контент кнопки с диплинком
struct ButtonContent<LinkButton> {
    let link: Link
}
Контент кнопки с промокодом
struct ButtonContent<PromocodeButton> {
    let promocode: String,
    let onCopyText: String, // текст на кнопке, на который нужно поменять после копирования промокода
    let alertText: String?  // текст всплывающего алерта поскле фактического копирования промокода в буфер обмена "Промокод скопирован"
}
Ссылка
struct Link {
    let type: String, // Deeplink, External
    let url: URL, // текст на кнопке, на который нужно поменять после копирования промокода
}
