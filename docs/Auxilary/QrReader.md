### Тип: QrReader
QR-сканер, подключённый по COM-порту.

**Настройки (`settings`):**
| Ключ | Тип | Описание |
|---|---|---|
| `port` | string | COM-порт устройства. Например: `"COM3"` |

**Событие `onTriggered`** — срабатывает при успешном сканировании QR-кода:
| Ключ | Тип | Описание |
|---|---|---|
| `saveAs` | string | Ключ сессии, в который сохраняется отсканированное значение |
| `route` | string или CommandDefinition | Команда, выполняемая после сохранения |

```json
{
  "key": "qrReader",
  "type": "QrReader",
  "settings": { "port": "COM3" },
  "autoStart": false,
  "enabled": true,
  "events": {
    "onTriggered": {
      "saveAs": "qrdata",
      "route": {
        "name": "ParseShowcaseQr"
      }
    }
  }
}
```

**Методы для `auxcall`:**
| Метод | Описание |
|---|---|
| `start` | Начать прослушивание COM-порта |
| `stop` | Остановить прослушивание |

---