# Темы

* [Структура темы](#file-structure)
* [Дочерние темы](#child-themes)

<a name="file-structure"></a>
## Структура папки темы

* [theme-name](#struc-theme)
  * [assets](#struc-assets)
    * images
    * scss
    * css
    * js
  * [iblocks](#struc-iblocks)
  * [languages](#struc-languages)
  * [layouts](#struc-layouts)
  * [modules](#struc-modules)
  * [theme.json](#struc-manifest)
  * [page.html](#struc-wrapper)
  * [custom.css](#struc-custom)
  * [custom.js](#struc-custom)
  * [composer.json](#struc-composer)

<a name="struc-theme"></a>
**theme-name** - 

<a name="struc-assets"></a>
**assets** - 

<a name="struc-iblocks"></a>
**iblocks** - 

<a name="struc-languages"></a>
**languages** - 

<a name="struc-layouts"></a>
**layouts** - 

<a name="struc-modules"></a>
**modules** - 

<a name="struc-manifest"></a>
**theme.json** - 

<a name="struc-wrapper"></a>
**page.html** - 

<a name="struc-custom"></a>
**custom.css и custom.js** - 

<a name="struc-composer"></a>
**composer.json** - 


<a name="child-themes"></a>
## Дочерние темы

Дочерняя тема SyDES — это тема, которая расширяет или заменяет функционал другой темы, называемой родительской темой.
Так же это рекомендованый способ модифицирования скачаных тем.

### Для чего нужно использовать дочерние темы?

* Использоование дочерних тем может ускорить разработку, ведь создав один раз родительскую тему вы можете
переиспользовать ее фнкционал, не плодя код
* При обновлении скачаной темы, ваши изменения за пределами custom.css и custom.js будут стерты. Используя дочернюю тему
вы будете уверены в сохранности изменений

### Минимально необходимые файлы

* custom.css - ваши новые стили
* theme.json - с содержимым
  ```json
  {
    "info": {
      "name": "Default's Child Theme",
      "parent": "default"
    }
  }
  ```

Все ассеты, инфоблоки, макеты и данные из theme.json, если не указаны в дочерней, будут браться из родительской темы.

Для того, что бы удалить некоторые ассеты родительской темы, добавьте в theme.json следующие параметры:
```json
{
  "hide_js": ["lightbox"],
  "hide_css": ["lightbox"],
}
```
