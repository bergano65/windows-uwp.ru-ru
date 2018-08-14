---
author: mcleanbyron
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Используйте этот метод в API аналитики для Microsoft Store для получения сводных данных об эффективности рекламных кампаний для определенного приложения в заданном диапазоне дат или с учетом других дополнительных фильтров.
title: Получение данных об эффективности рекламной кампании
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, службы Магазина, API аналитики для Microsoft Store, рекламные кампании
ms.localizationpriority: medium
ms.openlocfilehash: 79901ef38ca837ae547f1d25f98bb42a440c2619
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663634"
---
# <a name="get-ad-campaign-performance-data"></a>Получение данных об эффективности рекламной кампании


Используйте этот метод в API аналитики для Microsoft Store для получения сводных общих данных об эффективности рекламных кампаний для ваших приложений в заданном диапазоне дат или с учетом других дополнительных фильтров. Этот метод возвращает данные в формате JSON.

Этот метод возвращает те же данные, которые предоставляются в [отчете о рекламных объявлениях, которые привели к установке приложений](../publish/app-install-ads-reports.md) на информационной панели Центра разработки для Windows. Дополнительные сведения о рекламных кампаниях см. в разделе [Создание рекламной кампании для вашего приложения](../publish/create-an-ad-campaign-for-your-app.md).

Для создания и обновления рекламных кампаний, а также получения подробных сведений о них, вы можете использовать методы [Управление рекламными кампаниями](manage-ad-campaigns.md) в [API рекламных акций Microsoft Store](run-ad-campaigns-using-windows-store-services.md).

## <a name="prerequisites"></a>Предварительные условия

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Microsoft Store.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия маркера можно получить новый маркер.

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | URI запроса                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                |
|---------------|--------|---------------|
| Authorization | Строка | Обязательное. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

Чтобы получить данные об эффективности рекламной кампании для конкретного приложения, используйте параметр *applicationId*. Чтобы получить данные об эффективности рекламы для всех приложений, связанных с вашей учетной записью разработчика, пропустите параметр *applicationId*.

| Параметр     | Тип   | Описание     | Обязательный |
|---------------|--------|-----------------|----------|
| applicationId   | строка    | [Код продукта в Store](in-app-purchases-and-trials.md#store-ids) для приложения, по которому требуется получить данные об эффективности рекламной кампании. |    Нет      |
|  startDate  |  date   |  Начальная дата в диапазоне дат извлекаемых данных об эффективности рекламной кампании в формате ГГГГ/ММ/ДД. По умолчанию используется текущая дата минус 30 дней.   |   Нет    |
| endDate   |  date   |  Конечная дата в диапазоне дат извлекаемых данных об эффективности рекламной кампании в формате ГГГГ/ММ/ДД. По умолчанию используется текущая дата минус один день.   |   Нет    |
| top   |  целое число   |  Количество строк данных, возвращаемых в запросе. Максимальное значение и значение по умолчанию (если параметр не указан) — 10 000. Если в запросе содержится больше строк, то тело ответа будет содержать ссылку «Далее», которую можно использовать для запроса следующей страницы данных   |   Нет    |
| skip   | int    |  Количество строк, пропускаемых в запросе. Используйте этот параметр для постраничного перемещения по большим наборам данных. Например, при top=10000 и skip=0 извлекаются первые 10 000 строк данных; при top=10000 и skip=10000 извлекаются следующие 10 000 строк данных и т. д.   |   Нет    |
| filter   |  строка   |  Один или несколько операторов для фильтрации строк в ответе. **campaignId**— это единственный поддерживаемый фильтр. Каждый оператор может использовать операторы **eq** или **ne**; кроме того, операторы можно объединять с помощью **и** или **или**.  Ниже приводится пример параметра *filter*: ```filter=campaignId eq '100023'```.   |   Нет    |
|  aggregationLevel  |  строка   | Определяет диапазон времени, для которого требуется получить сводные данные. Можно использовать следующие строки: <strong>day</strong>, <strong>week</strong> или <strong>month</strong>. Если параметр не задан, значение по умолчанию — <strong>day</strong>    |   Нет    |
| orderby   |  строка   |  <p>Оператор, который определяет порядок значений результирующих данных об эффективности рекламной кампании. Используется следующий синтаксис: <em>orderby=field [order],field [order],...</em>, где параметр <em>field</em> параметр может принимать одно из следующих строковых значений:</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>Параметр <em>order</em> является необязательным и может принимать значения <strong>asc</strong> или <strong>desc</strong>, которые указывают, соответственно, порядок сортировки по возрастанию или по убыванию для каждого поля. Значение по умолчанию — <strong>asc</strong>.</p><p>Пример: строка <em>orderby</em>: <em>orderby=date,campaignId</em></p>   |   Нет    |
|  groupby  |  строка   |  <p>Оператор, который применяет агрегирование данных только к указанным полям. Можно указать следующие поля:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>Параметр <em>groupby</em> можно использовать вместе с параметром <em>aggregationLevel</em>. Например: <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   Нет    |


### <a name="request-example"></a>Пример запроса

В следующем примере демонстрируется несколько запросов на получение данных об эффективности рекламной кампании.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ


### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание  |
|------------|--------|---------------|
| Значение      | массив  | Массив объектов, содержащий сводные данные об эффективности рекламной кампании. Дополнительные сведения о данных в каждом объекте см. далее в разделе [Объект эффективности кампании](#campaign-performance-object).          |
| @nextLink  | строка | При наличии дополнительных страниц данных эта строка содержит URI-адрес, который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 5, но для данного запроса имеется больше 5 элементов данных. |
| TotalCount | целое число    | Общее количество строк в результирующих данных для запроса.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>Объект эффективности кампании

Элементы в массиве *Значение* содержат следующие значения.

| Значение               | Тип   | Описание            |
|---------------------|--------|------------------------|
| date                | строка | Первая дата в диапазоне дат для данных об эффективности рекламной кампании. Если в запросе указан один день, это значение равно соответствующей дате. Если запрос указывает неделю, месяц или другой диапазон дат, это значение равно первой дате в этом диапазоне дат |
| applicationId       | строка | Код продукта в Магазине для приложения, для которого извлекаются данные об эффективности рекламной кампании.                     |
| campaignId     | строка | Идентификатор рекламной кампании.           |
| lineId     | строка |    Идентификатор [линии поставки](manage-delivery-lines-for-ad-campaigns.md) рекламной кампании, которая сформировала эти данные об эффективности.        |
| currencyCode              | строка | Код валюты бюджета компании.              |
| spend          | строка |  Сумма бюджета, затрачиваемая на рекламную кампанию.     |
| impressions           | long | Количество показов рекламы для кампании.        |
| installs              | long | Число установок приложения, связанных с кампанией.   |
| clicks            | long | Количество нажатий рекламы для кампании.      |
| iapInstalls            | long | Число установок надстроек (также называемых покупками из приложения или IAP) в рамках кампании.      |
| activeUsers            | long | Число пользователей, перешедших по объявлению в составе кампании и вернувшихся в приложение.      |


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>Статьи по теме

* [Создание рекламной кампании для вашего приложения](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [Проведение рекламных кампаний с помощью служб Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Доступ к аналитическим данным с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)