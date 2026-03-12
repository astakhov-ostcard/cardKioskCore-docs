### Тип: CameraRecorder
Устройство записи видео через USB-камеру.

**Методы для `auxcall`:**
| Метод | Параметры | Описание |
|---|---|---|
| `start` | `sessionName` (опционально) | Начать запись. Имя сессии используется как имя файла. Поддерживает `session:` и `persistent:`. Если не задан — генерируется GUID |
| `stop` | — | Остановить запись |
| `isRecording` | — | Возвращает `true` если запись активна |
| `getCurrentFile` | — | Возвращает путь к текущему файлу записи |

Расширенный вызов (с параметрами и обработчиками):
```json
{
  "name": "auxcall:cameraMain|start",
  "parameters": {
    "sessionName": "session:currentSessionId"
  },
  "onSuccess": "screen:win_Recording",
  "onFailure": "error:CameraError"
}
```

> Разделитель в строковой команде — `.` (точка), в объектной команде — `|` (вертикальная черта).