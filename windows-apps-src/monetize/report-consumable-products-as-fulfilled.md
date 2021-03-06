---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Используйте этот метод в API коллекции Microsoft Store, чтобы объявить потребляемый продукт в качестве выполненного для указанного покупателя. Перед повторной покупкой продукта пользователем ваше приложение или служба должны сообщить о нем как о выполненном для этого пользователя.
title: Объявление потребляемого продукта в качестве выполненного
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, API коллекции Microsoft Store, исполнение, потребляемый продукт
ms.localizationpriority: medium
ms.openlocfilehash: 994113abc34a0a5f7905bff00aa77c6785409927
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372772"
---
# <a name="report-consumable-products-as-fulfilled"></a>Объявление потребляемого продукта в качестве выполненного

Используйте этот метод в API коллекции Microsoft Store, чтобы объявить потребляемый продукт в качестве выполненного для указанного покупателя. Перед повторной покупкой продукта пользователем ваше приложение или служба должны сообщить о нем как о выполненном для этого пользователя.

Существует два способа использования данного метода, которые позволяют объявить потребляемый продукт в качестве выполненного.

* Укажите код товара (он возвращается параметром **itemId**[запроса продуктов](query-for-products.md)) и предоставляемый вами уникальный ИД отслеживания. Если один и тот же ИД отслеживания будет использован несколько раз, будет возвращен тот же самый результат, даже если элемент уже потреблен. Если вы не уверены что запрос потребления был выполнен успешно, ваша служба должна повторно отправить запросы потребления с тем же самым ИД отслеживания. ИД отслеживания всегда будет привязан к этому запросу потребления и может отправляться неограниченное количество раз.
* Укажите код продукта (он возвращается параметром **productId**[запроса продуктов](query-for-products.md)) и код транзакции, получаемый от одного или нескольких источников, перечисленных в описании параметра **transactionId** в разделе «Текст запроса» этой статьи.

## <a name="prerequisites"></a>Предварительные требования


Для использования этого метода вам понадобится:

* Маркер доступа Azure AD, имеющий значение URI аудитории `https://onestore.microsoft.com`.
* Ключ идентификатора Microsoft Store, представляющий удостоверение пользователя, для которого потребляемый продукт необходимо объявить в качестве выполненного.

Дополнительные сведения см. в разделе [Управление правами на продукты из службы](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | Универсальный код ресурса (URI) запроса                                                   |
|--------|---------------------------------------------------------------|
| ПОМЕСТИТЬ   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>Заголовок запроса

| Header         | Тип   | Описание                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;.                           |
| Hyper-V           | строка | Должен иметь значение **collections.mp.microsoft.com**.                                            |
| Content-Length | number | Длина текста запроса.                                                                       |
| Content-Type   | строка | Указывает тип запросов и ответов. На данный момент единственным поддерживаемым значением является **application/json**. |


### <a name="request-body"></a>Тело запроса

| Параметр     | Тип         | Описание         | Обязательно |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | Пользователь, для которого выполняется потребление этого элемента. Подробная информация приводится в следующей таблице.        | Да      |
| itemId        | строка       | Значение *itemId*, возвращаемое [запросом продуктов](query-for-products.md). Используйте этот параметр с *trackingId*      | Нет       |
| trackingId    | guid         | Уникальный ИД отслеживания, предоставляемый разработчиком. Используйте этот параметр с *itemId*.         | Нет       |
| productId     | строка       | Значение *productId*, возвращаемое [запросом продуктов](query-for-products.md). Используйте этот параметр с *transactionId*   | Нет       |
| transactionId | guid         | Код транзакции, получаемый от одного следующих источников. Используйте этот параметр с *productId*.<ul><li>Свойство [TransactionID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) класса [PurchaseResults](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.PurchaseResults).</li><li>Квитанция приложения или продукта, возвращаемая [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) или [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync).</li><li>Параметр *transactionId*, возвращаемый [запросом продуктов](query-for-products.md).</li></ul>   | Нет       |


Объект UserIdentity содержит следующие параметры.

| Параметр            | Тип   | Описание       | Обязательно |
|----------------------|--------|-------------------|----------|
| identityType         | строка | Укажите строковое значение **b2b**.    | Да      |
| identityValue        | строка | [Ключ идентификатора Microsoft Store](view-and-grant-products-from-a-service.md#step-4), представляющий удостоверение пользователя, для которого потребляемый продукт необходимо объявить в качестве выполненного.      | Да      |
| localTicketReference | строка | Запрошенный идентификатор для возвращаемого ответа. Мы рекомендуем использовать то же значение, что *userId*[утверждения](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key) в Microsoft Store идентификатор ключа. | Да      |


### <a name="request-examples"></a>Примеры запросов

В следующем примере используются *itemId* и *trackingId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

В следующем примере используются *productId* и *transactionId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>Ответ

Если потребление выполнено успешно, никакие данные не возвращаются.

### <a name="response-example"></a>Пример ответа

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>Коды ошибок


| Код | Ошибка        | Внутренний код ошибки           | Описание           |
|------|--------------|----------------------------|-----------------------|
| 401  | Недостаточно прав | AuthenticationTokenInvalid | Маркер доступа Azure AD недействителен. В некоторых случаях сведения об ошибке ServiceError содержат больше информации, например если истек срок действия маркера или отсутствует утверждение *appid*. |
| 401  | Недостаточно прав | PartnerAadTicketRequired   | Маркер доступа Azure AD не был передан службе в заголовке авторизации.                                                                                                   |
| 401  | Недостаточно прав | InconsistentClientId       | Утверждение *clientId* в ключе идентификатора Microsoft Store в теле запроса и утверждение *appid* в маркере доступа Azure AD в заголовке авторизации не совпадают.                     |

<span/> 

## <a name="related-topics"></a>См. также

* [Управление прав продукта из службы](view-and-grant-products-from-a-service.md)
* [Запрос для продуктов](query-for-products.md)
* [Предоставьте бесплатные продукты](grant-free-products.md)
* [Обновить ключ идентификатор Microsoft Store](renew-a-windows-store-id-key.md)
