---
author: mathy
title: Портал устройств— справочник по API для мониторинга HTTP
description: Узнайте, как осуществлять доступ к HTTP-трафику из приложения в фокусе на Xbox.
ms.localizationpriority: medium
ms.openlocfilehash: e7ce2ba75eb24f3963818935a7172b25114fd144
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2017
ms.locfileid: "1397297"
---
# <a name="http-monitor-api-reference"></a>Справочник по API для мониторинга HTTP   
Трафик HTTP в режиме реального времени доступен для приложения в фокусе через этот API, если на консоли Xbox был включен монитор HTTP (для этого нужно установить соответствующий флажок в Dev Home).

## <a name="get-if-the-http-monitor-is-enabled"></a>Как узнать, что монитор HTTP включен

**Запрос**

Узнать, включен ли монитор HTTP, можно в Dev Home.

Способ      | URI запроса
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**Параметры универсального кода ресурса (URI)**

- Нет

**Заголовки запроса**

- Нет

**Тело запроса**

- Нет

**Ответ**   
Объект JSON с перечисленными ниже полями.

* Включен: (логическое значение) показывает, включен ли монитор HTTP на консоли Xbox путем установки флажка в Dev Home.

**Код состояния**

Этот API имеет следующие предполагаемые коды состояния.

Код состояния HTTP      | Описание
:------     | :-----
200 | Запрос выполнен успешно
4XX | Коды ошибок
5XX | Коды ошибок

## <a name="get-http-traffic-from-the-focused-app"></a>Получение трафика HTTP от приложения в фокусе
**Запрос**

Вы можете получать трафик HTTP от приложения в фокусе на Xbox (кроме системных приложений) в режиме реального времени, если монитор HTTP включен в Dev Home.

Способ      | URI запроса
:------     | :-----
WebSocket | /ext/httpmonitor/sessions
<br />
**Параметры универсального кода ресурса (URI)**

- Нет

**Заголовки запроса**

- Нет

**Тело запроса**

- Нет

**Ответ**   
Объект JSON с перечисленными ниже полями.

* Сессии
    * RequestHeaders: (объект JSON) заголовки запроса из запроса HTTP.
    * RequestContentHeaders: (объект JSON) заголовки содержимого запроса из запроса HTTP.
    * RequestURL: (строка) URL-адрес запроса.
    * RequestMethod: (строка) метод запроса.
    * RequestMessage: (строка) сообщение запроса, которое в настоящее время поддерживает только JSON и текстовое содержимое.
    * ResponseHeaders: (объект JSON) заголовки запроса из запроса HTTP.
    * ResponseContentHeaders: (объект JSON) заголовки содержимого запроса из запроса HTTP.
    * StatusCode: (число) код состояния отклика.
    * ReasponsePhrase: (строка) фраза, обозначающая причину отклика.
    * ResponseMessage: (строка) сообщение отклика, которое в настоящее время поддерживает только JSON и текстовое содержимое.

**Код состояния**

Этот API имеет следующие предполагаемые коды состояния.

Код состояния HTTP      | Описание
:------     | :-----
200 | Запрос выполнен успешно
4XX | Коды ошибок
403 | Монитор HTTP отключен, его необходимо включить в Dev Home
5XX | Коды ошибок

<br />
**Доступные семейства устройств**

* Windows Xbox