---
author: mcleanbyron
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: Используйте этот метод в API отправки в Microsoft Store для получения информации о всех приложениях, которые зарегистрированы в вашей учетной записи Центра разработки для Windows.
title: Получение всех приложений
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, API отправки в Microsoft Store, приложения
ms.localizationpriority: medium
ms.openlocfilehash: bf2e7bb5e809d975c7217118ebc36409a54061c9
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816139"
---
# <a name="get-all-apps"></a>Получение всех приложений


Используйте этот метод в API отправки в Microsoft Store для получения данных для всех приложений, которые зарегистрированы в вашей учетной записи Центра разработки для Windows.

## <a name="prerequisites"></a>Необходимые условия

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки в Microsoft Store.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия маркера можно получить новый маркер.

## <a name="request"></a>Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | URI запроса                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Авторизация | Строка | Обязательный. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

Для данного метода все параметры запроса являются необязательными. При вызове этого метода без параметров ответ содержит данные для всех приложений, которые зарегистрированы в вашей учетной записи.

|  Параметр  |  Тип  |  Описание  |  Обязательный  |
|------|------|------|------|
|  top  |  int  |  Число элементов, возвращаемых в запросе (т. е., количество возвращаемых приложений). Если количество приложений в вашей учетной записи больше значения, указанного в запросе, текст ответа будет содержать относительный путь URI, который можно добавить в URI метода, чтобы запросить следующую страницу данных.  |  Нет  |
|  skip  |  int  |  Число элементов, которые требуется пропустить в запросе перед возвратом оставшихся элементов. Используйте этот параметр для постраничного перемещения по наборам данных. Например, если задано top = 10 и skip = 0, извлекаются элементы с 1 по 10; если задано top = 10 и skip = 10, извлекаются элементы с 11 по 20 и т. д.  |  Нет  |


### <a name="request-body"></a>Тело запроса

Предоставлять тело запроса для этого метода не требуется.

### <a name="request-examples"></a>Примеры запросов

В следующем примере демонстрируется способ извлечения информации о всех приложениях, которые зарегистрированы в вашей учетной записи.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

В следующем примере демонстрируется способ извлечения первых 10 приложений, которые зарегистрированы в вашей учетной записи.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

В следующем примере показано тело ответа JSON, возвращаемое успешным запросом первых 10 приложений, которые зарегистрированы в учетной записи разработчика, содержащей всего 21 приложение. Для краткости в этом примере показаны данные только первых двух приложений, возвращенных в запросе. Дополнительные сведения о значениях, которые могут содержаться в теле ответа, см. в следующем разделе.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | array  | Массив объектов, содержащих сведения о каждом приложении, зарегистрированном в вашей учетной записи. Дополнительные сведения о данных в каждом объекте см. в разделе [Ресурс приложения](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | string | При наличии дополнительных страниц данных эта строка содержит относительный путь, который можно добавить к базовому URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` запроса, чтобы запросить следующую страницу данных. Например, если для параметра *top* в тексте исходного запроса задано значение 10, но в учетной записи зарегистрировано 20 приложений, тело ответа будет содержать значение @nextLink ```applications?skip=10&top=10```, которое указывает, что можно вызвать ```https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10``` для запроса следующих 10 приложений. |
| totalCount | int    | Общее количество строк в результирующих данных для запроса (т. е., общее число приложений, которые зарегистрированы в вашей учетной записи).                                                |


## <a name="error-codes"></a>Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 404  | Приложения не найдены. |
| 409  | Приложения используют функции информационной панели Центра разработки, которые [в настоящее время не поддерживаются API отправки в Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Статьи по теме

* [Создание отправок и управление ими с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Получение приложения](get-an-app.md)
* [Получение тестовых пакетов для приложения](get-flights-for-an-app.md)
* [Получение надстроек для приложения](get-add-ons-for-an-app.md)