## Button

Кнопка с текстом и рамкой. При нажатии выполняет команду.

```json
{
  "type": "Button",
  "name": "btn_confirm",
  "margin": { "left": 100, "bottom": 200, "right": 100 },
  "size": { "height": 100 },
  "translationKey": "ConfirmButton",
  "textStyle": {
    "fontSize": 28,
    "foreground": "#FFFFFF",
    "textAlignment": "Center"
  },
  "border": {
    "background": "#2563EB",
    "cornerRadius": 16
  },
  "command": "screen:win_Next"
}
```

### Параметры

| Параметр | Тип | Описание |
|---|---|---|
| `translationKey` | string | Ключ перевода — текст кнопки. Поддерживает `session:` и `persistent:` |
| `textStyle` | TextStyle | Стиль текста кнопки |
| `border` | BorderOptions | Фон, рамка и скругление кнопки. Если не задан — применяется тёмно-синий фон со скруглением 10px |
| `onClick` | string | Строковая команда, выполняемая при нажатии |
| `command` | CommandDefinition или string | Команда при нажатии. Поддерживает объектную форму с `onSuccess`/`onFailure` |
| `clickThrough` | bool | Если `true` — клик передаётся также элементу позади кнопки |

> Если заданы одновременно `onClick` и `command` — выполняются оба, сначала `onClick`.
