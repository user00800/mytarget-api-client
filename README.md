MyTarget API Client for PHP
==============================

Простой и удобный PHP-клиент для [MyTarget Api](https://target.my.com/adv/api-marketing/).

## Требования
 * PHP 7.0 и выше
 
## Установка  
В файле `composer.json`:
```php
{
    ...
    "require": {
        ...
        "user00800/mytarget-api-client": "*"
    }
    ...
}
```

## Использование
### Оглавление
1. [Получение токена](https://github.com/kradwhite/mytarget-api-client#Получение-токена)
2. [Инициализация клиента](https://github.com/kradwhite/mytarget-api-client#Инициализация-клиента)
3. [Конфигурация клиента](https://github.com/kradwhite/mytaget-api-client#Конфигурация-клиета)
4. [Примеры запросов](https://github.com/kradwhite/mytarget-api-client#Примеры-запросов)
5. [Полезная информация](https://github.com/kradwhite/mytarget-api-client#Полезная-информация)

## Получение токена
```php
use kradwhite\myTarget\oauth2\Oauth2;

$oauth = new Oauth2();
$token = $oauth->clientCredentialsGrant('client_id', 'client_secret')->request();
$access_token = $token['access_token'];
```
Для получения информации по другим видам токенов можно познакомится в [kradwhite\mytarget-oauth2](https://github.com/kradwhite/mytarget-oauth2)

## Инициализация клиента
```php
use kradwhite\myTarget\api\Client;

$client = new Client($access_token);
```

## Конфигурация клиента
```php
$config = [
    // по умолчанию false. Если true, запросы будут отправляться к песочнице myTarget.
    'sandbox' => true,
    // по умолчанию true. Если true, ответом на запросы к myTarget будет ассоциативный массив,
    // в противно случае объект.
    'assoc' => false,
    // по умолчанию false. Включает опцию debug
    // http://docs.guzzlephp.org/en/stable/request-options.html#debug.
    'debug' => true,
    // по умолчанию 0. Установка опции timeout
    // http://docs.guzzlephp.org/en/stable/request-options.html#timeout.
    'timeout' => 0,
    // по умолчанию kradwhite\myTarget\transport\Transport. Имя класса реализующего
    // интерфейс kradwhite\mytarget\transport\TransportInterface.
    'transport' => Class::name,
];

// инициализация клиента с конфигурацией
$client = new Client($access_token, $config);
```

## Примеры запросов
```php
// получение кампаний
$allCampaigns = $client->campaigns()->get();

// получение активных кампаний
$activedCampaigns = $client->campaigns()->get([
    '_status' => 'active',
    'sorting' => 'id'
]);
```

```php
// создание ссылки
$newUrlId = $client->createUrl()->post([
    'url' => 'http://example.com/123456789?1=1'
]);
```

```php
// редактирование рекламного объявления
$response = $client->banner()->post([
    'status' => 'blocked'
]);
```

```php
// запрос статистика по кампании
$statistics = $client->statistics()->get(
    // название ресурса campaigns, banners или user
    'campaigns',
    // id ресурса, или несколько id через запятую
    '1234',
    // по умолчанию base, метрика
    'base',
    // по умолчанию summary, summary или days. Eсли days, нужно указать 
    // 2 следующих параметры в виде даты
    'day',
    // дата начала статистики
    '2019-10-08'
    // дата конца статистика
    '2019-11-01'
);
```

## Полезная информация
- В классе kradwhite\myTarget\api\Client перед каждым методом в комментариях имеется ссылка на оффициальную страницу в документации myTarget по запрашиваемому ресурсу.
- Имена методов клиента для получения ресурсов совпадают с именами ресурсов из официальной документации.
