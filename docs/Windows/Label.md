## Label
Текстовая метка. Отображает переведённый текст с поддержкой динамических аргументов, кликабельных областей и HTML-разметки.

```json
{
  "type": "Label",
  "name": "lbl_Title",
  "styles": ["s_TitleFont"],
  "margin": { "left": 100, "right": 100, "top": 192, "bottom": 1600 },
  "translationKey": "Welcome.Title",
  "textStyle": {
    "fontSize": 48,
    "foreground": "#FFFFFF",
    "textAlignment": "Center"
  }
}
```

### Параметры
| Параметр | Тип | Описание |
|---|---|---|
| `translationKey` | string | Ключ перевода. Поддерживает `session:varName` и `persistent:varName` — текст обновляется автоматически при смене языка или при изменении переменной |
| `textStyle` | TextStyle | Стиль текста. Переопределяет значения из `styles` |
| `styles` | string[] | Массив имён стилей из [Styles](Styles). Применяются последовательно |
| `clickThrough` / `ClickThrough` | bool | Если `true` — клик сквозь метку не перехватывается ею, передаётся элементу позади |

### CustomProperties
| Ключ | Тип | Описание |
|---|---|---|
| `translationArguments` | string | Аргументы для подстановки в строку перевода через format-placeholders. Перечисляются через запятую. Поддерживают `session:varName` — значение обновляется автоматически при изменении переменной |

**Пример с аргументами:**
```json
{
  "type": "Label",
  "name": "lbl_PhoneHint",
  "translationKey": "otp.Description",
  "customProperties": {
    "translationArguments": "session:clientPhoneField"
  }
}
```
Строка перевода: `"Код отправлен на номер {0}"` → подставляется значение `session:clientPhoneField`.

> Label поддерживает html-подобную разметку в значениях перевода: `<b>`, `<i>`, `<u>`, `<color=#HEX>`, `\n`. Подробнее — в разделе [Windows](Windows#Разметка текста).