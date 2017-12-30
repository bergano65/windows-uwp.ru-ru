---
author: mcleanbyron
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: "Используйте этот метод в API рекламных акций Магазина Windows для управления рекламными материалами в кампаниях."
title: "Управление рекламными материалами"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, API рекламных акций Магазина Windows, рекламные кампании"
ms.openlocfilehash: d94ff7863de620beab2ef67c4a6e5c4a50cf273d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-creatives"></a>Управление рекламными материалами

Используйте эти методы в API рекламных акций Магазина Windows, чтобы выкладывать собственную рекламу и использовать ее в кампаниях, либо получать существующие рекламные материалы. Реклама может быть связана с одной или несколькими линиями поставки даже в разных рекламных кампаниях при условии, что она всегда представляет одно и то же приложение.

Дополнительные сведения о связи между рекламными материалами, кампаниями, строками поставки и целевыми профилями см. в разделе [Проведение рекламных кампаний с помощью служб Магазина Windows](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Примечание.**&nbsp;&nbsp;Если вы используете этот API для добавления собственных рекламных материалов, их размер не должен превышать 40КБ. При отправке файла большего размера API не станет возвращать ошибку, однако кампании создана не будет.

## <a name="prerequisites"></a>Необходимые условия

Для использования этих методов сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](run-ad-campaigns-using-windows-store-services.md#prerequisites) для API рекламных акций Магазина Windows.
* [Получите маркер доступа Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этих методов. После получения маркера доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия маркера можно получить новый маркер.

## <a name="request"></a>Запрос

Эти методы имеют следующие URI.

| Тип метода | Универсальный код ресурса (URI) запроса     |  Описание  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Создает новую рекламу.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Получает рекламу, заданную *creativeId*.  |

>**Примечание**.&nbsp;&nbsp;Этот API в настоящее время не поддерживает метод PUT.

<span/> 
### <a name="header"></a>Заголовок

| Заголовок        | Тип   | Описание         |
|---------------|--------|---------------------|
| Authorization | Строка | Обязательное. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |
| Tracking ID   | Код GUID   | Необязательный параметр. Идентификатор, который отслеживает поток вызовов.                                  |


<span/>
### <a name="request-body"></a>Текст запроса

Метод POST требует тело запроса JSON с обязательными полями объекта [Creative](#creative).

<span/>
### <a name="request-examples"></a>Примеры запросов

Следующий пример демонстрирует вызов метода POST для создания рекламного материала. В этом примере значение *content* сокращено для краткости.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

Следующий пример демонстрирует вызов метода GET для получения рекламного материала.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>
## <a name="response"></a>Ответ

Эти методы возвращают тело ответа JSON с объектом [Creative](#creative), содержащее сведения о созданном или полученном рекламном материале. В следующем примере показано тело ответа для этих методов. В этом примере значение *content* сокращено для краткости.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```

<span id="creative"/>
## <a name="creative-object"></a>Объект Creative

Для этих методов тела запроса и ответа содержат следующие поля. В этой таблице показаны поля, которые доступны только для чтения (это означает, что они не могут изменяться в методе PUT), и поля, которые необходимы в теле запроса для метода POST.

| Поле        | Тип   |  Описание      |  Только чтение  | По умолчанию  |  Обязательный для POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  целое число   |  Идентификатор рекламного материала.     |   Да    |      |    Нет   |       
|  name   |  строка   |   Имя рекламного материала.    |    Нет   |      |  Да     |       
|  content   |  строка   |  Содержимое рекламного изображения в формате Base64.<br/><br/>**Примечание.**&nbsp;&nbsp;Максимально допустимый размер рекламных материалов— 40КБ. При отправке файла большего размера API не станет возвращать ошибку, однако кампании создана не будет.     |  Нет     |      |   Да    |       
|  height   |  integer   |   Высота рекламного материала.    |    Нет    |      |   Да    |       
|  width   |  integer   |  Ширина рекламного материала.     |  Нет    |     |    Да   |       
|  landingUrl   |  string   |  Если вы используете службы отслеживания кампаний, такие как Kochava, AppsFlyer или Tune, для анализа установок вашего приложения, укажите в этом поле URL-адрес отслеживания при вызове метода POST (это значение должно иметь допустимый URI). Если вы не используете службы отслеживания кампании, опустите это значение при вызове метода POST (в данном случае этот URL-адрес будет создан автоматически).   |  Нет    |     |   Да    |       
|  format   |  string   |   Формат рекламы. На данный момент единственным поддерживаемым значением является **Banner**.    |   Нет    |  Banner   |  Нет     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   Предоставляет атрибуты для рекламного материала.     |   Нет    |      |   Да    |       
|  storeProductId   |  строка   |   [Код продукта в Магазине](in-app-purchases-and-trials.md#store-ids) для приложения, с которым связана эта рекламная кампания. Пример кода продукта в Магазине — 9nblggh42cfd.    |   Нет    |    |  Нет     |   |  

<span id="image-attributes"/>
## <a name="imageattributes-object"></a>Объект ImageAttributes

| Поле        | Тип   |  Описание      |  Только чтение  | Значение по умолчанию  | Обязательный для POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   string  |   Принимает одно из следующих значений: **PNG** или **JPG**.    |    Нет   |      |   Да    |       |


## <a name="related-topics"></a>Связанные разделы

* [Проведение рекламных кампаний с помощью служб Магазина Windows](run-ad-campaigns-using-windows-store-services.md)
* [Управление рекламными кампаниями](manage-ad-campaigns.md)
* [Управление линиями поставки для рекламных кампаний](manage-delivery-lines-for-ad-campaigns.md)
* [Управление профилями таргетинга рекламных кампаний](manage-targeting-profiles-for-ad-campaigns.md)
* [Получение данных об эффективности рекламной кампании](get-ad-campaign-performance-data.md)