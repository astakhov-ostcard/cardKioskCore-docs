### Тип: OptController
Контроллер оптических датчиков и лотков через COM-порт.

**Настройки (`settings`):**
| Ключ | Тип | Описание |
|---|---|---|
| `port` | string | COM-порт устройства |
| `timeoutMs` | string | Таймаут ответа в миллисекундах |

```json
{
  "key": "optMain",
  "type": "OptController",
  "settings": {
    "port": "COM4",
    "timeoutMs": "2000"
  },
  "autoStart": false,
  "enabled": true
}
```

**Методы для `auxcall`:**
| Метод | Параметры | Описание |
|---|---|---|
| `sendCom` | `command` | Отправить команду контроллеру. Например: `"TrayLight"` |

Пример вызова:
```json
{
  "name": "auxcall:optMain|sendCom",
  "parameters": {
    "command": "TrayLight"
  }
}
```