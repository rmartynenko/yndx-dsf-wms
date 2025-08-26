# Yndx-DSF-WMS API Integration

Данный файл содержит перечень методов API для передачи/получения данных при взаимодействии с WMS-системой DSF

# Дерево методов

```
hs/ym/api/
├── health              # Проверка доступности
├── send-data           # Универсальный метод для отправки сообщений в сервис
├── inbounds            # Заявка на приемку товаров
│   ├── registries      # Реестр приемки товаров 
│   ├── statuses        # Статус приемки
│   │   └── history     # История статусов приемки
│   └── details         # Детали приемки
├── outbound            # Заявка на отгрузку товаров
└── stocks              # Получение остатков                 
```

# Описание методов

## `Служебные`
- ### Проверка доступности сервиса на стороне склада
```
GET /hs/ym/api/health HTTP/1.1
Host: example.com
```
- ### Универсальный, для отправки запросов на события любого типа
type `Любой из доступных`
```
POST /hs/ym/api/send-data HTTP/1.1
Host: example.com
Content-Type: application/xml
```

## `Приемка товаров`
- ### Создать или обновить заявку на приемку
type `putInbound`
```
POST /hs/ym/api/inbounds HTTP/1.1
Host: example.com
Content-Type: application/xml
```
*Позволяет изменять существующее состояние заявки или добавлять новую запись о планируемой поставке товара на склад.*

- ### Формирование или изменение реестра приемки товаров
type `putInboundRegistry`
```
POST /hs/ym/api/inbounds/registries HTTP/1.1
Host: example.com
Content-Type: application/xml
```
*Применяется для регистрации плана поступления товаров на склад с детализацией по номенкдатуре, её качеству и количеству.*

- ### Получение актуальной информации о статусе заявки на приемку
type `getInboundsStatus`
```
POST /hs/ym/api/inbounds/statuses HTTP/1.1
Host: example.com
Content-Type: application/xml
```
*Необходим для оперативного мониторинга состояний различных заявок на приемку товаров*

- ### История изменений статуса заявки на приемку
type `getInboundHistory`
```
POST /hs/ym/api/inbounds/statuses/history HTTP/1.1
Host: example.com
Content-Type: application/xml
```
*Хронология изменения статуса конкретной заявки на приемку товаров*

- ### Подробная информация о заявке на примку товаров
type `getInbound`
```
POST /hs/ym/api/inbounds/details HTTP/1.1
Host: example.com
Content-Type: application/xml
```
*Детальная инфоромация о конкретной заявке на приемку товаров*

## `Отгрузка товаров`

- ### Создать или обновить заявку на отгрузку (отправку) товаров
type `putOutbound`
```
POST /hs/ym/api/outbounds HTTP/1.1
Host: example.com
Content-Type: application/xml
```
*Позволяет создавать неовые и изменять существующиеся заявки на планируемую отгрузку товаров со склада.*

## `Остатки товаров`
- ### Информация об остатках товарных единиц на складе
type `getStocks`
```
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
*Данные об уровне остатков товаров на складе*

---
**Автор:** rmartynenko  
**Дата создания:** 2025  
**Версия:** 1.0.2