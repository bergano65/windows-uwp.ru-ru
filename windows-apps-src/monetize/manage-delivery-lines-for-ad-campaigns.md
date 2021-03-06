---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Используйте этот метод в API рекламных акций Microsoft Store для управления строками поставки в рекламных кампаниях.
title: Управление строками поставки
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, API рекламных акций Microsoft Store, рекламные кампании
ms.localizationpriority: medium
ms.openlocfilehash: 363f7034d7e353d9ee110637971e7b848dbca1bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625639"
---
# <a name="manage-delivery-lines"></a>Управление строками поставки

Используйте эти методы в API рекламных акций Microsoft Store, чтобы создать одну или несколько *строк поставки* для приобретения товаров и поставлять свои объявления для рекламных кампаний. Для каждого канала доставки можно задать целевую аудиторию, цену заявки, установить бюджет на допустимые траты и связать канал с нужными элементами рекламы.

Дополнительные сведения о связи между каналами доставки и кампаниями, целевыми профилями и рекламными материалами см. в разделе [Проведение рекламных кампаний с помощью служб Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Примечание**&nbsp;&nbsp;перед созданием успешно доставки строк для рекламных кампаний, этот API, необходимо сначала [создайте его, используя кампании платных ad **рекламных кампаний** странице в Центр партнеров](../publish/create-an-ad-campaign-for-your-app.md), и на этой странице необходимо добавить по крайней мере один платежного средства. После выполнения этих действий можно создавать платные строки поставки для рекламных кампаний с помощью этого API. Рекламные кампании, создаваемые с помощью API автоматически будет выставлять счета платежного средства по умолчанию, показанное на **рекламных кампаний** страницы в центре партнеров.

## <a name="prerequisites"></a>Предварительные условия

Для использования этих методов сначала необходимо сделать следующее.

* Если вы еще не сделали этого, выполните все [необходимые условия](run-ad-campaigns-using-windows-store-services.md#prerequisites) для API рекламных акций Microsoft Store.

  > [!NOTE]
  > В рамках предварительных требований, убедитесь, что вами [создайте по крайней мере одной платный рекламную кампанию в центре партнеров](../publish/create-an-ad-campaign-for-your-app.md) и добавить по крайней мере один инструмент оплаты для кампании ad в центре партнеров. Строки поставки, созданный с помощью этого API автоматически будет выставлять счета платежного средства по умолчанию, показанное на **рекламных кампаний** страницы в центре партнеров.

* [Получите маркер доступа Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этих методов. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

## <a name="request"></a>Запрос

Эти методы имеют следующие URI.

| Тип метода | Универсальный код ресурса (URI) запроса         |  Описание  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Создает новую линию поставки.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Редактирует линию поставки, заданную *lineId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Получает строку поставки, заданную *lineId*.  |


### <a name="header"></a>Заголовок

| Заголовок        | Тип   | Описание         |
|---------------|--------|---------------------|
| Authorization | Строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |
| Tracking ID   | Код GUID   | Необязательно. Идентификатор, который отслеживает поток вызовов.                                  |


### <a name="request-body"></a>Тело запроса

Методы POST и PUT требуют использовать тело запроса JSON с обязательными полями объекта [Линия поставки](#line) и любыми другими полями, которые вы хотите установить или изменить.


### <a name="request-examples"></a>Примеры запросов

Следующий пример демонстрирует вызов метода POST для создания линии поставки.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

Следующий пример демонстрирует вызов метода GET для получения линии поставки.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

Эти методы возвращают тело ответа JSON с объектом [Линия поставки](#line), содержащим сведения о линии поставки, которая была создана, обновлена или получена. В следующем примере показано тело ответа для этих методов.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>Объект линии поставки

Для этих методов тела запроса и ответа содержат следующие поля. В этой таблице показаны поля, которые доступны только для чтения (это означает, что они не могут изменяться в методе PUT), и поля, которые необходимы в теле запроса для методов POST или PUT.

| Поле        | Тип   |  Описание      |  Только чтение  | По умолчанию  | Обязательный для POST/PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  целое число   |  Идентификатор линии поставки.     |   Да    |      |  Нет      |    
|  name   |  Строка   |   Имя линии поставки.    |    Нет   |      |  POST     |     
|  configuredStatus   |  Строка   |  Одно из следующих значений, которое указывает статус линии поставки, заданный разработчиком: <ul><li>**Active**</li><li>**Неактивные**</li></ul>     |  Нет     |      |   POST    |       
|  effectiveStatus   |  Строка   |   Одно из следующих значений, определяющих действующий статус линии поставки, в зависимости от проверки системы: <ul><li>**Active**</li><li>**Неактивные**</li><li>**Обработка**</li><li>**не удалось**</li></ul>    |    Да   |      |  Нет      |      
|  effectiveStatusReasons   |  Массив   |  Одно или несколько из следующих значений, задающих причину нынешнего состояния линии поставки: <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Да     |     |    Нет    |           
|  startDatetime   |  Строка   |  Дата и время начала для линии поставки в формате ISO 8601. Невозможно изменить это значение, если оно уже в прошлом.     |    Нет   |      |    POST, PUT     |
|  endDatetime   |  Строка   |  Дата и время окончания для линии поставки в формате ISO 8601. Невозможно изменить это значение, если оно уже в прошлом.     |   Нет    |      |  POST, PUT     |
|  createdDatetime   |  Строка   |  Дата и время создания линии поставки в формате ISO 8601.      |    Да   |      |  Нет      |
|  bidType   |  Строка   |  Значение, указывающее тип ставки для линии поставки. На данный момент единственным поддерживаемым значением является **CPM**.      |    Нет   |  CPM    |  Нет     |
|  bidAmount   |  decimal   |  Сумма ставки для использования при торге за любой запрос рекламного объявления.      |    Нет   |  Среднее значение CPM в зависимости от целевых рынков (это значение периодически пересматривается).    |    Нет    |  
|  dailyBudget   |  decimal   |  Ежедневный бюджет для линии поставки. Одно из значений *dailyBudget* и *lifetimeBudget* должно быть задано.      |    Нет   |      |   POST, PUT (если *lifetimeBudget* не задано)       |
|  lifetimeBudget   |  decimal   |   Полный бюджет для линии поставки. Одно из значений lifetimeBudget* или *dailyBudget* должно быть задано.      |    Нет   |      |   POST, PUT (если *dailyBudget* не задано)    |
|  targetingProfileId   |  Объект   |  На объекте, который определяет [профиль таргетинга](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile), описывающий пользователей, регионы и типы продуктов, по которым вы хотите производить таргетинг для этой линии поставки. Этот объект состоит из одного поля *id*, которое определяет идентификатор профиля таргетинга.     |    Нет   |      |  Нет      |  
|  creatives   |  Массив   |  Один или несколько объектов, представляющих [рекламные материалы](manage-creatives-for-ad-campaigns.md#creative), связанные с линией поставки. Каждый объект в этом поле состоит из одного поля *id*, которое определяет идентификатор рекламного материала.      |    Нет   |      |   Нет     |  
|  campaignId   |  целое число   |  Идентификатор вышестоящей рекламной кампании.      |    Нет   |      |   Нет     |  
|  minMinutesPerImp   |  целое число   |  Определяет минимальный интервал времени (в минутах) между двумя показами одному пользователю из этой линии поставки.      |    Нет   |  4000    |  Нет      |  
|  pacingType   |  Строка   |  Одно из следующих значений, определяющих темп расходования: <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    Нет   |  SpendEvenly    |  Нет      |
|  currencyId   |  целое число   |  Код валюты для кампании.      |    Да   |  Валюта учетной записи разработчика (от вас не требуется указывать это поле в вызовах PUT или POST).    |   Нет     |      |


## <a name="related-topics"></a>Статьи по теме

* [Запустите рекламные кампании, с помощью служб Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Управление рекламных кампаний](manage-ad-campaigns.md)
* [Управление профилями выбора целевой платформы для рекламных кампаний](manage-targeting-profiles-for-ad-campaigns.md)
* [Управление предъявляемым для рекламных кампаний](manage-creatives-for-ad-campaigns.md)
* [Получить данные о производительности ad кампании](get-ad-campaign-performance-data.md)
