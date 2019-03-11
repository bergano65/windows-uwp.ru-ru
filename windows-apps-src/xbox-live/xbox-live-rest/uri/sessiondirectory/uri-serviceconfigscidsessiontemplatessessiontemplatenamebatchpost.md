---
title: POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
assetID: 1a0a62ca-e120-e705-3c93-efd4697e2ccf
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.html
description: " POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, игры, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 833b3c6b532dd65856342678d646798c5f24a6c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637189"
---
# <a name="post-serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
Создает пакетный запрос на несколько идентификаторов пользователей Xbox.

> [!IMPORTANT]
> Этот метод используется многопользовательскую 2015 и применяется только к этой многопользовательские версии и более поздних версий. Он предназначен для использования с контрактом шаблона 104/105 или более поздней версии и требуется элемент заголовка X-Xbl-контракт-версии: 104/105 или более поздней версии, при каждом запросе.

  * ["Примечания"](#ID4ET)
  * [Параметры URI](#ID4EKB)
  * [Параметры строки запроса](#ID4EVB)
  * [Коды состояния HTTP](#ID4EGF)
  * [Текст запроса](#ID4ENF)
  * [Текст ответа](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Замечания

Этот метод HTTP/REST создает пакетные запросы на несколько идентификаторов на уровне шаблона сеанса пользователей Xbox. Этот метод может быть заключена в **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**.

Многопользовательскую 2015, вы можете сочетать нескольких запросов в один вызов, если все запросы — прежним, но работа с разных идентификаторов пользователей Xbox (XUIDs), представленного в *xuid* параметр строки запроса. Обратите внимание на то, что параметры строки запроса и ответов, одинаковы для обычных запросов и пакетные запросы.

Для пакетного запроса, сеансы, принадлежащие к каждой XUID записываются в ответ, в том же порядке, что *xuid* параметры перечислены в запросе. Возможно, для одного сеанса встречается несколько раз в ответе, один раз для каждого *xuid* , он соответствует.

<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>Параметры универсального кода ресурса (URI)

| Параметр| Тип| Описание|
| --- | --- | --- | --- |
| scid| Код GUID| Идентификатор конфигурации службы (SCID). Часть 1 из идентификатор сеанса.|
| sessionTemplateName| Строка| Имя текущего экземпляра шаблона сеанса. Часть 2 из идентификатор сеанса.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>Параметры строки запроса

Запрос можно изменить с помощью параметров строки запроса в следующей таблице.

| <b>Параметр</b>| <b>Тип</b>| <b>Описание</b>|
| --- | --- | --- | --- | --- | --- | --- |
| Ключевое слово| Строка| A ключевое слово, например, «foo», который должен находиться в сеансов или шаблоны, если они должны быть получены. |
| xuid| 64-разрядного целого числа без знака| Xbox идентификатор пользователя, например, «123», для сеансов включить в запрос. По умолчанию пользователь должен быть активной в сеансе для него необходимо включить. |
| резервирования| Логический| Значение true для включения сеансов, для которого пользователь задается как зарезервированные проигрывателя, но не присоединен к становятся проигрыватель с active. Этот параметр используется только в том случае, при запросе собственный сеанс, или при запросе определенного пользователя сеансов от сервера к серверу. |
| Неактивные| Логический| Значение true для включения сеансов, которые пользователь принял, но не воспроизводится активно. Сеансы, в которых пользователь является «Готово», но не «активен» учитываются как неактивные. |
| закрытый| Логический| Значение true для включения частных сеансов. Этот параметр используется только в том случае, при запросе собственный сеанс, или при запросе определенного пользователя сеансов от сервера к серверу. |
| visibility| Строка| Состояние видимости для сеансов. Возможные значения определяются <b>MultiplayerSessionVisibility</b>. Если этот параметр имеет значение «открыть», запрос должен включать только открытые сеансы. Если задано значение «частный», <i>частного</i> параметра должно быть установлено в значение true. |
| version| 32-разрядное знаковое целое число| Версия максимальное число сеансов, который должен быть включен. Например значение 2 указывает, что только сеансы с версией 2 основных сеанса или меньше должны быть включены. Номер версии должен быть меньше или равным mod версии контракта запроса 100. |
| Take| 32-разрядное знаковое целое число| Максимальное количество сеансов для извлечения. Это число должно быть в диапазоне от 0 до 100. |


При настройке любого *частного* или *резервирования* true требуется доступ на уровне сервера к сеансу. Кроме того эти параметры требуют утверждения XUID вызывающей стороны на соответствие фильтру XUID в URI. В противном случае — код состояния HTTP 403 — возвращается, независимо от того, имеется ли все такие сеансы действительно существуют.

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>Коды состояния HTTP
Служба возвращает код состояния HTTP, применительно к MPSD.  
<a id="ID4ENF"></a>


## <a name="request-body"></a>Тело запроса


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>Тело ответа

Из этого метода возвращается массив JSON ссылки на сеанс, со встроенным данных включены некоторые сеанса.


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}
```


<a id="ID4EDG"></a>


## <a name="see-also"></a>См. также

<a id="ID4EFG"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)