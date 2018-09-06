---
author: Xansky
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Используйте этот метод в API аналитики для Microsoft Store для получения ежемесячных данные об использовании приложений в заданном диапазоне дат и других дополнительных фильтров.
title: Получение ежемесячные использование приложений
ms.author: mhopkins
ms.date: 08/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, службы хранилища, хранилища Microsoft analytics API, использование
ms.localizationpriority: medium
ms.openlocfilehash: ad45422dea9b0c4335fa3cf67a594f819a60ca9c
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2018
ms.locfileid: "3382740"
---
# <a name="get-monthly-app-usage"></a>Получение ежемесячные использование приложений

Используйте этот метод в интерфейсе API Microsoft Store analytics для получения приложения во время указанного диапазона дат (за последние 90 дней только) и другие необязательные фильтры (не включая многопользовательские Xbox) статистические данные в формате JSON. Эта информация также доступна в [отчет об использовании](../publish/usage-report.md) в панели мониторинга Центр разработчиков Windows.

## <a name="prerequisites"></a>Предварительные условия

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Microsoft Store.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия маркера можно получить новый маркер.

## <a name="request"></a>Запрос

### <a name="request-syntax"></a>Синтаксис запроса

| Метод | URI запроса                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Обязательный. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

| Параметр     | Тип   |  Описание                                                                                                    |  Обязательный  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | string | [Код продукта в Store](in-app-purchases-and-trials.md#store-ids) для приложения, по которому требуется получить данные об отзывах. |  Да       |
| startDate     | date   | Начальная дата диапазона дат, для которого требуется получить данные об отзывах. По умолчанию используется текущая дата                   |  Нет        |
| endDate       | date   | Конечная дата диапазона дат, для которого требуется получить данные об отзывах. По умолчанию используется текущая дата                     |  Нет        |
| top           | int    | Количество строк данных, возвращаемых в запросе. Максимальное значение и значение по умолчанию (если параметр не указан) — 10 000. Если в запросе содержится больше строк, то тело ответа будет содержать ссылку «Далее», которую можно использовать для запроса следующей страницы данных                          |  Нет        |
| skip          | int    | Количество строк, пропускаемых в запросе. Используйте этот параметр для постраничного перемещения по большим наборам данных. Например, при top=10000 и skip=0 извлекаются первые 10 000 строк данных; при top=10000 и skip=10000 извлекаются следующие 10 000 строк данных и т. д.                         |  Нет        |  
| filter        |string  | Одно или несколько выражений для фильтрации строк в ответе. Каждое выражение содержит имя поля из тела ответа и значение, которое связано с помощью операторов eq или ne; выражения можно комбинировать, используя операторы and или or. В параметре filter строковые значения должны быть заключены в одиночные кавычки. Вы можете указать следующие поля из тела ответа: <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion.**</li></ul>                                                                                                                                              | Нет         |  
| orderby       | string | Выражение, которое определяет порядок полученных значений данных. Используется следующий синтаксис: <em>orderby=field [order],field [order],...</em>, где параметр <em>field</em> может принимать одно из следующих строковых значений:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Параметр <em>order</em> является необязательным и может принимать значения **asc** или **desc**, которые указывают, соответственно, порядок сортировки по возрастанию или по убыванию для каждого поля. Значение по умолчанию — **asc**.</p><p>Пример: строка <em>orderby</em>: <em>orderby=date,market</em></p> |  Нет        |
| groupby       | string | Выражение, которое применяет агрегирование данных только к указанным полям. Вы можете указать следующие поля из тела ответа:<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>Возвращенные строки данных будут содержать поля, указанные в параметре <em>groupby</em>, а также:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Параметр <em>groupby</em> можно использовать вместе с параметром <em>aggregationLevel</em>. Например: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Нет        |


### <a name="request-example"></a>Пример запроса

В следующем примере показано запроса на получение ежемесячные данные об использовании приложений. Замените значение *applicationId* кодом продукта в Магазине для вашего приложения.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ


### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Значение      | array  | Массив объектов, содержащих статистические данные. Дополнительные сведения о данных в каждом объекте см. в следующей таблице. |
| @nextLink  | string | При наличии дополнительных страниц данных эта строка содержит URI-адрес, который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 10 000, но для данного запроса имеется больше 10 000 строк с информацией об отзывах.                 |
| TotalCount | int    | Общее количество строк в результирующих данных для запроса.                                                                          |

 
### <a name="usage-values"></a>Использование значений

Элементы в массиве *Value* содержат следующие значения.

| Значение                     | Тип    | Описание                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | string  | Первая дата в диапазон дат для данных об использовании. Если в запросе указан один день, это значение равно соответствующей дате. Если запрос указывает неделю, месяц или другой диапазон дат, это значение равно первой дате в этом диапазоне дат                          |
| applicationId             | string  | КОД магазина приложения, для которого необходимо извлечь данные об использовании.                            |
| applicationName           | string  | Отображаемое имя приложения.                                                                |
| market                    | string  | Код страны ISO 3166 рынка, где используется приложение клиента.                   |
| packageVersion            | string  | Версия пакета, где произошло использование.                                            |
| deviceType                | string  | Одна из следующих строк, задающий тип устройства, где произошло использования:<ul><li>**Компьютер**</li><li>**Телефон**</li><li>**Console (консоль),**</li><li>**Планшет**</li><li>**IoT**</li><li>**Server**</li><li>**Holographic (голография),**</li><li>**Unknown (неизвестно).**</li></ul>                                                                                                                           |
| subscriptionName          | строка  | Указывает, было ли использование через проход игры Xbox.                                              |
| monthlySessionCount       | long    | Количество сеансов пользователей во время соответствующего месяца.                                              |
| engagementDurationMinutes | double  | Минут, где активно используется приложение измеренное различных период времени начинается при запуске приложения (Начало процесса) и заканчивается при прерывании (завершение процесса) или после определенного периода бездействия пользователей.                               |
| monthlyActiveUsers        | long    | Количество клиентов, использующих приложение месяца.                                           |
| monthlyActiveDevices      | long    | Количество устройств, выполнение приложений для различных периода времени, начинается при запуске приложения (Начало процесса) и заканчивается, когда она завершается (завершение процесса) или после определенного периода бездействия.                                                        |
| monthlyNewUsers           | long    | Число клиентов, используется приложение в первый раз этот месяц.                    |
| averageDailyActiveUsers   | double  | Среднее число клиентов, использующих приложение ежедневно.                             |
| averageDailyActiveDevices | double  | Среднее количество устройств, используемых для взаимодействия с вашим приложением по всем пользователям ежедневно. |


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Статьи по теме

* [Доступ к аналитическим данным с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Получение ежедневных ussage приложения](get-app-usage-daily.md)
* [Получение сведений о покупках приложения](get-app-acquisitions.md)
* [Получение сведений о покупках надстройки](get-in-app-acquisitions.md)
* [Получение данных отчетов об ошибках](get-error-reporting-data.md)