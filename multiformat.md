struct AdResponse {
    let padId: Int,
    let templateGroupName: "multiformat",
    let templateName: String,
    let displayOptions: DisplayOptions, // опции, описывающие UI конфигруацию для отображения items в контейнере
    let items: [Creative],
    let statistics: [Statistic]         // пиксели для всего баннерного блока
}


DisplayOptions:

общие UI конфиги для всего баннерного блока
struct DisplayOptions {
    let size: AspectRatio        // смотрим соотношение сторон слайдов
    let useAutoScroll: true,
    let autoScrollTimeout: Int?, // время между автоперелистыванием слайдов
    let useHeader: Bool          // используем ли хедер над креативами. Если используется, то в AdResponse смотрим модель header
    let cornerRadius: Int        // радиус скругления слайдов
    let earsWidth: Int           // ширина ушей (видимых частей боковых баннеров)
    let interItemSpacing: Int    // отступы между слайдами
    let format: "carousel"       // enum: carousel, MxN (конкретные примеры: 1x1, 2x2, 2x1, 3x3)
}


Модель слайдов:

struct Creative {
    let id: String                // уникальный идентификатор рекламного объявления
    let integratonType: String,   // описывает способ интеграции: enum("sdk", "native")
    let statistics: [Statistic]   // пиксели для слайда
    let contentType: "image",
    let content: Image / Video,
    let markingInfo: MarkingInfo
    let link: Link                // ссылка в слайде
}