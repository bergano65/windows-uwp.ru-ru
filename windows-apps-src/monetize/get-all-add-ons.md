---
author: mcleanbyron
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: Используйте этот метод в API отправки в Microsoft Store для получения всех данных надстроек для всех приложений, которые зарегистрированы в вашей учетной записи Центра разработки для Windows.
title: Получение всех надстроек
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, API отправки в Microsoft Store, надстройки, продукты внутри приложения, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 69ec59a39c91152788f757beb56afc75e7191922
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663124"
---
# <a name="get-all-add-ons"></a>Получение всех надстроек

Используйте этот метод в API отправки в Microsoft Store для получения данных по всем надстройкам для всех приложений, которые зарегистрированы в вашей учетной записи Центра разработки для Windows.

## <a name="prerequisites"></a>Необходимые условия

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки в Microsoft Store.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия маркера можно получить новый маркер.

## <a name="request"></a>Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | URI запроса                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Строка | Обязательный. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

Для данного метода все параметры запроса являются необязательными. При вызове этого метода без параметров ответ содержит данные для всех надстроек для всех приложений, которые зарегистрированы в вашей учетной записи.

|  Параметр  |  Тип  |  Описание  |  Обязательный  |
|------|------|------|------|
|  top  |  int  |  Число элементов, возвращаемых в запросе (т. е., количество возвращаемых надстроек). Если количество надстроек в вашей учетной записи больше значения, указанного в запросе, текст ответа будет содержать относительный путь URI, который можно добавить в URI метода, чтобы запросить следующую страницу данных.  |  Нет  |
|  skip  |  int  |  Число элементов, которые требуется пропустить в запросе перед возвратом оставшихся элементов. Используйте этот параметр для постраничного перемещения по наборам данных. Например, если задано top = 10 и skip = 0, извлекаются элементы с 1 по 10; если задано top = 10 и skip = 10, извлекаются элементы с 11 по 20 и т. д.  |  Нет  |


### <a name="request-body"></a>Тело запроса

Предоставлять тело запроса для этого метода не требуется.

### <a name="request-examples"></a>Примеры запросов

В следующем примере демонстрируется способ извлечения данных всех надстроек для всех приложений, которые зарегистрированы в вашей учетной записи.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

В следующем примере демонстрируется способ извлечения только первых 10 надстроек.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

В следующем примере показано тело ответа JSON, возвращаемое успешным запросом первых 5 надстроек, которые зарегистрированы в учетной записи разработчика, содержащей всего 1072 надстройки. Для краткости в этом примере показаны данные только первых двух надстроек, возвращенных в запросе. Дополнительные сведения о значениях, которые могут содержаться в теле ответа, см. в следующем разделе.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMP",
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | При наличии дополнительных страниц данных эта строка содержит относительный путь, который можно добавить к базовому URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` запроса, чтобы запросить следующую страницу данных. Например, если для параметра *top* в тексте исходного запроса задано значение 10, но в учетной записи зарегистрировано 100 надстроек, тело ответа будет содержать значение @nextLink ```inappproducts?skip=10&top=10```, которое указывает, что можно вызвать ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10``` для запроса следующих 10 надстроек. |
| value            | array  |  Массив, содержащий объекты, которые предоставляют сведения о каждой надстройке. Дополнительные сведения см. в разделе [Ресурс надстройки](manage-add-ons.md#add-on-object).   |
| totalCount   | int  | Количество объектов приложения в массиве *value* тела ответа.     |


## <a name="error-codes"></a>Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 404  | Надстройки не найдены. |
| 409  | Приложения или надстройки используют функции информационной панели Центра разработки, которые [в настоящее время не поддерживаются API отправки в Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Связанные разделы

* [Создание отправок и управление ими с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Управление отправками надстроек](manage-add-on-submissions.md)
* [Получение надстройки](get-an-add-on.md)
* [Создание надстройки](create-an-add-on.md)
* [Удаление надстройки](delete-an-add-on.md)