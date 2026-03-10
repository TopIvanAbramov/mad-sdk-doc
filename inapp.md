struct AdResponse {
    let padId: Int,
    let templateGroupName: "modalwindow", // в будущем появится фулскрин: fulscreen
    let templateName: String,
    let displayOptions: DisplayOptions,  // опции, описывающие UI конфигруацию
    let items: [Creative],               // массив модалок
    let statistics: [Statistic]          // пиксели для всего блока сторис
}


DisplayOptions:

общие UI конфиги для всех модалок

struct DisplayOptions {
    let modalWindowCornerRadius: Int  // радиус скругления верха модалки
    let backgroundColor: String       // цвет заднего фона
}


Модель модалки:

struct Creative {
    let id: String                // уникальный идентификатор рекламного объявления
    let integratonType: String,   // описывает способ интеграции: enum("sdk", "native")
    let statistics: [Statistic]   // пиксели для модалки
    let contentType: "image",     // пока только картинка: видео будет в будущем
    let content: Image / Video,
    let markingInfo: MarkingInfo
    let title: String?            // заголовок: может отсутствовать
    let subtitle: String?         // подзаголовок: может отсутствовать
    let button: Button?           // кнопка может отсутствовать
}