Yml генератор на php
============

Генератор YML для быстрого и *безболезненного* подключения вашего магазина к Yandex.Market. Основные фишки:

**Ультимативно прост.** Состоит из двух файлов: [ymlDocument.php](ymlDocument.php) и [ymlOffer.php](ymlOffer.php). В них соответственно объявлены классы `ymlDocument` и `ymlOffer`, унаследованные от встроенных классов `DomDocument` и `DomElement`. Никакие сторонние библиотеки и фреймворки не требуются. Может пожаловаться на отсутствие `DomElement`, что решается установкой пакета `php-xml`, хотя обычно на хостингах он уже есть.

**Простота встраивания** - можно взять один из примеров, проверить его работоспособность валидатором Яндекса, и на основе него сделать генератор для своего магазина.

Для каждого типа описаний сделан свой пример использования по принципу "удали лишнее". Порядок следования полей такой же, как и в документации, за исключением вынесенных в конструктор. Примеры выглядят вот так:
```php
// model, vendor, id, price, currencyId, categoryId	, [price from - "цена от ххх руб." ]
$offer = $y->arbitrary( '3811', 'Brand', 'id01id1111', 900, "USD", 15 /* , true*/ );

$offer	->cbid(80)				//	Размер ставки на карточке товара. 0,8 у.е.
	->url("http://magaz.ru/tovar.html")	// !!!	условно обязательный. URL страницы товара 
	//->vat('VAT_10_110') отсутствует в схеме	// Ставка НДС для товара.
```
Здесь `arbitrary()` создает предложение типа «произвольный», в него вынесены гарантированно обязательные поля. Метод  `cbid()` уже не обязателен, его можно просто удалить, если он не нужен. Метод `url()` также можно удалить, но без него не будет работать модель «Переход на сайт» . Далее, метод `vat()` описан в документации, но отсутствует в xsd-схеме указанной в [технических требованиях](https://yandex.ru/support/webmaster/goods-prices/technical-requirements.html) и не проходит валидацию.
 Каждая строчка имеет комментарий. Файлы примеров:

Тип предложения			| 	Пример												| Дата валидации
----------------		| ------------- 										| -------
Упрощенный				| [examples/simple.php](examples/simple.php)			| 16.09.2018
Произвольный 			| [examples/arbitrary.php](examples/arbitrary.php)		| 16.09.2018
Книги					| [examples/book.php](examples/book.php)				| 16.09.2018
Аудиокниги				| [examples/audiobook.php](examples/audiobook.php)		| 16.09.2018
Аудио и видеопродукция	| [examples/artist.php](examples/artist.php)			| 16.09.2018
Туры					| [examples/tour.php](examples/tour.php)				| 16.09.2018
Мероприятия				| [examples/event.php](examples/event.php)				| 16.09.2018
Лекарства				| [examples/medicine.php](examples/medicine.php)		| Невозможна <br>16.09.2018

Валидация проводилась с помощью [сервиса Яндекса](https://webmaster.yandex.ru/tools/xml-validator/), указывая тип *"Маркет"*. Тип "лекартсва" не проходит, т.к он отсутствует в схеме валидатора, даже примеры из документации не проходят проверку.

**Быстрая проверка актуальности.** Документация у Яндекса сделана преимущественно в виде таблиц. Соответственно я задокументировал свои примеры такими же таблицами с аналогичным порядком следования полей, описав текущие правила и ограничения. Просто открываете рядом два окна браузера, с документацией Яндекса и моей, и ищете различия. Нет различий - хорошо, есть - пишете на *e-mail* в профиле.

Таблица Яндекса																			| 	Таблица моя		
----------------------- 																| ------------- 
[Общие поля магазина](https://yandex.ru/support/partnermarket/export/yml.html)			| [Общие поля магазина](docs/yml.md)
[Упрощенный тип](https://yandex.ru/support/partnermarket/offers.html)					| [Упрощенный тип](docs/simple.md)
[Произвольный тип](https://yandex.ru/support/partnermarket/export/vendor-model.html)	| [Произвольный тип](docs/arbitrary.md)
[Книги](https://yandex.ru/support/partnermarket/export/books.html)						| [Книги](docs/book.md)
[Аудиокниги](https://yandex.ru/support/partnermarket/export/audiobooks.html)			| [Аудиокниги](docs/audiobook.md)
[Аудио и видеопродукция](https://yandex.ru/support/partnermarket/export/music-video.html)| [Аудио и видеопродукция](docs/artist.md)
[Туры](https://yandex.ru/support/partnermarket/export/tours.html)						| [Туры](docs/tour.md)
[Мероприятия](https://yandex.ru/support/partnermarket/export/event-tickets.html)		| [Мероприятия](docs/event.md)
[Лекарства](https://yandex.ru/support/partnermarket/export/medicine.html)				| [Лекарства](docs/medicine.md)

Не обязательно проверять все, используются обычно общие поля и какой-то один тип.

Также был сделан нагрузочный тест [examples/stress-test.php](examples/stress-test.php). Для генерации файла объемом 500 Мб (максимально разрешенный Яндексом) понадобилось 143 секунды, потребление памяти составило 2 Мб.

В случае необходимости внести изменения, рекомендую сначала ознакомиться с документацией по классам [ymlDocument](docs/ymlDocument.md) и [ymlOffer](docs/ymlOffer.md).

**P.S.** Ставь звездочки, если считаешь, что проект должен быть в топе.