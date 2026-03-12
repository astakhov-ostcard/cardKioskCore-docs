## Apis
Раздел `apis` описывает HTTP-запросы к внешним или внутренним REST API. Запросы вызываются командой `apicall:requestKey`.

```json
{
  "apis": [
    {
      "Name": "BankAPI",
      "BaseUrl": "https://api.example.com",
      "DefaultHeaders": {
        "Accept": "application/json",
        "Accept-Language": "ru"
      },
      "Auth": {
        "Type": "BearerToken",
        "BearerToken": "STATIC_BEARER"
      },
      "Requests": [
        {
          "Key": "post_StartProcess",
          "Summary": "Start Card Print Process",
          "Method": "POST",
          "PathTemplate": "/api/v1/process/start",
          "BodyKind": "Json",
          "BodyTemplate": {
            "clientIin": "session:clientIDField",
            "clientPhone": "session:clientPhoneField"
          },
          "ExpectedStatusCodes": [200],
          "DefaultSuccessSaveAs": "result_post_StartProcess",
          "CodeOutcomeMappings": {
            "404": "warning:ClientNotFound"
          }
        }
      ]
    }
  ]
}
```

### Параметры API (верхний уровень)
| Параметр | Тип | Обязательный | Описание |
|---|---|---|---|
| `Name` | string | ✓ | Уникальное имя API. Используется только для идентификации |
| `Description` | string | | Краткое описание назначения API |
| `Version` | string | | Версия API (например, `"1.0.0"`) |
| `BaseUrl` | string | ✓ | Базовый URL для всех запросов этого API |
| `DefaultHeaders` | object | | HTTP-заголовки, применяемые ко всем запросам этого API |
| `DefaultQuery` | object | | Query-параметры по умолчанию, добавляемые ко всем запросам |
| `Auth` | AuthConfig | | Конфигурация аутентификации |
| `Requests` | Request[] | ✓ | Список запросов |

### Параметры AuthConfig
| Параметр | Тип | Описание |
|---|---|---|
| `Type` | string | Тип аутентификации. На данный момент поддерживается `"BearerToken"` |
| `BearerToken` | string | Значение токена. Добавляется в заголовок `Authorization: Bearer <token>` |

### Параметры Request
| Параметр | Тип | Обязательный | Описание |
|---|---|---|---|
| `Key` | string | ✓ | Уникальный ключ запроса. Используется в команде `apicall:Key` |
| `Summary` | string | | Краткое описание запроса |
| `Method` | string | ✓ | HTTP-метод: `"GET"`, `"POST"`, `"PUT"`, `"DELETE"` и т.д. |
| `PathTemplate` | string | ✓ | Путь запроса. Поддерживает переменные `{param}`, которые заменяются из `Query` |
| `BodyKind` | string | | Тип тела запроса: `"Json"` или `"None"`. Требуется при наличии `BodyTemplate` |
| `Query` | object | | Query-параметры запроса. Значения поддерживают `session:` и `persistent:` |
| `BodyTemplate` | object | | Тело запроса. Значения поддерживают `session:varName` и `persistent:varName` с опциональным `\|fallback:defaultValue` |
| `ExpectedStatusCodes` | int[] | | Список HTTP-кодов, считающихся успешным ответом. Ответы с другими кодами вызывают `onFailure`. Если не задан — любой 2xx код считается успехом |
| `DefaultSuccessSaveAs` | string | | Ключ сессии (`session:key`), в который сохраняется тело успешного ответа |
| `CodeOutcomeMappings` | object | | Сопоставление конкретных HTTP-кодов с командами. Переопределяет стандартное поведение для указанных кодов |
| `TimeoutOverride` | string | | Переопределение таймаута запроса в формате `"ЧЧ:ММ:СС"`. Например: `"00:02:00"` |

### CodeOutcomeMappings
Позволяет назначить команду или цепочку команд для конкретного HTTP-кода ответа.

```json
"CodeOutcomeMappings": {
  "404": "warning:ClientNotFound",
  "408": {
    "name": "apicall:get_WaitForCardIssuedRetry",
    "onSuccess": "screen:win_Finish",
    "onFailure": "error:TimeoutError"
  }
}
```

Значение для каждого кода — это команда: строка, объект `CommandDefinition` или массив команд.

### Переменные в Query и BodyTemplate
Значения в `Query` и `BodyTemplate` поддерживают:
- `"session:varName"` — значение из сессии
- `"persistent:varName"` — значение из персистентного хранилища
- `"session:varName|fallback:defaultValue"` — со значением по умолчанию
- Статические значения: `"60"`, `true`, `false`, числа

### Путь с переменными (PathTemplate)
Переменные в пути задаются в фигурных скобках и берутся из `Query`:
```json
{
  "Key": "get_CheckStatus",
  "Method": "GET",
  "PathTemplate": "/api/v1/process/{id}/check-status",
  "Query": {
    "id": "session:processId"
  }
}
```

### Вызов запросов
Запросы вызываются командами:
```json
"apicall:post_StartProcess"
```
Или в объектной форме с обработчиками:
```json
{
  "name": "apicall:post_StartProcess",
  "onSuccess": "screen:win_Next",
  "onFailure": "error:ServerError"
}
```
Для асинхронного вызова (без блокировки потока UI):
```json
{
  "name": "apicallasync:post_StartProcess",
  "onSuccess": "screen:win_Next"
}
```

Результат успешного запроса сохраняется в `session:{DefaultSuccessSaveAs}`. Из него можно читать значения через точечную нотацию:
```json
"session:result_post_StartProcess.data.id"
```