# Темы

* [Установка темы](#installation)
* [Структура темы](#file-structure)
* [Манифесты](#theme-manifests)
* [Мультиязычные темы](#multilingual)
* [Дочерние темы](#child-themes)

<a name="installation"></a>
## Установка темы

Установить тему можо одним из трех способов.

1. Загрузить архив темы в админке, на странице Темы -> Добавить тему
2. Загрузить архив темы через FTP и распаковав в папку /themes
3. Запросить с помошью консоли из репозитория композера командой `composer require vendor/theme-name`

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
    * en.php
    * ru.php
  * [layouts](#struc-layouts)
  * [modules](#struc-modules)
  * [theme.json](#struc-manifest)
  * [page.html](#struc-wrapper)
  * [custom.css](#struc-custom)
  * [custom.js](#struc-custom)
  * [composer.json](#struc-composer)

<a name="struc-theme"></a>
**theme-name** - Идентификатор темы и имя папки, может содержать только латинские буквы в нижнем регистре, цифры и тире.

<a name="struc-assets"></a>
**assets** - Папка со стилями, скриптами и картинками. Так же можно хранить сырые данные для препроцессоров.

<a name="struc-iblocks"></a>
**iblocks** - Инфоблоки, связанные с темой или только их шаблоны. Необязательная папка, создается автоматически при необходимости.

<a name="struc-languages"></a>
**languages** - Директория для переводов текстов темы, для мультиязычных сайтов, не обязательная.

<a name="struc-layouts"></a>
**layouts** - Макеты темы. Для дочерней темы не обязательны.

<a name="struc-modules"></a>
**modules** - Содержит замену для представлений модулей, совсем не обязательны.

<a name="struc-manifest"></a>
**theme.json** - Манифест темы, содержит информацию, данные и настройки.

<a name="struc-wrapper"></a>
**page.html** - Главный оберточный файл темы, необязателен для дочерней темы.

<a name="struc-custom"></a>
**custom.css и custom.js** - Кастомные стили и скрипты темы. Не должны создаваться в публичной теме, что бы не перезаписывались
при обновлениях. Создаются только конечными пользователями, при необходимости. Подгружаются автоматически, так что нет необходимости
добавлять в манифест.

<a name="struc-composer"></a>
**composer.json** - Не обязателен, используется для устновки через композер.


<a name="theme-manifests"></a>
## Манифесты

### theme.json

Основной манифест темы, который состоит из трех основных и двух необязательных блоков.

#### Основные блоки

**info** - Описание темы, содержит информацию для отображения в админке и каталоге расширений.

**js** и **css** - Aссеты, которые будут загружаться на всех страницах сайта. Если их прописать здесь, а не прямо в шаблоне,
то будет возможным использование сжимателя и склейщика ассетов, а так же удаление их в дочерних темах.

#### Дополнительные блоки

**settings** - Настройки для инфоблоков и редактора данных темы.

**data** - Данные, специфичные для текущей темы. Здесь может храниться информация о логотипе, телефоне в шапке, текст слогана.

Пример заполнения данных

```json
{
  "info": {
    "name": "Name of this theme",
    "description": "Short description for catalog",
    "version": "1.0",
    "tags": ["any columns", "gray"],
    "screenshot": "assets/images/screenshot.jpg",
    "authors": [
      {
        "name": "ThemeMaker",
        "email": "me@email.com",
        "homepage": "http://yoursite.org"
      }
    ]
  },
  "settings": {
    "image_styles": {
	     "medium": {
	       "width": 250,
	       "height": 200,
	       "type": "crop_center"
	     }
	   },
    "data_fields": {
      "map_show": {
        "type": "yesNo"
      },
      "map_type": {
        "type": "select",
        "options": ["default", "gmap"]
      },
      "logo": {
        "type": "image"
      }
    }
  },
  "data": {
    "phone": "+7(123)45-67-890",
    "address": "Most Famous st.",
    "map_show": 1,
    "map_type": "gmap",
    "logo": "/themes/default/assets/images/logo.png"
  },
  "js": {
    "bootstrap": [
      "//cdnjs.cloudflare.com/ajax/libs/tether/1.4.0/js/tether.min.js",
      "//maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/js/bootstrap.min.js"
    ],
    "lightbox": "//cdnjs.cloudflare.com/ajax/libs/fancybox/2.1.5/jquery.fancybox.pack.js",
    "theme": [
      "assets/js/main.js"
    ]
  },
  "css": {
    "bootstrap": "//maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css",
    "lightbox": "//cdnjs.cloudflare.com/ajax/libs/fancybox/2.1.5/jquery.fancybox.css",
    "theme": [
      "assets/css/main.css",
      "assets/css/color1.css"
    ]
  }
}
```

Обязательными являются только поля `info.name`, `js` и `css`.

### composer.json

Пример заполнения данных

```json
{
  "name": "vendor/theme-name",
  "description": "Demonstration of theme description",
  "type": "sydes-theme",
  "keywords": ["sydes", "theme", "cool-tags"],
  "license": "MIT",
  "authors": [
    {
      "name": "ThemeMaker",
      "email": "me@email.com",
      "homepage": "http://yoursite.org"
    }
  ],
  "require": {
    "composer/installers": "~1.0"
  }
}
```

Обратите внимание на поля `type` и `require`. Благодаря ним тема после запроса окажется сразу в папке с другими темами.

<a name="multilingual"></a>
## Мультиязычные темы

Для того, что бы сделать мультиязычную тему нужно создать папку `languages` в корне темы и в ней php файлы с двубуквенными
кодами локалей, например ru.php.

В эти файлы вставить следующий код:

```php
<?php return [
  'last_news' => 'Последние новости',
  'our_services' => 'Наши услуги',
];
```

Далее в оберточном файле или макетах можно использовать токены вида `{t:last_news}`, а в инфоблоках - функцию `<?=t('last_news');?>`.

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
  "hide_js": ["lightbox"],
  "hide_css": ["lightbox"]
```
