# Структура JSON-файлов проекта

---

## 1. `appsettings.json` — основная конфигурация приложения

Загружается **первым**, из директории, в которой находится **KardKioskCore.exe**

Пример appsettings.json:
```json
{
  "DesignFile": "TestUI.json",
  "ResourcesFolder": "Resources",
  "DefaultEnvironments": ["test", "ipSet1"],
  "WindowMode": false,
  "DefaultWidth": 1080,
  "DefaultHeight": 1920,
  "DisableEdgeProtection": false
}
```

| Поле | Назначение | По умолчанию | Обязательное |
|---|---|---|---|
| `DesignFile` | Путь к единому `.json`-файлу дизайна. Это устаревшая технология, оставленная для обратной совместимости со старыми картоматами | `"TestUI.json"` | Нет |
| `ResourcesFolder` | Путь к папке проекта (абсолютный или относительный). | `"Resources"` | Нет |
| `DefaultEnvironments` | Список окружений, применяемых при старте | — | Нет|
| `WindowMode` | Оконный режим (не полноэкранный) | `false` | Нет |
| `DefaultWidth` / `DefaultHeight` | Размер окна | 1080×1920 | Нет |
| `DisableEdgeProtection` | Отключить защиту от свайпов по краям | `false` | Нет |

> Если файл не найден — приложение **завершает работу** с ошибкой.

---

## 2. Файловая структура папки проекта (`ResourcesFolder`)

```
ProjectName/
├── Json/
│   ├── _root.json          ← метаданные проекта
│   ├── windows.json        ← экраны и элементы UI
│   ├── styles.json         ← стили
│   ├── apis.json           ← REST-запросы
│   ├── auxilary.json       ← подключаемые устройства
│   └── timers.json         ← таймеры (необязательный)
├── environments.json       ← переопределение значений по среде
├── Images/
├── Videos/
├── Fonts/
└── .pepk                   ← маркер зашифрованного проекта
```

Каждый файл отвечает за свою секцию итогового дизайна картомата:

| Файл | Корневой ключ | Обязательный |
|---|---|---|
| `_root.json` | `name`, `root`, `defaultLanguage`, `defaultKeyboard` | Да |
| `windows.json` | `windows` | Да |
| `styles.json` | `styles` | Да |
| `apis.json` | `apis` | Да |
| `auxilary.json` | `auxilary` | Да |
| `timers.json` | `timers` | Нет |

---

## 3. Описание каждого JSON-файла

### `_root.json`

Метаданные проекта. Определяет начальное состояние приложения.

```json
{
  "name": "PROJECT_NAME",
  "root": "win_Welcome",
  "defaultLanguage": "ru",
  "defaultKeyboard": "numeric"
}
```

| Поле | Назначение |
|---|---|
| `name` | Название проекта (отображается в логах) |
| `root` | `windowId` стартового экрана |
| `defaultLanguage` | Язык по умолчанию (ключ в `Translations.xlsx`) |
| `defaultKeyboard` | Тип клавиатуры по умолчанию: `numeric` / `numericlatin` / `universal` |

---

### `windows.json`

Список экранов и UI-элементов, содержащихся в них. Основной файл визуальной части.

```json
{
  "windows": [ объекты окон ]
}
```

---

### `styles.json`

Именованные наборы параметров отображения. Применяются к элементам через поле `styles`. Подробнее описано в разделе [Styles](Styles.md).

```json
{
  "styles": [ объекты стилей ]
}
```

---

### `apis.json`

Описание REST API-клиентов и их запросов. Подробнее в разделе [APIs](Apis.md)

```json
{
  "apis": [ объекты api ]
}
```
---

### `auxilary.json`

Подключаемые внешние устройства (QR-сканеры, лотки и т.д.). Подробнее в разделе [Auxislary](Auxilary.md)

```json
{
  "auxilary": [ объекты устройств ]
}
```

---

### `timers.json`

Таймеры для отложенного или периодического выполнения команд.

```json
{
  "timers": [ объекты таймеров ]
}
```

> Если файл отсутствует — используется пустой список таймеров, ошибки не возникает.

---

## 4. `environments.json` — переопределение значений по среде

Находится в директории с ресурсами проекта (рядом с `Images/`, `Json/`). Подробнее в разделе [Environments](Environments.md)

Пример файла environments.json:
```json
{
  "params": {
    "myKey": "myValue"
  },
  "environments": [
    {
      "name": "test",
      "objects": {
        "apis[0].BaseUrl": "http://000.000.0.000:00000",
        "windows.[4].elements.[2].visible": true
      }
    },
    {
      "name": "prod",
      "objects": {
        "apis[0].BaseUrl": "http://111.11.1.111:11111",
        "windows.[4].elements.[2].visible": false
      }
    }
  ]
}
```

- Секция `params` → значения автоматически помещаются в `PersistentData` с префиксом `params.*`
- Секция `environments` → патчит итоговый `JObject` по JSON-path нотации
- Применяются только окружения, перечисленные в `appsettings.DefaultEnvironments`

> Если `environments.json` не найден — окружения не применяются, без ошибки.

---

## 5. `Translations.xlsx` — переводы

Подробнее о работе переводов в разделе [Localization](LocalizationBase.md)

### Приоритет поиска файла

```
1. C:\ProgramData\KioskCommon\Translations.xlsx   ← системный (defaultPath)
        ↓ если не найден
2. {ResourcesFolder}\Translations.xlsx            ← рядом с CardKioskCore.exe
        ↓ если не найден
3. Возвращается пустой словарь
   + создаётся error flag "Translations.xlsx not found!"
```

### Формат файла

| *(пусто)* | `ru` | `en` | `uz` |
|---|---|---|---|
| `WelcomeTitle` | Добро пожаловать | Welcome | Xush kelibsiz |
| `AuthBack` | Назад | Back | Orqaga |
| `loaderLabel` | Загрузка... | Loading... | Yuklanmoqda... |

- Первая строка — **языковые ключи** (заголовки колонок)
- Первая колонка — **ключи переводов**

---

## 6. Порядок загрузки при старте

```
[1] appsettings.json
    └── Не найден? → Ошибка запуска

[2] ResourcesFolder (папка)
    ├── .pepk найден?
    │     └── Расшифровать и использовать его вместо json файлов проекта
    └── .pepk не найден?
          └── Загрузить файлы из папки Json

[3] DesignFile (одиночный .json, опционально)
    └── Найден? → DesignFile ПЕРЕЗАПИСЫВАЕТ все данные!

[4] environments.json из ResourcesFolder
    └── Найден? → Перезаписывает указанные в нем данные

[5] Финальная проверка json файлов
    └── Нет windows? → завершение работы с ошибкой

[6] Translations.xlsx
    └── Не найден в C:\ProgramData\KioskCommon\ И не найден в папке ресурсов проекта → завершение работы с ошибкой

[7] KioskSetting.json (системная папка)
    └── Не найден? → сообщение с предупреждением, без завершения работы
```

### Приоритет источников дизайна

| Приоритет | Источник | Примечание |
|---|---|---|
| Низкий | Папка `ResourcesFolder` | Базовый дизайн |
| Средний (перезаписывает данные из `ResourcesFolder`) | `DesignFile` (одиночный `.json`) | Перезаписывает конфликтующие ключи папки `ResourcesFolder` |
| Высокий (перезаписывает все указанные в нем данные) | `environments.json` | Патчит финальный объект дизайна |
