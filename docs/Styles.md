## Styles
Стили — именованные наборы параметров отображения, которые можно применять к элементам. Позволяют переиспользовать оформление без дублирования кода.

```json
{
  "styles": [
    {
      "name": "s_BaseButton",
      "size": {
        "width": 600,
        "height": 93
      },
      "border": {
        "background": "#2D3436",
        "cornerRadius": 35
      },
      "textStyle": {
        "fontSize": 32,
        "fontFamily": "Montserrat",
        "fontWeight": "Medium",
        "foreground": "#FFFFFF",
        "textAlignment": "Center",
        "verticalAlignment": "Center",
        "TextWrapping": true
      }
    }
  ]
}
```

### Параметры стиля
| Параметр | Тип | Описание |
|---|---|---|
| `name` | string | Уникальное имя стиля. Обязательный. Используется в элементах через `"styles": ["s_BaseButton"]` |
| `textStyle` | TextStyle | Параметры отображения текста. Применяется к Label, Button, InputField |
| `size` | SizeDefinition | Размер элемента (`width`, `height`) |
| `border` | BorderOptions | Параметры рамки и фона. Применяется к Button и Border |
| `margin` | MarginDefinition | Внешние отступы (`left`, `top`, `right`, `bottom`) |

### Применение стилей к элементам
Стили перечисляются в поле `styles` как массив имён. Они применяются последовательно — каждый следующий переопределяет значения предыдущего:

```json
{
  "type": "Label",
  "name": "lbl_Title",
  "styles": ["s_BaseFontWhite", "s_TitleFont"],
  "translationKey": "Welcome.Title"
}
```

Параметры, заданные непосредственно в элементе (`textStyle`, `size`, `border`, `margin`), имеют приоритет над стилями и переопределяют их.

### Переключение стилей текста через команды
К Label и InputField можно применить именованный стиль текста в рантайме:
```json
"element:setTextStyle:elementName#styleName"
```
Сбросить обратно к исходному:
```json
"element:resetTextStyle:elementName"
```

> Поле `styles` в JSON нечувствительно к регистру: `"s_BaseButton"` и `"S_BASEBUTTON"` работают одинаково. Имена стилей рекомендуется начинать с `s_` для удобства поиска.