---
Description: Узнайте, как использовать плитки, индикаторы событий, всплывающие и простые уведомления, чтобы предоставлять точки входа в свои приложения и держать пользователей в курсе последних событий.
title: Плитки, индикаторы событий и уведомления
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a87fe2bbff1768da43d6cb366b173077555270f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57583386"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>Плитки, индикаторы событий и уведомления для приложений UWP
 

Узнайте, как использовать плитки, индикаторы событий, всплывающие и простые уведомления, чтобы предоставлять точки входа в свои приложения и держать пользователей в курсе последних событий.

> **Важные API**: [пакет NuGet уведомлений набора средств сообщества UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Плитки используются для представления приложений в меню "Пуск". У каждого приложения UWP имеется плитка. Можно включить несколько размеров плиток (маленькая, средняя, широкая и большая).</p>

<p>Можно использовать <em>уведомление на плитке</em>, чтобы сообщать пользователю новую информацию, например, заголовки новостей или тему последнего непрочитанного сообщения.</p>

<p>Можно использовать <em>индикатор событий</em> для предоставления сводной информации или информации о статусе в виде предоставленных системой глифов или числа от 1 до 99. Индикаторы событий также отображаются на значке приложения в панели задач. </p>

<p><em>Всплывающее уведомление</em> — это уведомление, которое ваше приложение отправляет пользователю с помощью элемента всплывающего пользовательского интерфейса под названием <em>всплывающее уведомление</em> (или <em>баннер</em>). Уведомление отображается независимо от того, включено ли приложение.</p>
<p><em>Push-уведомление</em> или <em>необработанное уведомление</em> – это уведомление, отправляемое приложению из служб push-уведомлений Windows (WNS), или уведомление от фоновой задачи. Ваше приложение может реагировать на эти уведомления, либо сообщая пользователю о том, что произошло какое-то значимое событие (путем обновления индикатора событий, плитки или с помощью всплывающего уведомления), либо другим способом на ваш выбор.</p>

 
## <a name="tiles"></a>Плитки
| Статья | Описание |
| --- | --- |
| [Создание плиток](creating-tiles.md) | Настройте стандартную плитку для своего приложения и предоставьте ресурсы для разных размеров экрана. |
| [Ресурсы значков приложения](app-assets.md) | Ресурсы значков приложения, присутствующие в различных формах во всевозможных разделах операционной системы Windows 10, являются картами вызова приложения универсальной платформы для Windows (UWP). В данном руководстве рассказывается, где в системе расположены ресурсы значков приложений, а также приведены подробные советы по созданию безупречных значков. |
| [API основной плитки](primary-tile-apis.md) | Запросите закрепление основной плитки вашего приложения и убедитесь, что основная плитка в настоящее время закреплена. |
| [Содержимое плиток](create-adaptive-tiles.md) | Содержимое уведомления плитки указывается с помощью адаптивной новой функции в Windows 10, которая позволяет создавать уведомления плиток с помощью простого и гибкого языка разметки. В этой статье рассказывается, как создать адаптивные живые плитки для вашего приложения универсальной платформы Windows (UWP). |
| [Схема содержимого плитки](../tiles-and-notifications/tile-schema.md) | Ниже приводится список элементов и атрибутов, используемых для создания этих плиток. |
| [Специальные шаблоны плиток](special-tile-templates-catalog.md) | Специальные шаблоны плиток — это уникальные шаблоны, которые могут быть анимированными или предоставлять возможности, не поддерживаемые адаптивными плитками. |
| [Отправка локального уведомления на плитке](sending-a-local-tile-notification.md) | Узнайте, как отправлять локальное уведомление на плитке, добавляя форматированное динамическое содержимое на живую плитку. |


## <a name="notifications"></a>Уведомления

| Статья | Описание |
| --- | --- |
| [Всплывающие уведомления](adaptive-interactive-toasts.md) | Функция адаптивных и интерактивных всплывающих уведомлений позволяет создавать удобные всплывающие уведомления с большим объемом содержимого, встроенными изображениями и дополнительными возможностями взаимодействия с пользователем. |
| [Отправка локального всплывающего уведомления](send-local-toast.md) | Узнайте, как отправлять интерактивное всплывающее уведомление. |
| [Визуализатор уведомлений](notifications-visualizer.md) | Визуализатор уведомлений — это новое приложение универсальной платформы Windows (UWP) в [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1), которое помогает разработчикам создавать адаптивные живые плитки для Windows 10. |
| [Выбор способа доставки уведомлений](choosing-a-notification-delivery-method.md) | В этой статье рассматривается четыре варианта уведомлений локальные, запланированные, периодические и push-уведомления для доставки обновлений плиток и индикаторов событий, а также содержимого всплывающих уведомлений. |
| [Обзор периодических уведомлений](periodic-notification-overview.md) | Периодические, или опросные, уведомления обновляют плитки и индикаторы событий через фиксированные интервалы, скачивая содержимое напрямую из облачной службы. |
| [Обзор служб push-уведомлений Windows (WNS)](windows-push-notification-services--wns--overview.md) | Службы push-уведомлений Windows (WNS) позволяют сторонним разработчикам отправлять обновления всплывающих уведомлений, плиток, индикаторов событий и необработанные обновления из своей облачной службы. Это обеспечивает энергоэффективный и надежный механизм передачи пользователям свежих обновлений. |
| [Код, генерируемый мастером push-уведомлений](the-code-generated-by-the-push-notification-wizard.md) | С помощью мастера Visual Studio можно формировать push-уведомления из мобильной службы, созданной средствами мобильных служб Windows Azure. Мастер Visual Studio формирует код, облегчая начало работы. В этом разделе объясняется, как мастер изменяет ваш проект, что делает сформированный код, как его использовать и что надо сделать дальше для максимально полного использования преимуществ push-уведомлений. См. [общую информацию о службах push-уведомлений Windows (WNS)](windows-push-notification-services--wns--overview.md). |
| [Общие сведения о необработанных уведомлениях](raw-notification-overview.md) | Необработанные уведомления представляют собой короткие push-уведомления общего характера. Они являются строго поясняющими и не включают компонент пользовательского интерфейса. Как и в случае других push-уведомлений, необработанные уведомления из облачной службы доставляются в ваше приложение с помощью функции WNS. |
