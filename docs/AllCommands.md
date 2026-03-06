# Команды (Commands)

Команды — это строки или объекты, которые описывают действие, которое должна выполнить система. Они используются в полях `onComplete`, `onSuccess`, `onFailure`, `onError`, `OnClick`, `OnOpened` и других событиях.

Команды бывают двух видов:
1. **Строковые команды** — простая строка вида `"screen:win_Main"`. Описаны на этой странице.
2. **Объектные команды** — объект `CommandDefinition` для вызова API и внешних устройств. Описаны в разделе [Apis](Apis) и [Auxilary](Auxilary).

---

## Навигация

### Перейти на экран
```json
"screen:windowId"
```
Открывает экран с указанным идентификатором. Предыдущий экран сохраняется в истории.

---

### Вернуться назад
```json
"back"
```
Закрывает текущий экран и возвращается к предыдущему. Если текущий экран — корневой, команда игнорируется.

---

### Вернуться на корневой экран
```json
"root"
```
Закрывает все экраны в истории и переходит на корневой экран. Также сбрасывает сессию пользователя.

---

## Сообщения об ошибках и предупреждениях

### Показать предупреждение
```json
"warning:WarningTextKey"
"warning:WarningTextKey|DescriptionKey"
"warning:WarningTextKey|DescriptionKey|win_CustomWarning"
```
Сохраняет текст в `persistent:WarningText` и `persistent:WarningDescription`, затем открывает экран предупреждения.  
По умолчанию используется экран `win_Warning`. Третьим параметром можно указать произвольный экран.

---

### Показать ошибку
```json
"error:ErrorTextKey"
"error:ErrorTextKey|DescriptionKey"
"error:ErrorTextKey|DescriptionKey|win_CustomError"
```
Сохраняет текст в `persistent:ErrorText` и `persistent:ErrorDescription`, затем открывает экран ошибки.  
По умолчанию используется экран `win_Error`. Третьим параметром можно указать произвольный экран.

---

## Управление элементами

### Показать элемент
```json
"element:show:elementName"
```

### Скрыть элемент
```json
"element:hide:elementName"
```

### Переключить видимость элемента
```json
"element:toggleshow:elementName"
```
Если элемент виден — скрывает его, и наоборот.

---

### Активировать элемент
```json
"element:activate:elementName"
```
Устанавливает `IsEnabled = true`. Используется, например, для разблокировки кнопок.

### Деактивировать элемент
```json
"element:deactivate:elementName"
```
Устанавливает `IsEnabled = false`.

### Переключить активацию элемента
```json
"element:toggleactivate:elementName"
```

---

### Применить стиль текста к элементу
```json
"element:setTextStyle:elementName#styleName"
```
Применяет текстовый стиль из раздела [Styles](Styles) к указанному элементу (Label или InputField).

### Сбросить стиль текста элемента
```json
"element:resetTextStyle:elementName"
```
Возвращает элементу исходный стиль, заданный в его определении.

---

### Передать фокус на поле ввода
```json
"focus:elementName"
```
Передаёт фокус на InputField и открывает клавиатуру по умолчанию.

---

## Работа с переменными

Подробнее об операциях соединения, разделения строк и порядке выполнения — в разделе [Операции с переменными](VariablesFunctions).

### Записать значение в сессию
```json
"add:varName"
"add:varName#value"
```
Если указан только ключ — записывает `true`.  
Если указано значение — разрешает его (поддерживает конкатенацию `&&` и Split) и записывает результат в `session:varName`.  
Всегда перезаписывает существующее значение.

---

### Записать в сессию (только если пусто)
```json
"fill:varName"
"fill:varName#value"
```
Аналог `add:`, но записывает значение только если переменная ещё не существует в сессии.

---

### Удалить переменную из сессии
```json
"remove:varName"
```

---

### Записать в persistent (только если пусто)
```json
"persistentfill:varName"
"persistentfill:varName#value"
```
Аналог `fill:`, но работает с постоянным хранилищем `PersistentData`.

---

### Удалить переменную из persistent
```json
"persistentremove:varName"
```

---

### Скопировать значение в сессию
```json
"pushSession:sourceKey>targetKey"
```
Копирует значение переменной `sourceKey` (ищет сначала в Session, затем в Persistent) в `session:targetKey`.

---

### Скопировать значение в persistent
```json
"pushPersistent:sourceKey>targetKey"
```
Копирует значение переменной `sourceKey` в `persistent:targetKey`.

---

## Буфер обмена

### Скопировать текст в буфер
```json
"copyFrom:elementName"
"copyFrom:session:varName"
"copyFrom:persistent:varName"
```
Копирует текст из InputField или Label в буфер обмена. Также поддерживает прямое чтение из переменных.

---

### Вставить текст из буфера
```json
"pasteTo:elementName"
```
Вставляет текст из буфера в InputField или Label. Максимальная длина — 1000 символов.

---

## Язык интерфейса

### Сменить язык
```json
"language:ru"
"language:session:selectedLanguage"
"language:persistent:selectedLanguage"
```
Устанавливает язык интерфейса. Ключ языка должен совпадать с колонкой в файле `Translations.xlsx`.  
Поддерживает прямое указание кода языка, а также чтение из Session и Persistent.

---

## Таймеры

Подробнее о структуре таймеров — в разделе [Timers](Timers).

### Запустить таймер
```json
"timer.start:timerName"
```

### Остановить таймер
```json
"timer.stop:timerName"
```

### Перезапустить таймер
```json
"timer.reset:timerName"
```

### Остановить все таймеры
```json
"timers.allstop"
```

---

## Таймер бездействия

Таймер бездействия отслеживает активность пользователя (нажатия, движения мыши). При истечении времени выполняет заданную команду.

### Запустить
```json
"iTimer:start"
```
Запускает таймер. Не запускается, если текущий экран — корневой или экран загрузки.

### Остановить
```json
"iTimer:stop"
```

### Перезапустить
```json
"iTimer:restart"
```

### Задать команду при срабатывании
```json
"iTimer:command#screen:win_Standby"
```
По умолчанию: `"screen:win_StandbyTimerWarning"`.

### Задать таймаут (в секундах)
```json
"iTimer:timeout#120"
```
По умолчанию: 180 секунд.

---

## Оверлеи

Оверлей — это экран, который отображается поверх основного контента. Настраивается в `WindowDefinition.Overlay`.

### Показать оверлей
```json
"showOverlay:overlayWindowId"
```

### Скрыть оверлей
```json
"hideOverlay:overlayWindowId"
```

### Переключить оверлей
```json
"toggleOverlay:overlayWindowId"
```

---

## Вызов устройств (Auxilary)

### Простой вызов (без параметров и обработки результата)
```json
"auxcall:deviceKey.methodName"
```
Выполняется асинхронно, без ожидания результата. Используется внутри строковых команд.

### Расширенный вызов (с параметрами и ветвлением)
Используется внутри объектной команды `CommandDefinition`:
```json
{
  "name": "auxcall:deviceKey|methodName",
  "parameters": { "param1": "value1" },
  "saveAs": "session:resultVar",
  "onSuccess": "screen:win_Next",
  "onFailure": "screen:win_Error"
}
```
Если устройство возвращает `bool` — выполняется `onSuccess` или `onFailure`.  
Если возвращает любое другое значение — всегда `onSuccess`. Результат сохраняется в переменную `saveAs`.  
Подробнее — в разделе [Auxilary](Auxilary).

---

## Вызов API

Используется только в виде объектной команды `CommandDefinition`:
```json
{
  "name": "apicall:requestName",
  "parameters": { "param1": "session:varName" },
  "onSuccess": "screen:win_Next",
  "onFailure": "error:RequestFailed"
}
```
Для асинхронного вызова (без блокировки потока):
```json
{
  "name": "apicallasync:requestName",
  ...
}
```
Подробнее о настройке запросов — в разделе [Apis](Apis).

---

## Выполнение нескольких команд

### Список команд в одной строке
```json
"[screen:win_Next, timer.stop:mainTimer, remove:userData]"
```
Команды выполняются последовательно.

---

### Список команд в массиве
```json
[
  "remove:userData",
  "timer.stop:mainTimer",
  "screen:win_Next"
]
```
Поддерживается в полях `onSuccess`, `onFailure`, `onError`, `OnOpened` и других.

---

## Отмена асинхронных операций

### Отменить операцию по токену
```json
"cancel:tokenName"
```
Отменяет конкретную асинхронную операцию, связанную с токеном отмены.

### Отменить все операции
```json
"cancelAll"
```

---

## Скрытая маска поля ввода

### Переключить маскировку символов
```json
"togglemask:elementName"
```
Переключает отображение символов в InputField между открытым и скрытым видом (например, для паролей).

---

## Вызов зарегистрированной команды

```json
"command:registeredCommandName"
```
Запускает команду, зарегистрированную в `CommandRegistry` через код. Используется для вызова встроенных системных операций (например, `ParseHopperState`).
