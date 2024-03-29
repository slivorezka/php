## Разница между версиями PHP
* PHP 5.3
  * By default `register_globals` is disabled
  * There is `Namespaces`
  * `Late Static Binding`
  * Magic methods have to be `public` and can't be `static` `__get()`, `__set()`, `__isset()`, `__unset()`, `__call()`
  * Constant `__DIR__`
  * Integral special names `__FUNCTION__`, `__METHOD__`, `__CLASS__`
  * Dynamic access to the static objects like `$foo::bar(), $foo::$bar`
  * Comparison operator `?:`, for example `$a ?: $ b === $a ? $a : $b`
  * `NowDoc`
  * Different settings for different directories `INI System`
* PHP 5.4
  * Array without `array` like `['foo', 'bar']`
  * Fast access to the array element like `$foo = foo()[0];`
  * `Trait`, like `trait Class {}`
  * Access member of newly created object like `(new Foo)->method()`
* PHP 5.5
  * Possibility to use `empty()` to functions and methods.
  * A new element for `try / catch` like `finally`
  * Possibility to use `FooClass::class` for getting `Namespaces`
  * Generators `yield`
* PHP 5.6
  * Possibility to use `const PLUS = 1 + 2;`
  * Импорт функций из пространства имён
  * Import `functions` from `Namespaces`
* PHP 7
  * Short `isset` for checking variable like `$a ?? ''`
  * Combined comparison (Spaceship) operator `$a <=> $b`
  * Grouping of `Namespaces` like `use foo\bar\{A, B, C as c};`
  * New type declarations for method and functions like `int, float, bool, array`
  * Anonymous classes like  `$foo = new Foo{};`
  * Return `return` in generators like `yield ... return 99;`  
* PHP 7.1
  * New type declaration for method and functions like `void`  
* PHP 7.2
  * Possibility to reload abstract methods
  * Everything `Libsodium` lib in the Core
* PHP 7.3
  * Possibility to use comma in the method and functions like `function($arg1, $arg2, $arg3,)`
  * New functions `array_key_first()` и `array_key_last()`  
* PHP 7.4
  * Typed properties of classes like `class Foo { public string $foo; public Bar $bar;}` 
* PHP 8
  * JIT
  * Foreign Function Interface (FFI).
  * Union types like `public function foo(Foo|Bar $input): int|float;`
  * Nullable unions `public function bar(?Bar $bar): ?void;`
  * `JIT` is a technique that will compile parts of the code at runtime, so that the compiled version can be used instead
* PHP 8.1  
  * `Enums`
  * 
  * `Fibers` — aka `green threads` — are a low level mechanism to manage parallelism
* PHP 8.2
  * Readonly classes
  * A new class called `Randomizer`, which accepts a randomizer engine
  * `null`, `true`, and `false` as standalone types like `function alwaysFalse(): false`
  * Disjunctive Normal Form Types like `function generateSlug((HasTitle&HasId)|null $post)`
  * Constants in `traits`
  * Fetch properties of enums in const expressions `const C = [self::B->value => self::B];`
  * Deprecate `${}` string interpolation
* PHP 8.3
  * Typed class constants like `public const string BAR = 'baz';` 
  * Negative indices in arrays like if `$array[-4] = 'a'; $array[] = 'b';` it will `[-4, -3]` not `[-4, 0]`
  * Anonymous readonly classes
  * Dynamic class constant fetch like `Foo::{$name};`
***

## CGI, FastCGI и FPM

### CGI — Common Gateway Interface
Common Gateway Interface, "общий интерфейс шлюза" — это стандарт,
который описывает, как веб-сервер должен запускать прикладные программы (скрипты),
как должен передавать им параметры HTTP-запроса, как программы должны передавать результаты своей работы веб-серверу.
Прикладную программу взаимодействующую с веб-сервером по протоколу CGI принято называть шлюзом,
хотя более распространено название CGI-скрипт или CGI-программа.
В качестве CGI-программ могут использоваться программы/скрипты написанные на любых языках программирования, как на компилируемых,так и на скриптовых, и даже на shell.

### FastCGI — Fast Common Gateway Interface
Дальнейшее развитие технологии CGI, является более производительным и безопасным, снимает множество ограничений CGI-программ.
FastCGI программа работает следующим образом: программа единожды загружается в память в качестве демона (независимо от HTTP-сервера),
а затем входит в цикл обработки запросов от HTTP-сервера.
Один и тот же процесс обрабатывает несколько различных запросов один за другим, что отличается от работы в CGI-режиме,
когда на каждый запрос создается отдельный процесс, "умирающий" после окончания обработки.
Написание FastCGI программ-демонов сложнее чем CGI, нужны дополнительные библиотеки, зависящие от языка.
Опять же, сама аббревиатура FastCGI это не язык программирования и не отдельная программа, это, как и в случае CGI — просто спецификация.
Следует помнить, что при работе PHP в режиме FastCGI в памяти висит сам php интерпретатор, а не какой-то конкретный php-скрипт.

### FPM — Fast Common Gateway Interface Process Manager
Это альтернативная реализация FastCGI режима в PHP с несколькими дополнительными возможностями,
которые обычно используются для высоко нагруженных сайтов.
Изначально PHP-FPM представлял из себя набор патчей от Андрея Нигматулина,
которые устраняли ряд проблем, мешающих полноценно использовать PHP в режиме FastCGI.
С версии PHP 5.3 набор патчей включён в ядро, а дополнительные возможности PHP-FPM включаются флагом при компиляции.
PHP-FPM используется в основном в связке с Nginx.

***

## Кеширование опкодов
* OPcache — Zend Opcache
  * Только кэш байткода
  * Начиная с PHP 5.5.0 уже в ядре
* APC — Alternative PHP Cache
  * Это кэш байткода и пользовательских данных
  * Был хорош для PHP 5, для PHP 7 уже все
* APCu — APC User Cache
  * Кэш пользовательских данных
* XCache
  * Устарел. Последняя версия для PHP 5.5.0
  
***

## HTTP

### Уровни стека TCP / IP (Сетевые модели OSI)
1. Прикладной: HTTP, FTP, POP3, SMTP
2. Транспортный:  TCP, UDP
3. Сетевой: IP
4. Канальный: Проводной (Ethernet) , Беспроводной (Wireless Ethernet (Wi-Fi, 3G))

### Порты
* 80 — порт WEB-сервера
* 443 —  порт WEB-сервера для HTTPS
* 21 — порт FTP сервера (File Transfer Protocol)
* 25 — порт почтового SMTP сервера. (Simple Mail Transfer Protocol)
* 110 — порт POP3 сервера (Post Office Protocol 3)

### Общая структура HTTP запросов и ответов
* Строка запроса (Request Line)
* Заголовки запроса (Headers)
* Пустая строка (Space)
* Тело сообщения (Body)

### Заголовки запроса
* Основные заголовки — обязательно включаются в любое сообщение клиента и сервера
* Заголовки запроса — можно встретить только в запросах от клиента
* Заголовки ответа — можно встретить только в ответах от сервера
* Заголовки сущности — описывают сущность каждого сообщения (может относиться как к клиенту, так и к серверу)

***

## PSR — PHP Standard Recommendation

### PHP-FIG — PHP Framework Interop Group
PHP-FIG — организованная группа разработчиков, цель которой находить способы совместной работы нескольких фрейморков.

* PSR-0 — Autoloading Standard (deprecated)
* PSR-1 — Basic Coding Standard
* PSR-2 — Coding Style Guid (deprecated)
* PCR-3 — Logger Interface
* PCR-4 — Improved Autoloading (предоставляет улучшенные методы автозагрузки)
* PCR-12 — Extended Coding Style Guide
* PCR-18 — HTTP Client

***

## SPL — Standard PHP Library
Standard PHP Library — это набор интерфейсов и классов, предназначенных для решения стандартных задач.

***

## SOAP — Simple Object Access Protocol
SOAP — это протокол используется для обмена произвольными сообщениями в формате XML.
Это протокол на основе XML, который позволяет обмениваться информацией по определенному протоколу (например, HTTP или SMTP) между приложениями.
Это означает простой протокол доступа к объектам и использует XML для его формата обмена сообщениями для передачи информации.

***

## ORM — Object Relational Mapping
Doctrine и Eloquent — это ORM по-прежнему одна из самых мощных ORM с поддержкой DBAL (DataBase Abstraction Layer).

***

## Cache
* Varnish — программное обеспечение, реализующее сервис кэширования данных в оперативной.
Основным механизмом конфигурации является "Varnish Configuration Language" (VCL), а предметно-ориентированный язык (DSL) используется для записи перехватчиков, которые вызываются в критических точках при обработке каждого запроса.
* Memcached или Memcache — программное обеспечение, реализующее сервис кэширования данных в оперативной памяти на основе хеш-таблицы. Максимальная длина ключа 250 байт. Максимальный размер даных для одного ключа 1Мб
* Redis — резидентная система управления базами данных класса NoSQL с открытым исходным кодом, работающая со структурами данных типа «ключ — значение». Используется как для баз данных, так и для реализации кэшей.
Хранит базу данных или кэш в оперативной памяти, снабжена механизмами снимков и журналирования для обеспечения постоянного хранения (на дисках, твердотельных накопителях).

***

## Поиск
* Elasticsearch — поисковый движок с json rest api, использующий Lucene и написанный на Java
* Solr — поисковый движок на Java и запускается как отдельное веб-приложение полнотекстового поиска

***

## База данных
* **MyISAM** — интересен тем, что дает просто безумную скорость на select-ах и insert-ах.
С другой стороны, он не поддерживает транзакционность и
блокировку на уровне строк, что в свою очередь приводит к страшным тормозам при использовании delete\update.
Проще говоря, на таблицу допускается только одна одновременная delete или update операция,
и остальные вынуждены ждать завершения текущей операции, что на больших объемах данных приводит к серьезным проблемам.
* **InnoDB** — был спроектирован для обработки транзакций,
в частности для большого количества короткоживущих транзакций, которые чаще комитятся чем откатываются.
Это не единственный движок, поддерживающий транзакционность, но на сегодня он является самым популярным для этой цели.

* **INNER JOIN** — Возвращает строки, когда есть совпадение в обеих таблицах
* **LEFT JOIN** — Возвращает все строки из левой таблицы, даже если в правой таблице нет совпадений (значение NULL)
* **RIGHT JOIN** — Возвращает все строки из правой таблицы, даже если в левой таблице нет совпадений (значение NULL)
* **FULL JOIN** — Комбинирует результаты элементов LEFT JOIN и RIGHT JOIN. Результатом такого запроса станет таблица, которые содержит все записи левой и правой таблицы, а колонки записей без совпадений будут иметь значение NULL.
* **SELF JOIN** — Используется для объединения таблицы с ней самой таким образом, будто это две разные таблицы, временно переименовывая одну из них.
* Управление транзакциями
  * **COMMIT** — Сохраняет изменения
  * **ROLLBACK** — Откатывает (отменяет) изменения
  * **SAVEPOINT** — Создаёт точку к которой группа транзакций может откатиться
  * **SET TRANSACTION** — Размещает имя транзакции

### NoSQL — Not Only SQL
* JSON - Используется для хранения данных
* Не предопределены типы для колонок

***

## WSDL — Web Services Description Language
WSDL — это язык описания веб-сервисов и доступа к ним, основанный на языке XML.

***

## REST — Representational State Transfer
REST — это архитектурный стиль сетевых систем и выступает за передачу репрезентативного состояния.
Это не стандарт сам, но использует такие стандарты, как HTTP, URL, XML и т.д.
REST является архитектурным стилем, в то время как SOAP является протоколом.

***

## API — Application Programming Interface
API определяет функциональность, которую предоставляет программа (модуль, библиотека), при этом API позволяет абстрагироваться от того, как именно эта функциональность реализована.

***

## SOLID — Single responsibility, Open-closed, Liskov substitution, Interface segregation и Dependency inversion
* Принцип единственной ответственности (Single responsibility)
«На каждый объект должна быть возложена одна единственная обязанность»
Для этого проверяем, сколько у нас есть причин для изменения класса — если больше одной, то следует разбить данный класс.

* Принцип открытости (Open-closed)
Данный принцип гласит — «программные сущности должны быть открыты для расширения, но закрыты для модификации». На более простых словах это можно описать так — все классы, функции и т.д. должны проектироваться так, чтобы для изменения их поведения, нам не нужно было изменять их исходный код.

* Принцип подстановки Барбары Лисков (Liskov substitution)
«Объекты в программе могут быть заменены их наследниками без изменения свойств программы»
Для этого проверяем, не усилили ли мы предусловия и не ослабили ли постусловия. Если это произошло — то принцип не соблюдается

* Принцип разделения интерфейса (Interface segregation)
«Много специализированных интерфейсов лучше, чем один универсальный»
Проверяем, насколько много интерфейс содержит методов и насколько разные функции накладываются на эти методы, и если необходимо — разбиваем интерфейсы.

* Принцип инверсии зависимостей (Dependency Inversion)
«Зависимости должны строится относительно абстракций, а не деталей»
Проверяем, зависят ли классы от каких-то других классов(непосредственно инсталлируют объекты других классов и т.д) и если эта зависимость имеет место, заменяем на зависимость от абстракции.

***

## TESTING
* BDD — Behavior driven development
Это разработка, основанная на описании поведения. То есть, есть специальный человек(или люди) который пишет описания вида "я как пользователь хочу когда нажали кнопку пуск тогда показывалось меню как на картинке". (там есть специально выделенные ключевые слова). Программисты давно написали специальные тулы (например, cucumber), которые подобные описания переводят в тесты (иногда совсем прозрачно для программиста). А дальше классическая разработка с тестами.

* TDD — Test Driven Development
Это такая техника разработки программного обеспечения, при которой вся разработка разбивается на множество небольших циклов: сначала пишутся тесты, которые покрывают желаемое изменение, затем пишется код, который эти тесты проходит. После этого производится рефакторинг этого кода, при необходимости пишутся новые тесты. Если какие-то тесты участок кода не проходит, это исправляется.
