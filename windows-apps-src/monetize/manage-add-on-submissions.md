---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Используйте эти методы в API отправки Microsoft Store, чтобы управлять отправкой надстроек для приложений, зарегистрированных в учетной записи центра партнеров.
title: Управление отправками надстроек
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API отправки в Microsoft Store, отправки надстроек, продукт внутри приложения, IAP
ms.localizationpriority: medium
ms.openlocfilehash: c8204382a4e341083ce825a9424181cdd75771e1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260240"
---
# <a name="manage-add-on-submissions"></a>Управление отправками надстроек

API отправки в Microsoft Store предоставляет методы, которые можно использовать для управления отправками надстроек (также называются внутренним продуктом приложения или IAP) для ваших приложений. Введение в API отправки в Microsoft Store, включая необходимые условия для использования этого API, см. в разделе [Создание отправок и управление ими с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> При использовании API отправки Microsoft Store для создания отправки надстройки необходимо внести дальнейшие изменения в отправку только с помощью API, а не вносить изменения в центре партнеров. Если вы используете Центр партнеров для изменения отправки, изначально созданной с помощью API, вы больше не сможете изменить или зафиксировать эту отправку с помощью API. В некоторых случаях отправка может оставаться в состоянии ошибки, когда продолжить процесс отправки невозможно. В этом случае необходимо удалить отправку и создать новую.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Методы для управления отправками надстроек

Используйте следующие методы для получения, создания, обновления, фиксации и удаления отправки надстройки. Прежде чем использовать эти методы, надстройка должна уже существовать в учетной записи центра партнеров. Вы можете создать надстройку в центре партнеров, [определив ее тип продукта и идентификатор продукта](../publish/set-your-add-on-product-id.md) или используя методы API отправки Microsoft Store, описанные в разделе [Управление надстройками](manage-add-ons.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Метод</th>
<th align="left">URI</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Получить существующую отправку надстройки</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Получение состояния существующей отправки надстройки</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Создание новой отправки надстройки</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Обновить существующую отправку надстройки</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Фиксация новой или обновленной отправки надстройки</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Удаление отправки надстройки</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Создание отправки надстройки

Чтобы создать отправку для надстройки, выполните следующие действия.

1. Если вы еще не сделали этого, выполните предварительные требования, описанные в статье [Создание и управление отправкой с помощью Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md), включая связывание приложения Azure AD с учетной записью центра партнеров и получение идентификатора и ключа клиента. Это необходимо сделать только один раз; после получения идентификатора клиента и ключа с их помощью можно в любой момент создать новый маркер доступа Azure AD.  

2. [Получение токена доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Этот маркер доступа необходимо передавать методам из API отправки в Microsoft Store. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

3. Выполните следующий метод в API отправки в Microsoft Store. Этот метод создает новую выполняющуюся отправку, которая является копией последней опубликованной отправки. Дополнительные сведения см. в разделе [Создание отправки надстройки](create-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    Тело ответа содержит ресурс [отправки надстройки](#add-on-submission-object), включающий в себя идентификатор и данные новой отправки (в том числе все описания и информацию о ценах), а также URI подписанного URL-адреса (Shared Access Signature, SAS) для передачи любых значков надстроек для отправки в хранилище BLOB-объектов Azure.

    > [!NOTE]
    > URI SAS предоставляет доступ к защищенному ресурсу в хранилище Azure без необходимости в использовании ключей учетной записи. Основные сведения об URI SAS и их использовании с хранилищем BLOB-объектов Azure см. в разделах [Подписанные URL-адреса, часть 1. Общие сведения о модели SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) и [Подписанные URL-адреса, часть 2. Создание и использование SAS с хранилищем BLOB-объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. При добавлении новых значков для отправки [подготовьте значки](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions) и добавьте их в ZIP-архив.

5. Обновите данные [отправки надстройки](#add-on-submission-object), внеся все необходимые для новой отправки изменения, затем выполните следующий метод для обновления отправки. Дополнительные сведения см. в разделе [Обновление отправки надстройки](update-an-add-on-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Если в отправку добавляются новые значки, обязательно обновите данные отправки, чтобы они ссылались на имя и относительный путь этих файлов в ZIP-архиве.

4. При добавлении новых значков для отправки выложите ZIP-архив в [хранилище BLOB-объектов Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage), используя URI SAS, полученный в теле ответа метода POST, вызов которого был выполнен ранее. Существуют разные библиотеки Azure, которые можно использовать в этих целях на различных платформах, включая следующие.

    * [Клиентская библиотека службы хранилища Azure для .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Пакет SDK службы хранилища Azure для Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Пакет SDK службы хранилища Azure для Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    В следующем фрагменте кода на C# показано, как передать ZIP-архив в хранилище BLOB-объектов Azure с помощью класса [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) клиентской библиотеки службы хранилища Azure для .NET. В этом примере кода предполагается, что ZIP-архив уже был записан в потоковый объект.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Подтвердите отправку, выполнив следующий метод. В результате вы получите оповещение об этом центре партнеров, и теперь ваши обновления должны быть применены к вашей учетной записи. Дополнительные сведения см. в разделе [Подтверждение отправки надстройки](commit-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Проверьте состояние подтверждения, выполнив следующий метод. Дополнительные сведения см. в разделе [Получение состоянии отправки надстройки](get-status-for-an-add-on-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Чтобы проверить состояние отправки, посмотрите значение поля *status* в теле ответа. Это значение должно измениться с **CommitStarted** на **PreProcessing**, если запрос выполнен успешно, или на **CommitFailed**, если в запросе возникли ошибки. Если имеются ошибки, поле *statusDetails* содержит дополнительные сведения об ошибке.

7. После успешного завершения подтверждения отправки она отправляется в Магазин для внедрения. Вы можете продолжить отслеживать ход отправки, используя предыдущий метод или посетив центр партнеров.

<span/>

## <a name="code-examples"></a>Примеры кода

Следующие статьи содержат подробные примеры кода на нескольких разных языках программирования, демонстрирующие процесс создания отправки надстройки.

* [C#примеры кода](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Примеры кода Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Примеры кода Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Модуль StoreBroker PowerShell

В качестве альтернативы прямому вызову API отправки в Microsoft Store также предоставляется модуль PowerShell с открытым исходным кодом, в котором поверх API реализован интерфейс командной строки. Этот модуль называется [StoreBroker](https://github.com/Microsoft/StoreBroker). Этот модуль можно использовать для управления приложением, тестируемой возможностью и отправками надстроек из командной строки, вместо того чтобы вызывать API отправки в Microsoft Store напрямую. Кроме того, можно просмотреть дополнительные примеры вызова этого API, изучив исходный код. Модуль StoreBroker активно используется Microsoft в качестве основного способа отправки многих собственных приложений компании в Магазин.

Дополнительные сведения см. на нашей [странице StoreBroker на сайте GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Ресурсы данных

Для управления отправками надстроек методы API отправки в Microsoft Store используют следующие ресурсы данных JSON.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>Ресурс отправки надстройки

Этот ресурс описывает отправку надстройки.

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

У этого ресурса есть следующие значения.

| Значение      | Тип   | Описание        |
|------------|--------|----------------------|
| id            | Строка  | Идентификатор отправки. Этот идентификатор доступен в данных ответа на запросы на [создание отправки надстройки](create-an-add-on-submission.md), [получение всех надстроек](get-all-add-ons.md) и [получение определенной надстройки](get-an-add-on.md). Для отправки, созданной в центре партнеров, этот идентификатор также доступен в URL-адресе страницы отправки в центре партнеров.  |
| contentType           | Строка  |  [Тип содержимого](../publish/enter-add-on-properties.md#content-type), которое предоставляется в надстройке. Может принимать одно из следующих значений. <ul><li>NotSet (Не задано)</li><li>BookDownload (Загрузка книги)</li><li>EMagazine (Электронный журнал)</li><li>ENewspaper (Электронная газета)</li><li>MusicDownload (Загрузка музыки)</li><li>MusicStream (Потоковая передача музыки)</li><li>OnlineDataStorage (Сетевое хранилище данных)</li><li>VideoDownload (Загрузка видео)</li><li>VideoStream (Потоковая передача видео)</li><li>Asp</li><li>OnlineDownload (Загрузка по Интернету)</li></ul> |  
| keywords           | Массив  | Массив строк, содержащих до 10 [ключевых слов](../publish/enter-add-on-properties.md#keywords) для надстройки. Приложение может запрашивать надстройки с помощью этих ключевых слов.   |
| lifetime           | Строка  |  Время существования надстройки. Может принимать одно из следующих значений. <ul><li>Forever (Навсегда)</li><li>OneDay (Один день)</li><li>ThreeDays (3 дня)</li><li>FiveDays (5 дней)</li><li>OneWeek (Одна неделя)</li><li>TwoWeeks (Две недели)</li><li>OneMonth (Один месяц)</li><li>TwoMonths (Два месяца)</li><li>ThreeMonths (Три месяца)</li><li>SixMonths (Шесть месяцев)</li><li>OneYear (Один год)</li></ul> |
| listings           | object  |  Словарь пар "ключ-значение", где каждый ключ представляет собой код страны ISO 3166-1 alpha-2 из двух букв, а каждое значение — объект [ресурса описания](#listing-object), содержащий информацию для описании надстройки.  |
| pricing           | object  | [Ресурс цены](#pricing-object), содержащий сведения о цене надстройки.   |
| targetPublishMode           | Строка  | Режим публикации для отправки. Может принимать одно из следующих значений. <ul><li>Immediate (Незамедлительно)</li><li>Manual (Вручную)</li><li>SpecificDate (Указанная дата)</li></ul> |
| targetPublishDate           | Строка  | Дата публикации отправки в формате ISO 8601, если для *targetPublishMode* задано значение SpecificDate.  |
| tag           | Строка  |  [Настраиваемые данные разработчика](../publish/enter-add-on-properties.md#custom-developer-data) для надстройки (ранее эта информация называлась *tag*).   |
| visibility  | Строка  |  Видимость надстройки. Может принимать одно из следующих значений. <ul><li>Hidden (Скрыто)</li><li>Public (Общее)</li><li>Private (Частное)</li><li>NotSet (Не задано)</li></ul>  |
| status  | Строка  |  Состояние отправки. Может принимать одно из следующих значений. <ul><li>None</li><li>Canceled (Отменено)</li><li>PendingCommit (Ожидание фиксации)</li><li>CommitStarted (Фиксация запущена)</li><li>CommitFailed (Сбой фиксации)</li><li>PendingPublication (Ожидание публикации)</li><li>Publishing (Выполняется публикация)</li><li>Published (Опубликовано)</li><li>PublishFailed (Сбой публикации)</li><li>PreProcessing (Предварительная обработка)</li><li>PreProcessingFailed (Сбой предварительной обработки)</li><li>Certification (Сертификация)</li><li>CertificationFailed (Сбой сертификации)</li><li>Выпуск</li><li>ReleaseFailed (Сбой выпуска)</li></ul>   |
| statusDetails           | object  |  [Ресурс сведений о состоянии](#status-details-object), который содержит дополнительные сведения о состоянии отправки, включая сведения об ошибках. |
| fileUploadUrl           | Строка  | URI подписанного URL-адреса (SAS) для передачи пакетов для отправки. При добавлении новых пакетов для отправки выложите ZIP-архив, содержащий пакеты, по этому URI. Дополнительные сведения см. в разделе [Создание отправки надстройки](#create-an-add-on-submission).  |
| friendlyName  | Строка  |  Понятное имя отправки, как показано в центре партнеров. Это значение создается при создании отправки.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Ресурс описания

Этот ресурс содержит [информацию описания для надстройки](../publish/create-add-on-store-listings.md). У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание       |
|-----------------|---------|------|
|  Описание               |    Строка     |   Описание для описания надстройки.   |     
|  Значок               |   object      |[Ресурс значка](#icon-object), содержащий данные для значка описания надстройки.    |
|  title               |     Строка    |   Заголовок для описания надстройки.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Ресурс значка

Этот ресурс содержит данные значка для описания надстройки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание     |
|-----------------|---------|------|
|  fileName               |    Строка     |   Имя файла значка в ZIP-архиве, который был передан для отправки. Значок должен представлять собой PNG-файл размером 300 x 300 пикселей.   |     
|  fileStatus               |   Строка      |  Состояние файла значка. Может принимать одно из следующих значений. <ul><li>None</li><li>PendingUpload (Ожидает передачи)</li><li>Uploaded (Передан)</li><li>PendingDelete (Ожидает удаления)</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Ресурс цены

Этот ресурс содержит сведения о ценах для надстройки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание    |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Словарь пар "ключ-значение", где каждый ключ представляет собой код страны ISO 3166-1 alpha-2 из двух букв, а каждое значение — [ценовую категорию](#price-tiers). Эти элементы представляют [особые цены на вашу надстройку для определенных рынков](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability). Любые элементы этого словаря переопределяют базовую цену, заданную значением *priceId* для указанного рынка.     |     
|  sales               |   Массив      |  **Не рекомендуется**. Массив [ресурсов продажи](#sale-object), содержащих сведения о продажах для надстройки.     |     
|  priceId               |   Строка      |  [Ценовая категория](#price-tiers), указывающая [базовую цену](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability) для надстройки.    |    
|  isAdvancedPricingModel               |   Логический      |  Если **true**, ваша учетная запись разработчика имеет доступ к расширенному набору ценовых категорий от 0,99 долл. США до 1999,99 долл. США. Если **false**, ваша учетная запись разработчика имеет доступ к стандартному набору ценовых категорий от 0,99 долл. США до 999,99 долл. США. Дополнительные сведения о разных категориях см. в разделе [Ценовые категории](#price-tiers).<br/><br/>**Примечание**.&nbsp;&nbsp;Данное поле предназначено только для чтения.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Ресурс продажи

Этот ресурс содержит сведения о продаже для надстройки.

> [!IMPORTANT]
> Ресурс **продажи** больше не поддерживается, и в настоящее время невозможно получить или изменить данные продажи для отправки надстройки с помощью API отправки в Microsoft Store. В дальнейшем мы обновим API отправки в Microsoft Store для внедрения нового способа программного доступа к информации о продажах для отправок надстроек.
>    * После вызова [метода GET для получения отправки надстройки](get-an-add-on-submission.md) значение *sales* будет пустым. Вы можете продолжить использовать центр партнеров для получения данных о продажах для отправки надстройки.
>    * При вызове [метода PUT для обновления отправки надстройки](update-an-add-on-submission.md) сведения в значении *sales* будут игнорироваться. Вы можете продолжить использовать центр партнеров для изменения данных о продажах для отправки надстройки.

У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание           |
|-----------------|---------|------|
|  name               |    Строка     |   Имя продажи.    |     
|  basePriceId               |   Строка      |  [Ценовая категория](#price-tiers), используемая для базовой цены продажи.    |     
|  startDate               |   Строка      |   Дата начала для продажи в формате ISO 8601.  |     
|  endDate               |   Строка      |  Дата окончания для продажи в формате ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Словарь пар "ключ-значение", где каждый ключ представляет собой код страны ISO 3166-1 alpha-2 из двух букв, а каждое значение — [ценовую категорию](#price-tiers). Эти элементы представляют [особые цены на вашу надстройку для определенных рынков](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability). Любые элементы этого словаря переопределяют базовую цену, заданную значением *basePriceId* для указанного рынка.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ресурс сведений о состоянии

Этот ресурс содержит дополнительные сведения о состоянии отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание       |
|-----------------|---------|------|
|  errors               |    object     |   Массив [ресурсов сведений о состоянии](#status-detail-object), который содержит информацию об ошибках для отправки.   |     
|  warnings               |   object      | Массив [ресурсов сведений о состоянии](#status-detail-object), который содержит информацию о предупреждениях для отправки.     |
|  certificationReports               |     object    |   Массив [ресурсов отчета сертификации](#certification-report-object), который предоставляет доступ к данным отчета о сертификации для отправки. В случае сбоя сертификации можно проверить эти отчеты для получения дополнительной информации.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ресурс сведений о состоянии

Этот ресурс содержит дополнительные сведения обо всех связанных ошибках или предупреждениях для отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание    |
|-----------------|---------|------|
|  code               |    Строка     |   [Код состояния отправки](#submission-status-code), описывающий тип ошибки или предупреждения.   |     
|  details               |     Строка    |  Сообщение с дополнительными сведениями о проблеме.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ресурс отчета о сертификации

Этот ресурс предоставляет доступ к данным отчета о сертификации для отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание               |
|-----------------|---------|------|
|     date            |    Строка     |  Дата и время создания отчета в формате ISO 8601.    |
|     reportUrl            |    Строка     |  URL-адрес, по которому можно получить доступ к отчету.    |

## <a name="enums"></a>перечислениям;

Эти методы используют следующие перечисления.

<span id="price-tiers" />

### <a name="price-tiers"></a>Ценовые категории

Следующие значения представляют доступные ценовые категории в ресурсе [ценовой ресурс](#pricing-object) для отправки надстройки.

| Значение           | Описание       |
|-----------------|------|
|  Base               |   Ценовая категория не задана; используется базовая цена для надстройки.      |     
|  NotAvailable              |   Надстройка недоступна в указанном регионе.    |     
|  Free              |   Настройка бесплатна.    |    
|  Tier*xxxx*               |   Строка, указывающая ценовую категорию надстройки в формате **Tier<em>xxxx</em>** . В настоящее время поддерживаются следующие ценовые категории.<br/><br/><ul><li>Если значение *isAdvancedPricingModel*[ценового ресурса](#pricing-object) — **true**, для вашей учетной записи доступны такие ценовые категории: **Tier1012** - **Tier1424**.</li><li>Если значение *isAdvancedPricingModel*[ценового ресурса](#pricing-object) — **false**, для вашей учетной записи доступны такие ценовые категории: **Tier2** - **Tier96**.</li></ul>Чтобы просмотреть полную таблицу ценовых уровней, доступных для вашей учетной записи разработчика, включая цены, связанные с каждым из уровней рынка, перейдите на страницу **цены и доступности** для всех отправок приложения в центре партнеров и щелкните ссылку **Просмотреть таблицу** в разделе " **рынки и настраиваемые цены** " (для некоторых учетных записей разработчиков эта ссылка находится в разделе " **цены** ").     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Код состояния отправки

Следующие значения представляют код состояния отправки.

| Значение           |  Описание      |
|-----------------|---------------|
|  None            |     Код не указан.         |     
|      InvalidArchive        |     ZIP-архив, содержащий пакет, является недопустимым или имеет неизвестный формат архива.  |
| MissingFiles | В ZIP-архиве отсутствуют файлы, перечисленные в данных отправки, или они находятся в неправильном месте в архиве. |
| PackageValidationFailed | Один или несколько пакетов в вашей отправке не прошли проверку. |
| InvalidParameterValue | Один из параметров в теле запроса не является допустимым. |
| InvalidOperation | Операция, которую вы пытались выполнить, является недопустимой. |
| InvalidState | Операция, которую вы пытались выполнить, является недопустимой для текущего состояния тестового пакета. |
| ResourceNotFound | Не удалось найти указанный тестовый пакет. |
| ServiceError | Запрос не был успешно выполнен из-за внутренней ошибки службы. Попробуйте повторить запрос. |
| ListingOptOutWarning | Разработчик удалил описание из предыдущей отправки или не включил данные описания, которые поддерживаются пакетом. |
| ListingOptInWarning  | Разработчик добавил описание. |
| UpdateOnlyWarning | Разработчик пытается вставить какой-то элемент, который имеет только поддержку обновления. |
| Другое  | Отправка находится в неизвестном состоянии или состоянии, не отнесенном ни к одной из категорий. |
| PackageValidationWarning | В процессе проверки пакета создано предупреждение. |

<span/>

## <a name="related-topics"></a>См. также

* [Создание отправок и управление ими с помощью служб Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Управление дополнительными компонентами с помощью API отправки Microsoft Store](manage-add-ons.md)
* [Отправку надстроек в центре партнеров](https://docs.microsoft.com/windows/uwp/publish/iap-submissions)
