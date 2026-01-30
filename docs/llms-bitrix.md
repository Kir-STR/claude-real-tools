# Bitrix Framework

> Bitrix Framework — технологическая платформа для продуктов 1С-Битрикс: Управление сайтом (CMS) и Битрикс24 (корпоративный портал, CRM). Модульная архитектура с более чем 80 модулями, поддержка старого ядра и нового D7 (ООП с namespace). Основной язык — PHP.

Bitrix Framework использует два ядра: старое (процедурное API с классами C*) и новое D7 (объектно-ориентированное с namespace Bitrix\*). Для новой разработки рекомендуется D7. Все кастомизации размещаются в /local/, системные файлы в /bitrix/ изменять запрещено.

## Основные порталы документации

- [Официальная документация](https://docs.1c-bitrix.ru/): Новый портал документации Bitrix Framework (Documentation as Code)
- [Справочник разработчика на Bitrix Framework](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=105) Старый портал Справочник разработчика на Bitrix Framework
- [Документация старого ядра](https://dev.1c-bitrix.ru/api_help/): Справочное описание API старого ядра, классы C* и методы модулей
- [Документация D7](https://dev.1c-bitrix.ru/api_d7/): API нового ядра D7 с ORM, пространствами имён Bitrix\*
- [REST API документация](https://dev.1c-bitrix.ru/rest_help/): REST API для интеграций и приложений Битрикс24.Маркет
- [REST API Битрикс24](https://apidocs.bitrix24.ru/): Альтернативный портал документации REST API
- [GitHub репозиторий](https://github.com/bitrix-tools/framework-docs): Исходники документации в Markdown формате
- [Пользовательская документация](https://dev.1c-bitrix.ru/user_help/): Описание функционала 1C-Битрикс: Управление сайтом (CMS) и Битрикс24 (корпоративный портал, CRM)

## Учебные курсы

- [Разработчик Bitrix Framework](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43): Основной курс для разработчиков, 381 урок, уровни Junior/Middle/Senior
- [Контент-менеджер](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=34): Базовые понятия, работа с компонентами 2.0
- [Администратор базовый](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=35): Права доступа, инструменты администрирования
- [Администратор модули](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=41): Модули Управления сайтом
- [Композитный сайт](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=39): Технология ускорения сайта
- [Маркетплейс](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=101): Публикация модулей в Маркетплейсе
- [Многосайтовость](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=103): Многосайтовые конфигурации
- [Vue.js и Bitrix](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=176): Интеграция Vue.js с Bitrix Framework
- [Высоконагруженные проекты](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=38): Оптимизация производительности

## Файловая структура проекта

- [Структура /local/](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=4621): Папка для пользовательских кастомизаций, приоритет над /bitrix/
- [Структура /bitrix/](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2912): Системная папка, изменять запрещено
- [Файл init.php](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2916): Регистрация обработчиков событий в /local/php_interface/init.php
- [Файл .settings.php](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=14026): Конфигурация соединения с БД (версия 20.900.0+)
- [Список модулей](https://dev.1c-bitrix.ru/api_help/main/general/identifiers.php): Идентификаторы всех модулей системы

## Главный модуль (main)

- [Главный модуль D7](https://dev.1c-bitrix.ru/api_d7/bitrix/main/index.php): Ядро системы — Application, Context, Loader, EventManager, ORM
- [Главный модуль старое ядро](https://dev.1c-bitrix.ru/api_help/main/index.php): Классы CMain, CDatabase, CUser, CGroup
- [Класс Application](https://dev.1c-bitrix.ru/api_d7/bitrix/main/Application/index.php): Точка входа приложения Bitrix\Main\Application
- [Класс Loader](https://dev.1c-bitrix.ru/api_d7/bitrix/main/Loader/index.php): Автозагрузка классов и подключение модулей
- [Класс Context](https://dev.1c-bitrix.ru/api_d7/bitrix/main/Context/index.php): Контекст запроса — Request, Response, Server
- [События главного модуля](https://dev.1c-bitrix.ru/api_help/main/events/index.php): Список событий OnPageStart, OnProlog, OnEpilog и др.

## Информационные блоки (iblock)

- [Модуль iblock D7](https://dev.1c-bitrix.ru/api_d7/bitrix/iblock/index.php): ORM-классы IblockTable, ElementTable, SectionTable, PropertyTable
- [Модуль iblock старое ядро](https://dev.1c-bitrix.ru/api_help/iblock/index.php): Классы CIBlock, CIBlockElement, CIBlockSection, CIBlockProperty
- [CIBlockElement::GetList](https://dev.1c-bitrix.ru/api_help/iblock/classes/ciblockelement/getlist.php): Выборка элементов инфоблока с фильтрацией и сортировкой
- [Свойства инфоблоков](https://dev.1c-bitrix.ru/api_help/iblock/classes/ciblockproperty/index.php): Работа со свойствами — типы, множественные значения
- [ORM API инфоблоков](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5748): Работа с инфоблоками через D7 ORM по символьному коду API

## Highload-блоки (highloadblock)

- [Модуль highloadblock](https://dev.1c-bitrix.ru/api_d7/bitrix/highloadblock/index.php): Высокопроизводительные справочники без иерархии
- [HighloadBlockTable](https://dev.1c-bitrix.ru/api_d7/bitrix/highloadblock/HighloadBlockTable/index.php): Управление HL-блоками — getById, getList, add, compileEntity
- [Создание HL-блока](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5745): Программное создание и работа с Highload-блоками
- [DataClass HL-блока](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5746): Динамически генерируемый класс для CRUD-операций

## ORM D7 (Bitrix\Main\ORM)

- [ORM обзор](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5753): Object-Relational Mapping для работы с БД
- [DataManager](https://dev.1c-bitrix.ru/api_d7/bitrix/main/orm/data/DataManager/index.php): Базовый класс для работы с таблицами — getList, add, update, delete
- [Query построитель](https://dev.1c-bitrix.ru/api_d7/bitrix/main/orm/Query/index.php): Построитель запросов setSelect, setFilter, setOrder, setLimit
- [Entity сущности](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5756): Описание таблиц БД через getMap, поля и связи
- [Объекты и коллекции](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5757): ОО-подход к данным — fetchObject, fetchCollection
- [Runtime поля](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5758): Динамические поля через registerRuntimeField
- [Транзакции БД](https://docs.1c-bitrix.ru/database/transactions.html): Работа с транзакциями в D7

## Система событий (EventManager)

- [EventManager](https://dev.1c-bitrix.ru/api_d7/bitrix/main/EventManager/index.php): Регистрация и обработка событий (паттерн Observer)
- [Курс по событиям](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=3493): Обработчики событий — addEventHandler, registerEventHandler
- [Event класс](https://dev.1c-bitrix.ru/api_d7/bitrix/main/Event/index.php): Объект события Bitrix\Main\Event
- [EventResult](https://dev.1c-bitrix.ru/api_d7/bitrix/main/EventResult/index.php): Результат обработки события
- [События iblock](https://dev.1c-bitrix.ru/api_help/iblock/events/index.php): OnBeforeIBlockElementAdd, OnAfterIBlockElementUpdate и др.
- [События sale](https://dev.1c-bitrix.ru/api_d7/bitrix/sale/events/): События заказов, корзины, оплат, доставок

## Кэширование

- [CPHPCache старое ядро](https://dev.1c-bitrix.ru/api_help/main/reference/cphpcache/index.php): Кэширование HTML и переменных — InitCache, StartDataCache, EndDataCache
- [Cache D7](https://dev.1c-bitrix.ru/api_d7/bitrix/main/data/Cache/index.php): Класс Bitrix\Main\Data\Cache
- [TaggedCache](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2978): Тегированный кэш — startTagCache, registerTag, clearByTag
- [ManagedCache](https://dev.1c-bitrix.ru/api_d7/bitrix/main/data/ManagedCache/index.php): Управляемый кэш для разных бэкендов
- [Кэширование компонентов](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2832): Автокэширование и Cache Dependencies

## Компонентная архитектура

- [Компоненты 2.0](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2829): Визуальные компоненты для вывода данных
- [Структура компонента](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2830): class.php, component.php, .description.php, .parameters.php, templates/
- [CBitrixComponent](https://dev.1c-bitrix.ru/api_help/main/reference/cbitrixcomponent/index.php): Базовый класс компонента
- [Комплексные компоненты](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2838): Объединение нескольких простых компонентов
- [result_modifier.php](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2834): Модификация $arResult перед шаблоном
- [component_epilog.php](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2835): Код после кэширования (не кэшируется)

## Шаблоны сайтов и компонентов

- [Шаблоны сайтов](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2821): header.php, footer.php, styles.css, #WORK_AREA#
- [Шаблоны компонентов](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2833): template.php, style.css, script.js
- [Кастомизация шаблонов](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2836): Копирование в /local/templates/
- [Включаемые области](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2824): Редактируемые области в шаблоне

## Пользователи и права доступа

- [CUser класс](https://dev.1c-bitrix.ru/api_help/main/reference/cuser/index.php): Работа с пользователями — GetList, Add, Update, Authorize
- [Глобальный $USER](https://dev.1c-bitrix.ru/api_help/main/reference/cuser/index.php): IsAuthorized, IsAdmin, GetID, GetLogin, GetEmail
- [UserTable D7](https://dev.1c-bitrix.ru/api_d7/bitrix/main/UserTable/index.php): ORM-класс для пользователей
- [CGroup класс](https://dev.1c-bitrix.ru/api_help/main/reference/cgroup/index.php): Группы пользователей
- [Права доступа](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2940): Уровни доступа, ролевая модель

## Интернет-магазин (sale)

- [Модуль sale D7](https://dev.1c-bitrix.ru/api_d7/bitrix/sale/index.php): Заказы, корзина, оплаты, доставки, скидки
- [Модуль sale старое ядро](https://dev.1c-bitrix.ru/api_help/sale/index.php): Классы CSaleOrder, CSaleBasket
- [Order класс](https://dev.1c-bitrix.ru/api_d7/bitrix/sale/Order/index.php): Работа с заказами Bitrix\Sale\Order
- [Basket и BasketItem](https://dev.1c-bitrix.ru/api_d7/bitrix/sale/Basket/index.php): Корзина покупок
- [Payment и Shipment](https://dev.1c-bitrix.ru/api_d7/bitrix/sale/Payment/index.php): Оплаты и доставки
- [Практика работы с заказами](https://dev.1c-bitrix.ru/api_d7/bitrix/sale/technique/orders.php): Пошаговое руководство создания заказа

## Торговый каталог (catalog)

- [Модуль catalog D7](https://dev.1c-bitrix.ru/api_d7/bitrix/catalog/index.php): Товары, цены, склады, торговые предложения
- [Модуль catalog старое ядро](https://dev.1c-bitrix.ru/api_help/catalog/index.php): Классы CCatalogProduct, CPrice, CCatalogStore
- [События catalog](https://dev.1c-bitrix.ru/api_help/catalog/events/index.php): OnBeforePriceAdd, OnAfterProductUpdate и др.
- [Торговые предложения](https://dev.1c-bitrix.ru/api_help/catalog/classes/ccatalogssku/index.php): SKU — товары с вариациями

## CRM (Битрикс24)

- [Модуль CRM D7](https://dev.1c-bitrix.ru/api_d7/bitrix/crm/index.php): Лиды, сделки, контакты, компании, счета
- [Модуль CRM старое ядро](https://dev.1c-bitrix.ru/api_help/crm/index.php): Классы CCrmLead, CCrmDeal, CCrmContact
- [REST API CRM](https://dev.1c-bitrix.ru/rest_help/crm/index.php): Методы crm.lead.*, crm.deal.*, crm.contact.*

## REST API

- [REST API обзор](https://dev.1c-bitrix.ru/rest_help/index.php): Документация по REST API
- [OAuth авторизация](https://dev.1c-bitrix.ru/rest_help/general/oauth.php): Авторизация приложений
- [Входящие вебхуки](https://dev.1c-bitrix.ru/rest_help/general/webhooks.php): Локальные интеграции без приложения
- [Исходящие вебхуки](https://dev.1c-bitrix.ru/rest_help/general/outgoing_webhooks.php): Уведомления о событиях
- [Batch запросы](https://dev.1c-bitrix.ru/rest_help/general/batch.php): Пакетное выполнение методов

## Безопасность

- [Проактивная защита](https://dev.1c-bitrix.ru/api_help/security/index.php): WAF, журнал вторжений, сканер безопасности
- [B_PROLOG_INCLUDED](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2920): Защита от прямого вызова файлов
- [CSRF защита](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2921): bitrix_sessid(), check_bitrix_sessid()
- [XSS защита](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2922): htmlspecialchars, санитайзер HTML
- [SQL Injection защита](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2923): CDatabase::ForSql, использование ORM

## Дополнительные модули D7

- [Валюты (currency)](https://dev.1c-bitrix.ru/api_d7/bitrix/currency/): Работа с валютами и курсами
- [Бизнес-процессы (bizproc)](https://dev.1c-bitrix.ru/api_d7/bitrix/bizproc/): Автоматизация workflow
- [Задачи (tasks)](https://dev.1c-bitrix.ru/api_d7/bitrix/tasks/): Управление задачами Битрикс24
- [Календарь (calendar)](https://dev.1c-bitrix.ru/api_d7/bitrix/calendar/): Календарь событий
- [Диск (disk)](https://dev.1c-bitrix.ru/api_d7/bitrix/disk/): Файловое хранилище Битрикс24
- [Социальная сеть (socialnetwork)](https://dev.1c-bitrix.ru/api_d7/bitrix/socialnetwork/): Соцсеть, рабочие группы
- [Сайты24 (landing)](https://dev.1c-bitrix.ru/api_d7/bitrix/landing/): Конструктор сайтов
- [UI библиотека (ui)](https://dev.1c-bitrix.ru/api_d7/bitrix/ui/): Интерфейсные компоненты
- [Push and Pull (pull)](https://dev.1c-bitrix.ru/api_d7/bitrix/pull/): Real-time уведомления
- [Веб-мессенджер (im)](https://dev.1c-bitrix.ru/api_d7/bitrix/im/): Мгновенные сообщения
- [Телефония (voximplant)](https://dev.1c-bitrix.ru/api_d7/bitrix/voximplant/): VoIP интеграция
- [Почта (mail)](https://dev.1c-bitrix.ru/api_d7/bitrix/mail/): Работа с email
- [Поиск (search)](https://dev.1c-bitrix.ru/api_help/search/index.php): Полнотекстовый поиск

## Модули коммуникаций

- [Чат-боты (imbot)](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=93): Создание чат-ботов Битрикс24
- [Открытые линии (imopenlines)](https://dev.1c-bitrix.ru/api_d7/bitrix/imopenlines/): Омниканальная поддержка
- [Коннекторы (imconnector)](https://dev.1c-bitrix.ru/api_d7/bitrix/imconnector/): Интеграция с внешними мессенджерами
- [Рассылки (sender)](https://dev.1c-bitrix.ru/api_d7/bitrix/sender/): Email-маркетинг
- [SMS (messageservice)](https://dev.1c-bitrix.ru/api_d7/bitrix/messageservice/): Отправка SMS

## Best practices разработки

- [Кастомизации в /local/](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=4621): Изоляция от обновлений, работа с Git
- [Использование D7](https://dev.1c-bitrix.ru/api_d7/): Предпочтение нового ядра старому API
- [Кэширование](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2978): Обязательное кэширование компонентов
- [Стандарты кода](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=3513): Единый стиль, говорящие названия переменных
- [Безопасность](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2920): Проверка прав, CSRF-токены, экранирование

## Частые ошибки и решения

- [Изменение /bitrix/](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2912): Потеря изменений при обновлении — использовать /local/
- [Игнорирование кэширования](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2832): Низкая производительность — настроить кэширование компонентов
- [PHP в шаблонах](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2834): Логика в шаблоне — использовать result_modifier.php
- [Прямые SQL-запросы](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5753): SQL Injection — использовать ORM D7
- [SELECT * выборки](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=5754): Медленные запросы — выбирать только нужные поля
- [Отсутствие B_PROLOG_INCLUDED](https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=43&LESSON_ID=2920): Уязвимость — добавить проверку во все файлы

## Инфраструктура и DevOps

- [Веб-кластер (cluster)](https://dev.1c-bitrix.ru/api_help/cluster/index.php): Кластеризация и балансировка
- [Облачные хранилища (clouds)](https://dev.1c-bitrix.ru/api_help/clouds/index.php): Интеграция с S3, Azure, Google Cloud
- [Монитор производительности (perfmon)](https://dev.1c-bitrix.ru/api_help/perfmon/index.php): Профилирование и мониторинг
- [Контроллер сайтов (controller)](https://dev.1c-bitrix.ru/api_help/controller/index.php): Удалённое управление несколькими сайтами

## Интеграции

- [Интеграция с 1С](https://dev.1c-bitrix.ru/api_help/catalog/1c_exchange/index.php): Обмен данными с 1С:Предприятие (CommerceML)
- [AD/LDAP (ldap)](https://dev.1c-bitrix.ru/api_help/ldap/index.php): Интеграция с Active Directory
- [Социальные сервисы](https://dev.1c-bitrix.ru/api_help/socialservices/index.php): OAuth авторизация через соцсети
- [SOAP веб-сервисы](https://dev.1c-bitrix.ru/api_help/webservice/index.php): Публикация и потребление SOAP

## Полезные ресурсы

- [Телеграм документации](https://t.me/bitrixdoc): Канал с новостями документации
- [Блог документации](https://dev.1c-bitrix.ru/community/blogs/Docs_and_other/): Статьи от команды документации
- [Форум разработчиков](https://dev.1c-bitrix.ru/community/forums/): Обсуждения и вопросы
- [CHM справочники](https://dev.1c-bitrix.ru/api_d7/): Скачиваемые api_d7.chm, bsm_api.chm, b24_rest.chm
- [Маркетплейс модулей](https://marketplace.1c-bitrix.ru/): Готовые решения и модули

## Optional

- [Статистика (statistic)](https://dev.1c-bitrix.ru/api_help/statistic/index.php): Веб-аналитика посещений
- [Реклама (advertising)](https://dev.1c-bitrix.ru/api_help/advertising/index.php): Управление баннерами
- [Опросы (vote)](https://dev.1c-bitrix.ru/api_help/vote/index.php): Голосования и опросы
- [Техподдержка (support)](https://dev.1c-bitrix.ru/api_help/support/index.php): Система тикетов
- [Фотогалерея (photogallery)](https://dev.1c-bitrix.ru/api_help/photogallery/index.php): Фотогалереи 2.0
- [Wiki](https://dev.1c-bitrix.ru/api_help/wiki/index.php): Wiki-страницы
- [Блоги (blog)](https://dev.1c-bitrix.ru/api_help/blog/index.php): Ведение блогов
- [Форум (forum)](https://dev.1c-bitrix.ru/api_help/forum/index.php): Создание форумов