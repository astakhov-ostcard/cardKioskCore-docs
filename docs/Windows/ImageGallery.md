## Image Gallery

Галерея изображений с переключением через кнопки Назад/Вперёд. Текущий выбранный ключ сохраняется в сессию.

```json
{
  "type": "ImageGallery",
  "name": "gallery_products",
  "customProperties": {
    "prevButton": "btn_prev",
    "nextButton": "btn_next",
    "image0": "img_center",
    "item0": "visa>cards/visa.png",
    "item1": "mastercard>cards/mastercard.png",
    "item2": "mir>cards/mir.png",
    "defaultKey": "visa",
    "onChanged": "screen:win_ProductDetail"
  }
}
```

После переключения текущий ключ сохраняется в `session:{name}.currentItem` (в примере — `session:gallery_products.currentItem`).

### CustomProperties

| Ключ | Тип | Описание |
|---|---|---|
| `prevButton` | string | Имя (`name`) кнопки-элемента для перехода к предыдущему пункту |
| `nextButton` | string | Имя (`name`) кнопки-элемента для перехода к следующему пункту |
| `image0`, `image1`, ... | string | Имена (`name`) Image-элементов, в которых отображаются варианты. Центральное изображение — среднее в списке. Остальные показывают соседние пункты |
| `item0`, `item1`, ... | string | Пункты галереи в формате `"ключ>путь_к_картинке"`. Путь разрешается от папки `Images/` ресурсов |
| `defaultKey` | string | Ключ пункта, выбранного при инициализации |
| `onChanged` | string или CommandDefinition | Команда, выполняемая при каждом переключении |
