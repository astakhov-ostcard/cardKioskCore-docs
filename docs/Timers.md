## FILE: Timers.md

## Timers
Таймеры выполняют команды с задержкой или по расписанию. Управляются командами `timer.start`, `timer.stop`, `timer.reset`.

```json
{
  "timers": [
    {
      "name": "timer_StatusPoller",
      "intervalMs": 2000,
      "autoFire": false,
      "fireAtStart": true,
      "repeat": true,
      "onComplete": {
        "name": "apicall:get_CheckStatus",
        "onSuccess": "screen:win_Done",
        "onFailure": "error:PollingError"
      }
    }
  ]
}
```

### Параметры таймера
| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `name` | string | — | Уникальное имя таймера. Используется в командах: `timer.start:name` |
| `intervalMs` | int | — | Интервал в миллисекундах между выполнениями |
| `autoFire` | bool | `false` | Если `true` — таймер запускается автоматически при старте приложения |
| `fireAtStart` | bool | `false` | Если `true` — команда `onComplete` выполняется немедленно при запуске, не дожидаясь первого интервала |
| `repeat` | bool | `false` | Если `true` — таймер повторяется бесконечно с заданным интервалом. Если `false` — выполняется один раз |
| `onComplete` | string, object или array | — | Команда, выполняемая по истечении интервала |
| `onError` | string, object или array | — | Команда при ошибке выполнения |
| `onFailed` | string, object или array | — | Команда при неудачном результате |

### Управление таймерами
```json
"timer.start:timer_StatusPoller"
"timer.stop:timer_StatusPoller"
"timer.reset:timer_StatusPoller"
"timers.allstop"
```

**`timer.reset`** — сбрасывает счётчик таймера в начало, не останавливая его. Если таймер не был запущен — не выполняет никаких действий.

### Типичные сценарии использования

**Периодический опрос статуса:**
```json
{
  "name": "timer_Poller",
  "intervalMs": 2000,
  "autoFire": false,
  "fireAtStart": true,
  "repeat": true,
  "onComplete": {
    "name": "apicall:get_CheckStatus",
    "onSuccess": "screen:win_Done"
  }
}
```
Запускается командой `timer.start:timer_Poller`, останавливается `timer.stop:timer_Poller`.

**Одноразовая задержка:**
```json
{
  "name": "timer_Delay",
  "intervalMs": 3000,
  "autoFire": false,
  "fireAtStart": false,
  "repeat": false,
  "onComplete": "screen:win_Next"
}
```

**Таймер предупреждения о бездействии:**
```json
{
  "name": "time_ReturnToRootWarning",
  "intervalMs": 30000,
  "autoFire": false,
  "fireAtStart": false,
  "repeat": false,
  "onComplete": "screen:win_ReturnToRootWarning"
}
```