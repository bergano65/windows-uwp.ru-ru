---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Используйте API Microsoft Store Analytics для программного получения данных аналитики для приложений, зарегистрированных в вашей учетной записи центра партнеров Windows или вашей организации.
title: Доступ к аналитическим данным с помощью служб Магазина
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, службы Store, API аналитики для Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3b732da8f92c258647f905e6939dc3cb1b9c9f87
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853361"
---
# <a name="access-analytics-data-using-store-services"></a>Доступ к аналитическим данным с помощью служб Магазина

Используйте *API Microsoft Store Analytics* для программного получения данных аналитики для приложений, зарегистрированных в вашей учетной записи центра партнеров Windows или вашей организации. Этот API позволяет извлекать данные о приобретении, ошибках, оценках и отзывов для приложения и надстройки (внутреннего продукта или IAP). Для проверки подлинности вызовов из приложения или службы в этом интерфейсе используется служба Azure Active Directory (Azure AD).

Далее описан весь процесс.

1.  Убедитесь, что вы выполнили все [необходимые условия](#prerequisites).
2.  Перед вызовом метода в API аналитики для Microsoft Store[получите маркер доступа Azure AD](#obtain-an-azure-ad-access-token). После получения маркера доступа у вас будет 60 минут, чтобы использовать его в вызовах к API аналитики для Microsoft Store до окончания срока действия маркера. После истечения срока действия маркера можно сформировать новый маркер.
3.  [Вызов API аналитики для Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Шаг 1. Выполнение необходимых условий для использования API аналитики для Microsoft Store

Перед тем как начать писать код для вызова API аналитики для Microsoft Store, убедитесь, что вы выполнили следующие необходимые условия.

* У вас (или у вашей организации) должен быть каталог Azure AD, а также разрешение [глобального администратора](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) для этого каталога. Если вы уже используете Office 365 или другие бизнес-службы Майкрософт, то у вас уже есть Azure Active Directory. В противном случае вы можете [создать новую службу Azure AD в центре партнеров](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) без дополнительной платы.

* Необходимо связать приложение Azure AD с учетной записью центра партнеров, получить идентификатор клиента и идентификатор клиента для приложения и создать ключ. Приложение Azure AD представляет собой приложение или службу, из которой отправляются вызовы в API аналитики для Microsoft Store. Чтобы получить маркер доступа Azure AD, который вы передадите в API, необходимо иметь в наличии идентификатор владельца, идентификатор клиента и ключ.
    > [!NOTE]
    > Эту операцию необходимо выполнить только один раз. После того как вы получите идентификатор владельца, идентификатор клиента и ключ, их можно будет использовать повторно в любое время для создания нового маркера доступа Azure AD.

Чтобы связать приложение Azure AD с учетной записью центра партнеров и получить необходимые значения:

1.  В центре партнеров [свяжите учетную запись центра партнеров Организации с каталогом Azure AD вашей организации](../publish/associate-azure-ad-with-partner-center.md).

2.  Затем на странице **Пользователи** в разделе **Параметры учетной записи** центра партнеров [добавьте приложение Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , которое представляет приложение или службу, которые будут использоваться для доступа к данным аналитики для учетной записи центра партнеров. Обязательно назначьте этому приложению роль **Менеджер**. Если приложение еще не существует в каталоге Azure AD, вы можете [создать новое приложение Azure AD в центре партнеров](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Вернитесь на страницу **Пользователи**, щелкните имя приложения Azure AD, чтобы перейти к параметрам приложения, и скопируйте значения **идентификатора владельца** и **идентификатора клиента**.

4. Нажмите кнопку **Добавить новый ключ**. На следующем экране скопируйте **ключ**. Вы не сможете получить доступ к этой информации после закрытия страницы. Дополнительные сведения см. в разделе [Управление ключами для приложения Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Шаг 2. Получение маркера доступа Azure AD

Перед тем как можно будет вызвать любой из методов в API аналитики для Microsoft Store, сначала необходимо получить маркер доступа Azure AD и передать его в заголовок **Авторизация** каждого метода в API. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия маркера вы можете обновить его, чтобы дальше использовать в последующих вызовах к API.

Для получения маркера доступа следуйте инструкциям в разделе [Вызовы между службами с помощью учетных данных клиентов](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/), чтобы отправить HTTP-запрос POST в конечную точку ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Ниже приведен пример запроса.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Для значения *идентификатора\_клиента* в URI POST и параметров *Client\_id* и *Client\_Secret* укажите идентификатор клиента, идентификатор клиента и ключ для приложения, полученные из центра партнеров в предыдущем разделе. Для параметра *resource* укажите ```https://manage.devcenter.microsoft.com```.

После истечения срока действия маркера доступа вы можете обновить его, следуя инструкциям, приведенным [здесь](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Шаг 3. Вызов API аналитики для Microsoft Store

После получения маркера доступа Azure AD вы можете вызвать API аналитики для Microsoft Store. Необходимо передать токен доступа в заголовок **Authorization** каждого метода.

### <a name="methods-for-uwp-apps-and-games"></a>Методы для приложений и игр UWP
Следующие методы доступны для приобретения приложений и игр и дополнительных приобретений. 

* [Получение данных о приобретении для игр и приложений](acquisitions-data.md)
* [Получение данных о приобретении надстроек для игр и приложений](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Методы для приложений UWP 

Следующие методы аналитики доступны для приложений UWP в центре партнеров.

| Сценарий       | Методы      |
|---------------|--------------------|
| Получение, преобразование, установка и использование |  <ul><li>[Получение приобретений приложений](get-app-acquisitions.md) (прежние версии)</li><li>[Получение данных воронки для получения приложения](get-acquisition-funnel-data.md) (прежние версии)</li><li>[Получение преобразований приложений по каналу](get-app-conversions-by-channel.md)</li><li>[Получение дополнительных приобретений](get-in-app-acquisitions.md)</li><li>[Получение дополнительных приобретений подписки](get-subscription-acquisitions.md)</li><li>[Получение преобразований надстроек по каналу](get-add-on-conversions-by-channel.md)</li><li>[Получить установки приложений](get-app-installs.md)</li><li>[Получение ежедневного использования приложения](get-app-usage-daily.md)</li><li>[Получение ежемесячного использования приложений](get-app-usage-monthly.md)</li></ul> |
| Ошибки приложения | <ul><li>[Получение данных отчетов об ошибках](get-error-reporting-data.md)</li><li>[Получение сведений об ошибке в приложении](get-details-for-an-error-in-your-app.md)</li><li>[Получение трассировки стека для ошибки в приложении](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Скачивание CAB-файла для ошибки в приложении](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Аналитика | <ul><li>[Получение данных аналитики для приложения](get-insights-data-for-your-app.md)</li></ul>  |
| Оценки и отзывы | <ul><li>[Получить рейтинги приложений](get-app-ratings.md)</li><li>[Получить обзоры приложений](get-app-reviews.md)</li></ul> |
| Реклама в приложении и рекламные кампании | <ul><li>[Получение данных о производительности AD](get-ad-performance-data.md)</li><li>[Получение данных о производительности кампании ad](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Методы для классических приложений

Для учетных записей разработчиков, которые связаны с [программой для разработчиков классических приложений для Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program), доступны следующие методы аналитики.

| Сценарий       | Методы      |
|---------------|--------------------|
| Установка... |  <ul><li>[Получение установок для настольных приложений](get-desktop-app-installs.md)</li></ul> |
| Blocks |  <ul><li>[Получение блоков обновления для классического приложения](get-desktop-block-data.md)</li><li>[Получение сведений о блоке обновления для классического приложения](get-desktop-block-data-details.md)</li></ul> |
| Ошибки приложений. |  <ul><li>[Получение данных отчетов об ошибках для приложения для настольных систем](get-desktop-application-error-reporting-data.md)</li><li>[Получение сведений об ошибке в приложении для настольных систем](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Получение трассировки стека для ошибки в классическом приложении](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Скачайте CAB-файл для ошибки в классическом приложении](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Аналитика | <ul><li>[Получение данных аналитики для настольного приложения](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Методы для служб Xbox Live

Доступны следующие дополнительные методы для использования учетными записями разработчиков в играх, использующих [службы Xbox Live](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md).

| Сценарий       | Методы      |
|---------------|--------------------|
| Общая аналитика |  <ul><li>[Получение данных Xbox Live Analytics](get-xbox-live-analytics.md)</li><li>[Получение данных о достижениях Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Получение данных о параллельном использовании Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Аналитика работоспособности |  <ul><li>[Получение данных о работоспособности Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Аналитика сообщества |  <ul><li>[Получение данных из игрового центра Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Получение данных клуба Xbox Live](get-xbox-live-club-data.md)</li><li>[Получение данных многопользовательского режима Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>Методы для оборудования и драйверов

Учетные записи разработчиков, принадлежащие к [программе панели мониторинга оборудования Windows](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) , имеют доступ к дополнительному набору методов для получения данных аналитики для оборудования и драйверов. Дополнительные сведения см. в разделе [API аппаратной панели мониторинга](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Пример кода

В следующем примере кода показано, как получить маркер доступа Azure AD и вызвать API аналитики для Microsoft Store из консольного приложения C#. Чтобы использовать этот пример кода, назначьте переменным *tenantId*, *clientId*, *clientSecret* и *appID* соответствующие вашему сценарию значения. В этом примере для десериализации данных JSON, возвращаемых API аналитики для Microsoft Store, требуется [пакет Json.NET](https://www.newtonsoft.com/json).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Ошибочные ответы

API аналитики для Microsoft Store возвращает ошибочные ответы в объекте JSON, который содержит коды ошибок и сообщения. В следующем примере демонстрируется ошибочный ответ, вызванный недопустимым параметром.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```
