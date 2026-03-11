# Локализация — Примеры использования

## 1. Статичный текст элемента

Самый простой случай — задать `translationKey` у label или button. Текст будет автоматически подставлен из активного языка:

```json
{
  "type": "label",
  "name": "lbl_Title",
  "styles": ["s_BaseFont", "s_TitleFont"],
  "translationKey": "otp.Title"
}
```

```json
{
  "type": "button",
  "name": "btn_Next",
  "styles": ["s_BaseButton"],
  "translationKey": "Default.NextButton",
  "command": "screen:win_NextStep"
}
```

При смене языка текст обновится без перезагрузки экрана.

---

## 2. Перевод с подстановкой аргументов

Если перевод содержит плейсхолдеры `{0}`, `{1}` и т.д. — их можно заполнить значениями из сессии через `translationArguments`:

**В Translations.xlsx:**

| Ключ | ru | uz |
|---|---|---|
| `cardPrint.Title` | Печатаем карту, {0}! | Kartani chop etamiz, {0}! |
| `otp.Description` | Код отправлен на номер {0} | {0} raqamiga kod yuborildi |

**В JSON:**

```json
{
  "type": "label",
  "name": "clientPrintingLabel",
  "styles": ["s_BaseFont", "s_TitleFont"],
  "translationKey": "cardPrint.Title",
  "customProperties": {
    "translationArguments": "session:cardholder.name"
  }
}
```

```json
{
  "type": "label",
  "name": "HelperLabel",
  "translationKey": "otp.Description",
  "customProperties": {
    "translationArguments": "session:clientPhoneField"
  }
}
```

Несколько аргументов перечисляются через запятую:

```json
"translationArguments": "session:firstName, session:lastName"
```

Соответствует переводу: `"Здравствуйте, {0} {1}!"`.

> **Важно:** аргументы реактивны — если значение `session:clientPhoneField` изменится, текст элемента обновится автоматически.

---

## 3. Динамический ключ из сессии

Если сам ключ перевода не известен заранее и зависит от логики — его можно брать из `SessionData`:

```json
{
  "type": "label",
  "name": "lbl_Status",
  "translationKey": "session:currentStatusKey"
}
```

При изменении `session:currentStatusKey` (например, командой `add:currentStatusKey#error.notFound`) — текст элемента автоматически сменится на перевод нового ключа.

---

## 4. Смена языка командой

Для переключения языка используется команда `language:`:

```json
{
  "type": "button",
  "name": "btn_uz",
  "translationKey": "UZB",
  "command": "language:uz"
}
```

```json
{
  "type": "button",
  "name": "btn_ru",
  "translationKey": "RUS",
  "command": "language:ru"
}
```

**Язык из сессии:**

```json
"command": "language:session:userSelectedLang"
```

**Язык из persistent:**

```json
"command": "language:persistent:savedLang"
```

Выбранный язык автоматически сохраняется в `PersistentData` под ключом `-language-` и остаётся активным на всё время работы приложения.

---

## 5. Переключатель языков (реальный пример)

Типичный overlay с выбором языка:

```json
{
  "windowId": "over_LangAndLogoOverlay",
  "backgroundColor": "Transparent",
  "overlay": { "AutoShow": true, "ZIndex": 150 },
  "elements": [
    {
      "type": "button",
      "name": "btn_uz",
      "styles": ["s_LangButton"],
      "translationKey": "UZB",
      "command": [
        "language:uz"
      ]
    },
    {
      "type": "button",
      "name": "btn_ru",
      "styles": ["s_LangButton"],
      "translationKey": "RUS",
      "command": "language:ru"
    }
  ]
}
```

---

## 6. Использование ключей перевода в командах ошибок

Ключи переводов используются не только в элементах, но и напрямую в командах `error:` и `warning:`:

```json
"onError": "error:error.confirmOtp.title|error.confirmOtp.desc"
```

Формат: `error:ключ_заголовка|ключ_описания`

Оба значения (`error.confirmOtp.title` и `error.confirmOtp.desc`) должны присутствовать в `Translations.xlsx`.

---

## 7. Форматирование текста прямо в переводе

Строки переводов поддерживают HTML-подобную разметку:

| Запись в xlsx | Результат |
|---|---|
| `Нажмите <b>Далее</b>` | Слово «Далее» жирным |
| `<color=#FF6600>Внимание!</color> Карта готова` | «Внимание!» оранжевым |
| `Строка 1\nСтрока 2` | Две строки |
| `<b><color=#FFFFFF>Белый жирный</color></b>` | Вложенные теги |

Пример в xlsx:

| Ключ | ru |
|---|---|
| `cardReady.Title` | Ваша карта <b>готова</b>!\n Заберите её из лотка. |

---

## 8. buttonsToggleGroup — визуальная привязка к языку

`buttonsToggleGroup` позволяет автоматически выделять активную кнопку языка, читая текущее значение из `PersistentData`. Поскольку язык всегда хранится под ключом `-language-`, достаточно привязать группу к нему:

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
    "bindButton1": "btn_uz>uz",
    "bindButton2": "btn_ru>ru",
    "bindButton3": "btn_en>en"
  }
}
```

Когда язык меняется командой `language:uz` — группа автоматически переключает визуальное состояние кнопок без дополнительной логики.
