---
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: Используйте этот метод в API аналитики для Microsoft Store, чтобы получить подробные данные об определенной ошибке в вашем приложении.
title: Получение сведений об ошибке в приложении
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, службы Store, API аналитики для Microsoft Store, ошибки, подробные сведения
ms.localizationpriority: medium
ms.openlocfilehash: 5176e123d57b8bcc5d4981acc91b22329ad4c643
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662109"
---
# <a name="get-details-for-an-error-in-your-app"></a>Получение сведений об ошибке в приложении

Используйте этот метод в API аналитики для Microsoft Store, чтобы получить подробные данные об определенной ошибке в вашем приложении в формате JSON. Этот метод позволяет получать подробные сведения только об ошибках, возникших за последние 30 дней. Данные сведения об ошибке, также доступны в **сбоев** раздел [отчет о работоспособности](../publish/health-report.md) в центре партнеров.

Перед использованием этого метода вам потребуется воспользоваться методом [получения данных отчетов об ошибках](get-error-reporting-data.md), чтобы получить идентификатор ошибки, о которой требуется получить подробные сведения.

## <a name="prerequisites"></a>Предварительные условия


Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Microsoft Store.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.
* Получите идентификатор ошибки, о которой требуется получить подробные сведения. Чтобы получить этот идентификатор, используйте метод [получения данных отчетов об ошибках](get-error-reporting-data.md) и значение **failureHash** в тексте ответа этого метода.

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | Универсальный код ресурса (URI) запроса                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

| Параметр        | Тип   |  Описание      |  Обязательно  
|---------------|--------|---------------|------|
| applicationId | Строка | Код продукта в Магазине для приложения, по которому требуется получить подробные данные об ошибках. Идентификатор Store можно найти в [страницу удостоверения](../publish/view-app-identity-details.md) в центре партнеров. Пример кода продукта в Магазине: 9WZDNCRFJ3Q8. |  Да  |
| failureHash | Строка | Уникальный идентификатор ошибки, о которой требуется получить подробные сведения. Чтобы получить это значение для интересующей вас ошибки, используйте метод [получения данных отчетов об ошибках](get-error-reporting-data.md) и значение **failureHash** в тексте ответа этого метода. |  Да  |
| startDate | date | Начальная дата диапазона дат, для которого требуется получить подробные данные об ошибках. По умолчанию используется текущая дата минус 30 дней.<p/><p/>**Примечание:**&nbsp;&nbsp;этого метода можно получить только сведения об ошибках, произошедших за последние 30 дней. |  Нет  |
| endDate | date | Конечная дата диапазона дат, для которого требуется получить подробные данные об ошибках. По умолчанию используется текущая дата. |  Нет  |
| top | int | Количество строк данных, возвращаемых в запросе. Максимальное значение и значение по умолчанию (если параметр не указан) — 10 000. Если в запросе содержится больше строк, то тело ответа будет содержать ссылку "Далее", которую можно использовать для запроса следующей страницы данных |  Нет  |
| skip | int | Количество строк, пропускаемых в запросе. Используйте этот параметр для постраничного перемещения по большим наборам данных. Например, при top=10 и skip=0 извлекаются первые 10 строк данных; при top=10 и skip=10 извлекаются следующие 10 строк данных и т. д. |  Нет  |
| filter |Строка  | Один или несколько операторов для фильтрации строк в ответе. Каждое выражение содержит имя поля из тела ответа и значение, которое связано с помощью операторов **eq** или **ne**; выражения можно комбинировать, используя операторы **and** или **or**. В параметре *filter* строковые значения должны быть заключены в одиночные кавычки. Вы можете указать следующие поля из тела ответа:<p/><ul><li><strong>на рынке</strong></li><li><strong>Дата</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>типа устройства</strong></li><li><strong>deviceModel</strong></li><li><strong>версия ОС</strong></li><li><strong>osRelease</strong></li><li><strong>PackageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Нет   |
| orderby | Строка | Оператор, который определяет порядок полученных значений данных. Используется следующий синтаксис: <em>orderby=field [порядк.],field [порядк.],...</em>. Параметр <em>field</em> может быть одной из следующих строк:<ul><li><strong>на рынке</strong></li><li><strong>Дата</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>типа устройства</strong></li><li><strong>deviceModel</strong></li><li><strong>версия ОС</strong></li><li><strong>osRelease</strong></li><li><strong>PackageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Параметр <em>order</em> является необязательным и может принимать значения <strong>asc</strong> или <strong>desc</strong>, которые указывают, соответственно, порядок сортировки по возрастанию или по убыванию для каждого поля. Значение по умолчанию — <strong>asc</strong>.</p><p>Пример: строка <em>orderby</em>: <em>orderby=date,market</em></p> |  Нет  |


### <a name="request-example"></a>Пример запроса

В следующих примерах показано несколько запросов на получение подробных данных об ошибках. Замените значение *applicationId* кодом продукта в Магазине для вашего приложения.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ


### <a name="response-body"></a>Тело ответа

| Значение      | Тип    | Описание    |
|------------|---------|------------|
| Значение      | Массив   | Массив объектов, содержащий подробные данные об ошибках. Дополнительные сведения о данных в каждом объекте см. далее в разделе [Значения подробных сведений об ошибках](#error-detail-values).          |
| @nextLink  | Строка  | При наличии дополнительных страниц данных эта строка содержит универсальный код ресурса (URI), который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 10 000, но для данного запроса имеется больше 10 000 строк с информацией об ошибках. |
| TotalCount | целое число | Общее количество строк в результирующих данных для запроса.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Значения подробных сведений об ошибках

Элементы в массиве *Value* содержат следующие значения.

| Значение           | Тип    | Описание     |
|-----------------|---------|----------------------------|
| applicationId   | Строка  | Код продукта в Магазине для приложения, по которому требуется получить подробные данные об ошибках.      |
| failureHash     | Строка  | Уникальный идентификатор ошибки.     |
| failureName     | Строка  | Имя сбоя, состоящее из четырех частей: один или несколько классов проблемы, код проверки исключения или ошибки, имя образа, в котором произошла ошибка, и соответствующее имя функции.           |
| date            | Строка  | Первая дата в диапазоне дат, для которого требуется получить данные об ошибках. Если в запросе указан один день, это значение равно соответствующей дате. Если запрос указывает неделю, месяц или другой диапазон дат, это значение равно первой дате в этом диапазоне дат |
| cabId           | Строка  | Уникальный идентификатор CAB-файла, связанного с этой ошибкой.   |
| cabExpirationTime  | Строка  | Дата и время окончания срока действия CAB-файла, когда его больше нельзя будет скачать, в формате ISO 8601.   |
| market          | Строка  | Код страны рынка устройства по стандарту ISO 3166     |
| osBuild         | Строка  | Номер сборки операционной системы, в которой произошла ошибка.       |
| packageVersion  | Строка  | Версия пакета приложения, связанного с этой ошибкой.    |
| deviceModel           | Строка  | Строка, указывающая модель устройства, на котором выполнялось приложение в момент возникновения ошибки.   |
| osVersion       | Строка  | Одна из следующих строк, указывающая версию ОС, в которой произошла ошибка:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Неизвестный</strong></li></ul>    |
| osRelease       | Строка  |  Одна из следующих строк, задающих выпуск операционной системы или цикл фокус-тестирования (как подмножество в версиях ОС), в котором возникла ошибка.<p/><p>Для Windows 10.</p><ul><li><strong>Версии 1507</strong></li><li><strong>Версии 1511</strong></li><li><strong>Версии 1607</strong></li><li><strong>Версии 1703</strong></li><li><strong>Версия 1709</strong></li><li><strong>Версии 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider медленно</strong></li></ul><p/><p>Для Windows Server 1709</p><ul><li><strong>RTM-ВЕРСИИ</strong></li></ul><p>Для Windows Server 2016</p><ul><li><strong>Версии 1607</strong></li></ul><p>Для Windows 8.1.</p><ul><li><strong>Обновление 1</strong></li></ul><p>Для Windows 7.</p><ul><li><strong>Пакет обновления 1</strong></li></ul><p>Если выпуск операционной системы или цикл фокус-тестирования неизвестны, это поле имеет значение <strong>Unknown</strong>.</p>    |
| deviceType      | Строка  | Одна из следующих строк, указывающих тип устройства, на котором выполнялось приложение в момент возникновения ошибки:<ul><li><strong>ПК</strong></li><li><strong>Телефон</strong></li><li><strong>Консоль</strong></li><li><strong>Интернета вещей</strong></li><li><strong>Holographic</strong></li><li><strong>Неизвестный</strong></li></ul>     |
| cabDownloadable           | Boolean (Логическое)  | Указывает, может ли этот пользователь скачать CAB-файл.   |


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "deviceType": "PC",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Статьи по теме

* [Отчет о работоспособности](../publish/health-report.md)
* [Доступ к данным аналитики с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Получить данные отчетов об ошибках](get-error-reporting-data.md)
* [Получить трассировку стека для ошибки в приложении](get-the-stack-trace-for-an-error-in-your-app.md)
* [Загрузите CAB-файл для ошибки в приложении](download-the-cab-file-for-an-error-in-your-app.md)
