
# Yndx-DSF-WMS API Integration

[![DSF logo](https://img.shields.io/badge/DSF%20Trading-blue?link=https%3A%2F%2Fdsf.kz%2F
)](https://dsf.kz)
[![Extension version](https://img.shields.io/badge/extension-1.0.1.18-blue
)](https://dsf.kz)
[![MarketApi](https://img.shields.io/badge/%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5%20API%20%D0%B4%D0%BB%D1%8F%20%D0%B2%D0%B7%D0%B0%D0%B8%D0%BC%D0%BE%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D0%B8%D1%8F-xml-green?style=flat-square&logo=%23F9AB00&link=https%3A%2F%2Fyandex.ru%2Fdev%2Fmarket%2Ffulfillment%2Fru%2F
)](https://yandex.ru/dev/market/fulfillment/ru/)

Данный файл содержит перечень методов API для передачи/получения данных при взаимодействии с WMS-системой DSF

Примеры и описание содержимого xml-пакетов для запросов см. <https://yandex.ru/dev/market/fulfillment/ru/>

## Дерево методов

```text
┌── health                  # Проверка доступности
├── send-data               # Универсальный метод для отправки сообщений в сервис
├── v1
│   ├── inbounds            # Заявка на приемку товаров [putInbound]
│   │   ├── registries      # Реестр приемки товаров [putInboundRegistry]
│   │   ├── statuses        # Статус приемки [getInboundStatus]
│   │   │   └── history     # История статусов приемки [getInboundStatusHistory]
│   │   └── details         # Детали приемки [getInbound]
│   ├── outbounds           # Заявка на отгрузку товаров [putOutbound]
│   │   ├── registries      # Реестр отгрузки товаров [putOutboundRegistry]
│   │   ├── statuses        # Статус отгрузки [getOutboundStatus]
│   │   │   └── history     # История статусов заявок на отгрузку [getOutboundStatusHistory]
│   │   └── details         # Детали отгрузки [getOutbound]
│   └── stocks              # Получение остатков [getStocks]
└── v2
    ├── inbounds            
    │   └── statuses        # Статус приемки [getInboundsStatus]
    │       └── history     # История статусов приемки [getInboundHistory]
    └── outbounds           
        └── statuses        # Статус отгрузки [getOutboundsStatus]
            └── history     # История статусов заявок на отгрузку [getOutboundHistory]
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

*Позволяет изменять существующее состояние заявки или добавлять новую запись о планируемой поставке товара на склад.*

type `putInbound`

```text
POST /hs/ym/api/putInbound HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### Формирование или изменение реестра приемки товаров

*Применяется для регистрации плана поступления товаров на склад с детализацией по номенклатуре, её качеству и количеству.*

type `putInboundRegistry`

```text
POST /hs/ym/api/putInboundRegistry HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### Получение актуальной информации о статусе заявки на приемку

*Необходим для оперативного мониторинга состояний различных заявок на приемку товаров.*

type `getInboundStatus`

```text
POST /hs/ym/api/getInboundStatus HTTP/1.1
Host: example.com
Content-Type: application/xml
```

type `getInboundsStatus`

```text
POST /hs/ym/api/getInboundsStatus HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### История изменений статуса заявки на приемку

*Хронология изменения статуса конкретной заявки на приемку товаров.*

type `getInboundStatusHistory`

```text
POST /hs/ym/api/getInboundStatusHistory HTTP/1.1
Host: example.com
Content-Type: application/xml
```

type `getInboundHistory`

```text
POST /hs/ym/api/getInboundHistory HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### Подробная информация о заявке на приемку товаров

*Детальная информация о конкретной заявке на приемку товаров.*

type `getInbound`

```text
POST /hs/ym/api/getInbound HTTP/1.1
Host: example.com
Content-Type: application/xml
```

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

*Позволяет создавать новые и изменять существующиеся заявки на планируемую отгрузку товаров со склада.*

type `putOutbound`

```text
POST /hs/ym/api/putOutbound HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### Формирование или изменение реестра отгрузки товаров

*Применяется для регистрации плана отгрузки товаров на склад с детализацией по номенклатуре и количеству. Также с помощью запроса можно обновить реестр, который был создан ранее.*

type `putOutboundRegistry`

```text
POST /hs/ym/api/putOutboundRegistry HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### Получение актуальной информации о статусе заявки на отгрузку

*Метод позволяет узнать текущие статусы одной или нескольких отправок.*

type `getOutboundStatus`

```text
POST /hs/ym/api/getOutboundStatus HTTP/1.1
Host: example.com
Content-Type: application/xml
```

type `getOutboundsStatus`

```text
POST /hs/ym/api/getOutboundsStatus HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### История изменений статуса заявки на отправку

*Хронология изменения статуса заявки на отправку / отгрузку товаров.*

type `getOutboundStatusHistory`

```text
POST /hs/ym/api/getOutboundStatusHistory HTTP/1.1
Host: example.com
Content-Type: application/xml
```

type `getOutboundHistory`

```text
POST /hs/ym/api/getOutboundHistory HTTP/1.1
Host: example.com
Content-Type: application/xml
```

- #### Подробная информация о заявке на отгрузку товаров

*Детальная информация об исполнении заявки на отгрузку товаров.*

type `getOutbound`

```text
POST /hs/ym/api/getOutbound HTTP/1.1
Host: example.com
Content-Type: application/xml
```

### `Остатки товаров`

- #### Информация об остатках товарных единиц на складе

*Данные об уровне остатков товаров на складе.*

type `getStocks`

```text
POST /hs/ym/api/getStocks HTTP/1.1
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

---
**Автор:** rmartynenko  
**Дата создания:** 2025  
**Версия:** 1.0.6
