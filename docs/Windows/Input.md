## Input Field

Поле ввода текста с экранной клавиатурой. Сохраняет введённое значение в сессию по имени элемента.

```json
{
  "type": "InputField",
  "name": "input_phone",
  "margin": { "left": 80, "top": 800, "right": 80 },
  "size": { "height": 120 },
  "input": {
    "inputType": "classic",
    "length": 11,
    "inputMask": "[0-9]",
    "keyboard": "numeric",
    "autoActivate": true
  },
  "command": {
    "name": "apicall:CheckPhone",
    "parameters": { "phone": "session:input_phone" },
    "onSuccess": "screen:win_Next",
    "onFailure": "error:PhoneNotFound"
  }
}
```

После завершения ввода значение автоматически сохраняется в `session:{name}` (в примере выше — `session:input_phone`).

### Параметры `input`

| Параметр | Тип | Описание |
|---|---|---|
| `inputType` | string | Визуальный тип поля: `"classic"` — классические ячейки, `"centered"` — центрированные, `"plain"` — обычная строка |
| `length` | int | Максимальное количество символов. Игнорируется если задан `inputTemplate` |
| `minimalLength` | int | Минимальное количество символов для срабатывания события `onMinimalEntered` |
| `inputMask` | string | Regex-фильтр допустимых символов. Пример: `"[0-9]"` — только цифры, `"[a-zA-Z]"` — только латиница |
| `inputTemplate` | string | Шаблон с фиксированными символами. `n` — позиция ввода. Пример: `"+7 (nnn) nnn-nn-nn"` |
| `keyboard` | string | Тип клавиатуры: `"numeric"`, `"numericbcc"`, `"numericlatin"`, `"numericlatinExtended"`, `"universal"`. По умолчанию — клавиатура экрана |
| `autoActivate` | bool | Если `true` — поле активируется и клавиатура открывается автоматически при загрузке экрана |
| `maskCharacter` | string | Символ-заглушка для скрытия введённых символов. Например: `"●"` |
| `mode` | string | Режим работы: `"fixed"` или `"variable"` |
| `events` | array | Список обработчиков событий поля. Описание ниже |

### Параметры внешнего вида клавиатуры

Все параметры опциональны. Применяются к кнопкам экранной клавиатуры.

| Параметр | Тип | Описание |
|---|---|---|
| `keyWidth` | int | Ширина кнопки клавиатуры |
| `keyHeight` | int | Высота кнопки клавиатуры |
| `keyCornerRadius` | int | Скругление кнопок |
| `keyBackground` | string | Цвет фона кнопок |
| `keyForeground` | string | Цвет текста на кнопках |
| `keyFontSize` | int | Размер шрифта на кнопках |
| `keyMargin` | int | Отступ между кнопками |
| `keyContentMargin` | int | Внутренний отступ текста внутри кнопок |
| `keyboardOffset` | int | Смещение клавиатуры вверх от нижнего края экрана в пикселях |
| `inputStringCharacterSpace` | int | Расстояние между ячейками символов в поле ввода |
| `keys` | array | Кастомная раскладка клавиатуры — массив объектов `{ "label": "1", "value": "1" }` |
| `rowsOffsets` | int[] | Горизонтальные смещения строк клавиатуры в пикселях |

### CustomProperties

| Ключ | Тип | Описание |
|---|---|---|
| `keyboardOffset` | string | Альтернативный способ задать смещение клавиатуры (число в виде строки). Применяется вместо параметра `input.keyboardOffset` |

### События поля ввода

Задаются в массиве `input.events`. Каждый элемент:
```json
{ "event": "onComplete", "command": "screen:win_Next" }
```

| Событие | Описание |
|---|---|
| `onInputStart` | Первый символ введён |
| `onInputChanged` | Любое изменение ввода (каждый символ) |
| `onComplete` | Достигнута максимальная длина |
| `onErase` | Поле полностью очищено |
| `onMinimalEntered` | Введено не менее `minimalLength` символов |
| `onMinimalNotEntered` | При попытке завершить ввод введено меньше `minimalLength` |

### Параметр `command`

Выполняется при срабатывании `onComplete` (дополнительно к событию из `input.events`). Поддерживает полную форму `CommandDefinition`:
```json
"command": {
  "name": "apicall:VerifyCode",
  "parameters": { "code": "session:input_otp" },
  "onSuccess": "screen:win_Success",
  "onFailure": "error:WrongCode"
}
```

### Параметр `masked`

| Параметр | Тип | Описание |
|---|---|---|
| `masked` | bool | Если `true` — значение поля скрывается в логах. Не влияет на визуальное отображение (для маски символов используйте `input.maskCharacter`) |
