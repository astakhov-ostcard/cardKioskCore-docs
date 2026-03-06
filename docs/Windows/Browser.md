## Browser

Встроенный веб-браузер на базе CefSharp (Chromium). Отображает веб-страницу или локальный HTML.

```json
{
  "type": "Browser",
  "name": "browser_payment",
  "margin": { "left": 0, "top": 0, "right": 0, "bottom": 0 },
  "customProperties": {
    "source": "session:paymentPageUrl",
    "bindableObjectName": "kioskBridge"
  }
}
```

### CustomProperties

| Ключ | Тип | Описание |
|---|---|---|
| `source` | string | URL страницы или локального HTML-файла. Поддерживает `session:varName` и `persistent:varName` — браузер загружает актуальный адрес при создании. Если значение не найдено — загружается `about:blank` |
| `bindableObjectName` | string | Имя JavaScript-объекта, который будет зарегистрирован в браузере через `JavascriptObjectRepository`. Позволяет вызывать методы C# из JavaScript. Поддерживает `session:varName` и `persistent:varName` |

**Особенности:**
- Контекстное меню (правая кнопка мыши) отключено
- Весь ввод с физической клавиатуры заблокирован (кроме F12 — открывает DevTools)
- При скрытии элемента браузер уничтожается, при показе — создаётся заново (lazy lifecycle)
- Сообщения из консоли браузера выводятся в лог приложения
