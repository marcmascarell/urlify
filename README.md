# URLify PHP port for Laravel4

A PHP port of [URLify.js](https://github.com/django/django/blob/master/django/contrib/admin/static/admin/js/urlify.js)
from the Django project. Handles symbols from Latin languages, Czech, Greek, Latvian, 
Lithuanian, Polish, Romanian, Russian, Turkish and Ukrainian. Symbols it cannot 
transliterate it will simply omit.

* Author: [jbroadway](http://github.com/jbroadway)
* License: MIT

## Usage:

Add to `app/config` at the bottom of Providers:

```php
'Jbroadway\Urlify\UrlifyServiceProvider'
```

To generate slugs for URLs:

```php
<?php

echo Jbroadway\Urlify::filter (' J\'étudie le français ');
// "jetudie-le-francais"

echo Jbroadway\Urlify::filter ('Lo siento, no hablo español.');
// "lo-siento-no-hablo-espanol"

?>
```

To simply transliterate characters:

```php
<?php

echo Jbroadway\Urlify::downcode ('J\'étudie le français');
// "J'etudie le francais"

echo Jbroadway\Urlify::downcode ('Lo siento, no hablo español.');
// "Lo siento, no hablo espanol."

/* Or use transliterate() alias: */

echo Jbroadway\Urlify::transliterate ('Lo siento, no hablo español.');
// "Lo siento, no hablo espanol."

?>
```

To extend the character list:

```php
<?php

Jbroadway\Urlify::add_chars (array (
	'¿' => '?', '®' => '(r)', '¼' => '1/4',
	'¼' => '1/2', '¾' => '3/4', '¶' => 'P'
));

echo Jbroadway\Urlify::downcode ('¿ ® ¼ ¼ ¾ ¶');
// "? (r) 1/2 1/2 3/4 P"

?>
```

To extend the list of words to remove:

```php
<?php

Jbroadway\Urlify::remove_words (array ('remove', 'these', 'too'));

?>
```

To priorize a certain language map:

```php
<?php

echo Jbroadway\Urlify::filter (' Ägypten und Österreich besitzen wie üblich ein Übermaß an ähnlich öligen Attachés ',60,"de");
// "aegypten-und-oesterreich-besitzen-wie-ueblich-ein-uebermass-aehnlich-oeligen-attaches"
   
echo Jbroadway\Urlify::filter ('Cağaloğlu, çalıştığı, müjde, lazım, mahkûm',60,"tr");
// "cagaloglu-calistigi-mujde-lazim-mahkum"

?>
```
Please note that the "ü" is transliterated to "ue" in the first case, whereas it results in a simple "u" in the latter.