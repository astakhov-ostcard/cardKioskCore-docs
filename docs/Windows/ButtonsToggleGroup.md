## ButtonsToggleGroup
Группа кнопок-переключателей. Один из вариантов всегда «активен» — выделяется стилем. Синхронизируется с переменной в сессии или persistent.

```json
{
  "type": "buttonsToggleGroup",
  "name": "btg_Language",
  "customProperties": {
    "activeBackgroundColor": "#FFFFFF",
    "inactiveBackgroundColor": "#5A5A5C",
    "activeForegroundColor": "#5A5A5C",
    "inactiveForegroundColor": "#FFFFFF",
    "bindPersistent": "-language-",
    "bindButton1": "btn_kz>kz",
    "bindButton2": "btn_ru>ru",
    "bindButton3": "btn_en>en",
    "bindButton4": "btn_ch>ch"
  }
}
```

Кнопки `btn_kz`, `btn_ru`, `btn_en`, `btn_ch` — это обычные `Button`-элементы на том же экране. `ButtonsToggleGroup` управляет их визуальным состоянием, наблюдая за значением переменной.

### CustomProperties
| Ключ | Тип | Описание |
|---|---|---|
| `activeBackgroundColor` | string | Цвет фона активной (выбранной) кнопки |
| `inactiveBackgroundColor` | string | Цвет фона неактивных кнопок |
| `activeForegroundColor` | string | Цвет текста активной кнопки |
| `inactiveForegroundColor` | string | Цвет текста неактивных кнопок |
| `bindPersistent` | string | Ключ в `persistent`, за которым наблюдает группа. При изменении значения — обновляет визуальное состояние кнопок |
| `bindButton1`, `bindButton2`, ... | string | Привязка кнопки к значению в формате `"elementName>value"`. При совпадении значения переменной с `value` — кнопка становится активной |

> Элемент `ButtonsToggleGroup` не создаёт собственных кнопок — он только управляет стилями существующих кнопок-элементов на экране. Сами кнопки должны быть описаны отдельно в `elements` и самостоятельно менять значение переменной (например, командой `"language:kz"`).
