
# Yndx-DSF-WMS API Integration

[![DSF logo](https://img.shields.io/badge/DSF%20Trading-blue?link=https%3A%2F%2Fdsf.kz%2F
)](https://dsf.kz)
[![Extension version](https://img.shields.io/badge/extension-v.1.0.1.15-blue
)](https://dsf.kz)
[![MarketApi](https://img.shields.io/badge/%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5%20API%20%D0%B4%D0%BB%D1%8F%20%D0%B2%D0%B7%D0%B0%D0%B8%D0%BC%D0%BE%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D0%B8%D1%8F-xml-green?style=flat-square&logo=%23F9AB00&link=https%3A%2F%2Fyandex.ru%2Fdev%2Fmarket%2Ffulfillment%2Fru%2F
)](https://yandex.ru/dev/market/fulfillment/ru/)

Данный файл содержит перечень методов API для передачи/получения данных при взаимодействии с WMS-системой DSF

Примеры и описание содержимого xml-пакетов для запросов см. <https://yandex.ru/dev/market/fulfillment/ru/>

## Дерево методов

```text
hs/ym/api/
├── health              # Проверка доступности
├── send-data           # Универсальный метод для отправки сообщений в сервис
├── inbounds            # Заявка на приемку товаров
│   ├── registries      # Реестр приемки товаров 
│   ├── statuses        # Статус приемки
│   │   └── history     # История статусов приемки
│   └── details         # Детали приемки
├── outbounds           # Заявка на отгрузку товаров
│   ├── registries      # Реестр отгрузки товаров
│   ├── statuses        # Статус отгрузки
│   │   └── history     # История статусов заявок на отгрузку
│   └── details         # Детали отгрузки
└── stocks              # Получение остатков                 
```

## Описание методов

### `Служебные`

- #### Проверка доступности сервиса на стороне склада

```text
GET /hs/ym/api/health HTTP/1.1
Host: example.com
```

- #### Универсальный, для отправки запросов на события любого типа

type `Любой из доступных`

```text
POST /hs/ym/api/send-data HTTP/1.1
Host: example.com
Content-Type: application/xml
```

### `Приемка товаров`

- #### Создать или обновить заявку на приемку

type `putInbound`

```text
POST /hs/ym/api/inbounds HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Позволяет изменять существующее состояние заявки или добавлять новую запись о планируемой поставке товара на склад.

- #### Формирование или изменение реестра приемки товаров

type `putInboundRegistry`

```text
POST /hs/ym/api/inbounds/registries HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Применяется для регистрации плана поступления товаров на склад с детализацией по номенклатуре, её качеству и количеству.

- #### Получение актуальной информации о статусе заявки на приемку

type `getInboundsStatus`

```text
POST /hs/ym/api/inbounds/statuses HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Необходим для оперативного мониторинга состояний различных заявок на приемку товаров*

- #### История изменений статуса заявки на приемку

type `getInboundHistory`

```text
POST /hs/ym/api/inbounds/statuses/history HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Хронология изменения статуса конкретной заявки на приемку товаров

- #### Подробная информация о заявке на приемку товаров

type `getInbound`

```text
POST /hs/ym/api/inbounds/details HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Детальная информация о конкретной заявке на приемку товаров

Пример

```XML
<?xml version="1.0" encoding="UTF-8"?>
<root>
    <token>464adf0d-173d-4c79-a9f2-7192141895e1</token>
    <uniq>52b43d33-e56b-477d-902a-05603fe72efe</uniq>
    <request type="getInbound">
        <inboundId>
            <yandexId>4158fad6dcda49099f7abcea7105c674</yandexId>
            <partnerId>6671b39a-9c31-4624-9ef7-9bd1e2a23589</partnerId>
        </inboundId>   
    </request>
</root>
```

### `Отгрузка товаров`

- #### Создать или обновить заявку на отгрузку (отправку) товаров

type `putOutbound`

```text
POST /hs/ym/api/outbounds HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Позволяет создавать новые и изменять существующиеся заявки на планируемую отгрузку товаров со склада.

- #### Формирование или изменение реестра отгрузки товаров

type `putOutboundRegistry`

```text
POST /hs/ym/api/outbounds/registries HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Применяется для регистрации плана отгрузки товаров на склад с детализацией по номенклатуре и количеству. Также с помощью запроса можно обновить реестр, который был создан ранее.

- #### Получение актуальной информации о статусе заявки на отгрузку

type `getOutboundStatus`

```text
POST /hs/ym/api/outbounds/statuses HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Метод позволяет узнать текущие статусы отправок

- #### История изменений статуса заявки на отправку

type `getOutboundHistory`

```text
POST /hs/ym/api/outbounds/statuses/history HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Хронология изменения статуса заявки на отправку / отгрузку товаров

- #### Подробная информация о заявке на отгрузку товаров

type `getOutbound`

```text
POST /hs/ym/api/outbounds/details HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Детальная информация об исполнении заявки на отгрузку товаров

### `Остатки товаров`

- #### Информация об остатках товарных единиц на складе

type `getStocks`

```text
POST /hs/ym/api/inbounds/stocks HTTP/1.1
Host: example.com
Content-Type: application/xml
```

Пример:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<root>
    <token>14349ed5-da71-4142-81be-e9b040ae79fe</token>
    <uniq>b23f97dd-0672-4dbd-a823-16fc89a41348</uniq>
    <request type="getStocks">
        <limit>2</limit>
        <offset>1</offset>    
    </request>
</root>
```

Данные об уровне остатков товаров на складе

---
**Автор:** rmartynenko  
**Дата создания:** 2025  
**Версия:** 1.0.3
