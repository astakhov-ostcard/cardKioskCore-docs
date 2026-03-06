## PDF Viewer

Отображает PDF-документ во встроенном просмотрщике.

```json
{
  "type": "PdfViewer",
  "name": "pdf_contract",
  "margin": { "left": 40, "top": 200, "right": 40, "bottom": 200 },
  "customProperties": {
    "source": "C:\\Documents\\contract.pdf"
  }
}
```

### CustomProperties

| Ключ | Тип | Описание |
|---|---|---|
| `source` | string | Абсолютный путь к PDF-файлу |
