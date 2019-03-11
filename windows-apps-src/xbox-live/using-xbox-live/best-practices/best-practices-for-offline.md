---
title: Рекомендации для автономного режима
description: Дополнительные сведения о рекомендации по обработке сценарии автономной работы с заголовками, Xbox Live включена.
ms.assetid: 6290dd67-1145-4fe2-8ada-c3a29a9ad29a
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10 для настольных ПК, xbox, вне сети
ms.localizationpriority: medium
ms.openlocfilehash: 5680b045873ebf6eed1422cb915f3f53df748790
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618729"
---
# <a name="best-practices-for-offline"></a>Рекомендации для автономного режима

Xbox One разработан как подключенное устройство, и получить все возможности, такие как многопользовательские игры и потоковое видео, требуется подключение. Тем не менее Xbox One поддерживает многие виды воспроизведения в автономном режиме. Здесь описывается, как обрабатывать воспроизведение в автономном режиме.

## <a name="overview"></a>Обзор

Постоянно растущего числа клиентов по всему миру имеют доступ к Интернету. Однако существуют Вырезать по-прежнему местах в мире где непредсказуем подключения, а иногда, если не проходят маршрутизаторы, волокон, сбоя серверов и удалять беспроводных служб.

Для поддержки широкого набора потребителей и возможности, Xbox One позволяет распространенные случаи, когда теряется время от времени подключения к Интернету, или полностью недоступен. Игры сведения об проблемам с подключением, и Вы свободны в выборе способы реагирования — ли, продолжает полный игровой процесс, обратный переход к автономный режим, или полностью конечная игровой процесс.

## <a name="normal-online-operation"></a>Обычной интерактивной операции

Как правило консоли Xbox One работают в состоянии полностью подключен, где пользователь имеет постоянное подключение к Интернету и полный доступ к Xbox Live и сторонними службами. Это состояние подключения позволяет службе Xbox Live периодически проверяет состояние консоли, и для предоставления обновлений и для выполнения других фоновых служб, которые выигрывают игру и пользователей.

Мы рекомендуем, предполагается, что пользователи имеют доступа к Интернету большую часть времени.

## <a name="offline-play-principles"></a>Принципы воспроизведения в автономном режиме

Бывают случаи, где недоступен подключение к Интернету. Для автономного воспроизведения Xbox One разработан с в виду следующие принципы:

-   Самое главное пользователям продолжать играть несмотря на проблемы с подключением.

-   Пользователи продолжаться даже если они вообще нет соединения.

-   Сделать автономного воспроизведения простыми и предсказуемыми для пользователей, сохраняя при этом духе работы, всегда подключены.

## <a name="offline-modes"></a>Автономным режимами

Существует две высокоуровневые *потеря подключения* сценариев:

-   Полной потери службы Интернета

-   Потеря одного или нескольких веб-служб

В каждом из этих режимов существует множество ситуаций, которые могут возникнуть. Они описаны ниже с примеры распространенных сценариев вне сети, которые влияют на игровой процесс.

## <a name="offline-scenario-no-internet-service-upon-game-start"></a>Сценарий автономного режима: нет службы Интернета после запуска игр

Игры может объявлять себя как один из трех типов:

-   *Xbox Live требуется*: всеми видами игровой процесс требуется подключение к Интернету.

-   *Xbox Live Gold необходимые*: всеми видами игровой процесс требуется подключение к Интернету и Xbox Live Gold членства.

-   *Xbox Live не требуется*: игра имеет по крайней мере один режим игры, которые не требуют подключения к Интернету. С технической точки зрения этот тип не объявлен явно в манифесте приложения. Приложение, которое не объявлять себя, так как один из первых двух типов считается «Не требуется, Xbox Live» или автономном режиме поддерживается.

Когда Игра запущена пользователем, и консоль находится в автономном режиме, система проверяет объявление игр подключения в манифесте приложения. Если игра требует подключения (одно из первых двух случаях выше), система автоматически отображает сообщение для пользователя и не запускается заголовок.

Если консоль находится в автономном режиме, система будет запускать только те продукты, которые имеют по крайней мере одного режима игры, которые не требуют подключения. Другими словами система не запустится, «Xbox Live требуется» или «required Xbox Live Gold» заголовки.

## <a name="offline-scenario-connectivity-lost-during-gameplay"></a>Автономный сценарий: потерянное соединение во время игровой процесс

Если игра уже выполняется, при сбое подключения к, заголовок будет уведомляться по системе. Если игры не использует веб-служб, продолжить работу без прерывания. Если игра активно использует веб-служб, переключение в режим, где эти службы больше не нужны или информирования проигрыватель, что игровые сеанс завершается из-за состояния.

Заголовки, которые являются, объявленные как «Xbox Live required» или «required Xbox Live Gold» будет автоматически приостановлена систему, если консоль теряет все подключения к сети в течение заданного времени и системой автоматически предоставит пользователю сообщение об ошибке.

В любом другом сценарии, включающие приостановки игровой процесс, после сохранения состояния таким образом пользователь не потерять данные и возможность быстро вернуться к этому состоянию после сервером, вновь образуя подключения.

## <a name="offline-scenario-when-a-single-xbox-live-service-is-down"></a>Сценарий автономного режима: Если одна служба Xbox Live не работает

Существуют другие ситуации, в которых определенные веб-служб могут быть недоступны несмотря на то, что подключение к Интернету подходит.

Например одна служба Xbox Live может работать в автономном режиме на короткое время. В этом случае вызов конкретный службы будет превышено время ожидания или сообщить об ошибке в игру. Состояния недоступной службе должен правильно обрабатывать так же, как будет обрабатывать эти ситуации на Xbox 360 или Windows.

Как минимум, игра должна не аварийному завершению работы или зависанию. Если игровой процесс не может продолжить работу без службы, сообщить об этом пользователю и разрешить пользователю продолжить работу в другой области игры, когда служба не требуется.

В лучшем случае по-прежнему игрового процесса и кэш данных для последующей отправки (Если игра запись службы) или сделать обоснованные предположения о данных (Если игра читает из службы).

## <a name="offline-scenario-when-a-third-party-service-is-down"></a>Сценарий автономного режима: Если сторонняя служба не работает

Если игры полагается на сторонних веб-службы, то игры также должен быть устойчивым в том случае, если происходит сбой этой службы. В случае сбоя службы, затем игры должен не сбой или зависание. Ошибка службы может сообщать пользователю, если не удается продолжить игровой процесс. В идеале следует продолжить игровой процесс или должен позволять пользователю в области игру, которая не требует веб-службе продолжить.

## <a name="offline-scenario-when-xbox-live-cloud-compute-is-down"></a>Сценарий автономного режима: Если Xbox Live облачных вычислений не работает

Одна из Xbox One демонстрирует — сила облака. Некоторые игры может полностью использовать всегда подключенную службу, например Xbox Live облачных вычислительных ресурсов, который позволяет доступ к дополнительные вычислительные возможности или всегда доступен игровых серверов. Это всегда подключенном режиме допускается и рекомендуется, где он приносит новые возможности для игроков.

Если игра использует этот режим, то мы рекомендуем использовать свою игру устойчивым к перерывов в обслуживании (несколько секунд и до одной минуты), которые из-за либо общее потеря подключения к Интернету или определенной облачной службы. Тем не менее игра не требуется иметь автономный режим. Если подключение недоступно, игры, по-настоящему должен быть подключен, затем информировать пользователей и завершить сеанс игровой процесс.

## <a name="xbox-requirements"></a>Требования к Xbox

Самым важным требованием при работе с сценарии автономной работы — игры стабильности. Будь то завершения подключения к потере или только определенных служб online, игры должны не зависает, зависание или пользователь потеряет состояние. Игра должна иметь надежную систему для обработки сценариев времени ожидания сети и для обработки ошибок, возвращаемых из любого интерфейса API, которое обращается к веб-службы.

Игры не требуются для поддержки воспроизведения в автономном режиме. Если вашей игре просто продолжение невозможно из-за потери подключения к службе, затем уведомлять пользователя, завершения сеанса игры и вернуться в главное меню и начальное состояние интерактивного.

## <a name="best-practices"></a>Рекомендации

Ниже приведены рекомендации по работе с автономной ситуациях.

-   Разработка игр предположить, что пользователи имеют доступа к Интернету большую часть времени.

-   Если имеет смысл для данного дизайна, игры, возможно, стоит проектирование режимы игровой процесс, которые пользователи могут иметь интерфейс разработок, даже если консоль находится в автономном режиме.

-   Службы могут стать недоступными. Подключения может завершиться ошибкой. Создавайте надежную обработку ошибок для API-интерфейсов, можно тайм-аут, или сбоях отчета при веб-служба не работает или если потеряно подключение к Интернету. Когда это возможно, пользователи продолжаться несмотря на эти проблемы.

-   Соблюдать требования Xbox (XRs). Не зависание или сбой.

-   При получении уведомления приостановки PLM title, сохраните состояние, чтобы не потерять данные пользователя и возможность быстро вернуться к этому состоянию, при выходе игра.

-   Флаг заголовка, соответствующим образом в манифесте приложения. Только пометьте название как «required Xbox Live» Если всеми видами игровой процесс требуется подключение.

-   Игры для Xbox One разрешено использовать и полагаться на веб-служб во всех режимах игр. Если игра просто будет прервана из-за потери подключения к службе, затем уведомлять пользователя, завершения сеанса игры и вернуться в главное меню и начальное состояние интерактивный.

-   Не следует полагаться на службу Xbox справки для сообщения об ошибках и помочь соответствующим состояниям в автономном режиме. Службы Xbox справки требуется подключение к Xbox Live.

## <a name="conclusion"></a>Заключение

Всегда мир рассчитана Xbox One. Тем не менее на его пути к этой концепции, консоль позволяет игрокам слушать сценарии автономной работы. Игры должна быть надежной при сбоях подключения. Для игр с помощью автономного воспроизведения возможности позволяют игроков слушать эти возможности для максимального. Для игр, которые всегда находятся в режиме при потере подключения к сети корректно возвращать страницу в автономном режиме.