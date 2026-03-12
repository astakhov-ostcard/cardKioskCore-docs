## Auxilary
Раздел `auxilary` описывает подключаемые физические устройства (QR-сканеры, COM-контроллеры, камеры). Устройства управляются командой `auxcall:deviceKey.methodName`.

```json
{
  "auxilary": [
    {
      "key": "qrReader",
      "type": "QrReader",
      "settings": {
        "port": "COM3"
      },
      "autoStart": false,
      "enabled": true,
      "events": {
        "onTriggered": {
          "saveAs": "qrdata",
          "route": "screen:win_QrResult"
        }
      }
    }
  ]
}
```

### Общие параметры устройства
| Параметр | Тип | Обязательный | Описание |
|---|---|---|---|
| `key` | string | ✓ | Уникальный идентификатор устройства. Используется в командах `auxcall:key.method` |
| `type` | string | ✓ | Тип устройства. Определяет класс-обёртку и доступные методы |
| `settings` | object | | Настройки устройства (зависят от типа) |
| `autoStart` | bool | | Если `true` — устройство инициализируется и запускается автоматически при старте приложения |
| `enabled` | bool | ✓ | Если `false` — устройство полностью игнорируется системой |
| `events` | object | | Обработчики событий устройства |
