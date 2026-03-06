# Windows

Массив windows сождерит в себе экраны (окна). Экран - это совокупность всех визуальных элементов, которые видит пользователь в определенный момент. 

![Group 16.png](C:\Users\astak\Downloads\Group%2016.png)

рис. 1 - Пользователь при нажатии кнопок переходит с экрана заставки к экрану выбора действия, затем к экрану выбора услуг 

## Структура кода экрана

---

## Список доступных элементов

[Label](Label)

[Image](Image)

[Button](Button)

[Input Field](InputField)

[Border](Border)

[Video](Video)

[QR](QR)

[PDF Viewer](PdfViewer)

[Image gallery](ImageGallery)


## Общие параметры

Эти параметры доступны у всех элементов.

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `type` | string | — | Тип элемента. Обязательный. Например: `"Label"`, `"Button"`, `"Image"` |
| `name` | string | — | Уникальный идентификатор элемента на экране. Используется для управления через команды `element:show:name`, `element:hide:name` и т.д. |
| `visible` | bool | `true` | Видимость элемента при загрузке экрана |

### Позиционирование

Элемент можно позиционировать двумя способами: через `margin` (рекомендуется) или через `position` (устаревший способ).

**Через `margin`** — гибкое позиционирование с поддержкой растяжения:

| Параметр | Тип | Описание |
|---|---|---|
| `margin.left` | double | Отступ от левого края |
| `margin.top` | double | Отступ от верхнего края |
| `margin.right` | double | Отступ от правого края |
| `margin.bottom` | double | Отступ от нижнего края |

Логика работы margin:
- Если указаны одновременно `left` и `right` — элемент растягивается по горизонтали (ширина из `size` игнорируется)
- Если указаны одновременно `top` и `bottom` — элемент растягивается по вертикали (высота из `size` игнорируется)
- Если указан только `left` — элемент прижимается к левому краю
- Если указан только `right` — элемент прижимается к правому краю
- Если не указан ни `left`, ни `right` — элемент центрируется горизонтально

**Через `position`** (устаревший, canvas-подобный способ):

| Параметр | Тип | Описание |
|---|---|---|
| `position.x` | double | Отступ от левого края экрана в пикселях |
| `position.y` | double | Отступ от верхнего края экрана в пикселях |

**Размер:**

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `size.width` | double | `100` | Ширина элемента в пикселях |
| `size.height` | double | `100` | Высота элемента в пикселях |

### TextStyle

Параметры отображения текста. Применяются к Label, Button, InputField.

| Параметр | Тип | Описание |
|---|---|---|
| `textStyle.fontFamily` | string | Название шрифта, например `"Roboto"` |
| `textStyle.fontSize` | double | Размер шрифта в пикселях |
| `textStyle.fontWeight` | string | Жирность: `"Normal"`, `"Bold"`, `"Light"` и др. |
| `textStyle.fontStyle` | string | Начертание: `"Normal"`, `"Italic"` |
| `textStyle.foreground` | string | Цвет текста. Поддерживает HEX (`#FFFFFF`), названия цветов |
| `textStyle.textAlignment` | string | Выравнивание текста: `"Left"`, `"Center"`, `"Right"`, `"Justify"` |
| `textStyle.textWrapping` | bool | `true` — перенос строк, `false` — в одну строку |
| `textStyle.textDecorations` | string[] | Массив: `["Underline"]`, `["Strikethrough"]` |
| `textStyle.horizontalAlignment` | string | Горизонтальное выравнивание блока: `"Left"`, `"Center"`, `"Right"`, `"Stretch"` |
| `textStyle.verticalAlignment` | string | Вертикальное выравнивание блока: `"Top"`, `"Center"`, `"Bottom"`, `"Stretch"` |
| `textStyle.padding` | MarginDefinition | Внутренние отступы текста (left, top, right, bottom) |

### BorderOptions

Параметры рамки и фона. Применяются к Border и Button.

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `border.background` | string | — | Цвет фона (HEX или название) |
| `border.borderBrush` | string | — | Цвет рамки |
| `border.borderThickness` | double | `0` | Толщина рамки в пикселях |
| `border.cornerRadius` | double | `0` | Скругление углов в пикселях |
| `border.horizontalAlignment` | string | — | Горизонтальное выравнивание внутри контейнера |
| `border.verticalAlignment` | string | — | Вертикальное выравнивание внутри контейнера |
| `border.padding` | MarginDefinition | — | Внутренние отступы (left, top, right, bottom) |

### Разметка текста

В полях `translationKey` (и в переводах, которые они ссылаются) поддерживаются html-подобные теги для форматирования:

| Тег | Описание |
|---|---|
| `<b>текст</b>` | Жирный текст |
| `<i>текст</i>` | Курсив |
| `<u>текст</u>` | Подчёркивание |
| `<color=#FF0000>текст</color>` | Цвет текста. Поддерживаются HEX и названия цветов |
| `\n` | Перенос строки |

Теги можно вкладывать друг в друга: `<b><color=#FF0000>красный жирный</color></b>`