---
title: Многопользовательские инструкции
description: В этой статье описывается реализация распространенных задач в 2015-многопользовательскую Xbox Live.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10 для настольных ПК, xbox, многопользовательские 2015
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648639"
---
# <a name="multiplayer-how-tos"></a>Инструкции по многопользовательскому режиму

Этот раздел содержит сведения о том, как реализовать определенные задачи, связанные с использованием многопользовательские 2015.

* Подпишитесь на уведомления об изменении сеанса MPSD
* Создайте сеанс MPSD
* Задайте арбитра для сеанса MPSD
* Управление Title активации
* Сделать пользователя присоединяемая
* Отправка приглашений в игры
* Присоединиться к сеансу из сеанса основной игр
* Веб-трансляции MPSD от активации заголовка
* Задать текущее действие пользователя
* Обновить сеанс MPSD
* Оставьте сеанс MPSD
* Заливка слотов открытый сеанс во время подбор игроков
* Создайте запрос в службу соответствия
* Получение состояния билет соответствия

## <a name="subscribe-for-mpsd-session-change-notifications"></a>Подпишитесь на уведомления об изменении сеанса MPSD

| Примечание.                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Подписка для изменения сеанса требуется связанный проигрыватель, должна быть активной в сеансе. Поле connectionRequiredForActiveMembers также должно быть присвоено значение true, если в объекте /constants/system/capabilities для сеанса. Это поле обычно задается в шаблоне сеанса. См. в разделе [MPSD сеанса шаблоны](multiplayer-session-directory.md). |



Чтобы получать уведомления об изменении сеанса MPSD, заголовок можно выполните следующие действия.

1.  Используйте тот же **XboxLiveContext класс** объекта для всех вызовов, тем же пользователем. Подписки связаны с временем существования этого объекта. При наличии нескольких локальных пользователей, используйте отдельный **XboxLiveContext** объекта для каждого пользователя.
2.  Реализуйте обработчики событий **событий RealTimeActivityService.MultiplayerSessionChanged** и **RealTimeActivityService.MultiplayerSubscriptionsLost событий**.
3.  Если подписка на изменения для более чем одного пользователя, добавьте код для вашего **MultiplayerSessionChanged** обработчик событий, чтобы избежать ненужной работы. Используйте **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.Branch** и **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber свойство**. Использование этих свойств позволяет отслеживания последнего изменения видны и пропуск старых изменений.
4.  Вызовите **MultiplayerService.EnableMultiplayerSubscriptions метод** чтобы разрешить подписки.
5.  Создание объекта локальный сеанс и соединения данного сеанса, как активные.
6.  Вызовы для каждого пользователя для **MultiplayerSession.SetSessionChangeSubscription метод**, передаче сеанса изменить тип, для которого требуется получать уведомления.
7.  Теперь записи сеанса, как описано в разделе MPSD **как: Обновление многопользовательской**.

Следующий блок-схема показывает, как запустить Multplayer путем подписки на события, описанные в этой процедуре.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>Синтаксический анализ уведомления об изменении повторяющиеся сеанса

При наличии нескольких пользователей, которые подписаны на уведомления для одного сеанса, каждое изменение в этом сеансе активирует отвод плечо, сможет узнать, для каждого пользователя. Все, кроме одного из них будут повторяющиеся значения. Хотя по-прежнему рекомендуется подписаться, Заголовок каждого пользователя в сеансе на уведомления, заголовок должен игнорировать все изменения, которые уже были получать уведомление Это можно сделать с помощью свойства «ветви» и «ChangeNumber.

Чтобы обнаружить несколько касания плечо, сможет узнать, заголовок должен выполнить следующее.

-   Для каждого **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.Branch** значение, отображающееся, последние хранилище **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber свойство**.
-   Если отвод плечо, сможет узнать имеет более высокий **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** чем последнего видел, **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch свойство** , обработать его и обновить последнюю версию **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber свойство**.
-   Если отвод плечо, сможет узнать, имеет более высокий **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** для этого **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.Branch**, пропустить этот шаг. Это изменение будет обработан.

| Примечание.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Свойство RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** значения должны отслеживаться по **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch свойство**, а не по сеанса. Существует возможность **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.Branch** измените значение (и **свойство RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber**для сброса) за время существования сеанса. |

## <a name="create-an-mpsd-session"></a>Создайте сеанс MPSD


| Примечание.                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| По умолчанию сеанс MPSD создается при добавлении первого элемента, его. Если ваша логика title ожидает заголовок существует или не существует во время соединения, можно передать значение режима записи, соответствующие методу записи во время обновления сеанса. |



Заголовок, выполните следующую команду, чтобы создать новый сеанс:

1.  Создайте новый **XboxLiveContext класс** объекта. Название создает этот объект один раз, сохраняет его и повторно использует его при необходимости в исходном коде. Особенно при работе с подписками сеанса, бывает необходимо использовать именно тот же контекст.
2.  Создайте новый **MultiplayerSession класс** объекта, чтобы подготовить все данные сеанса, MPSD должен создать новый сеанс.
3.  Внесите необходимые изменения перед записью MPSD сеанса. Например, при присоединении к сеансу с помощью вызова члена **MultiplayerSession.Join метод**, клиент добавляет данные скрытый локальный запрос, сообщающий MPSD для присоединения после вызова для обновления сеанса.
4.  По завершении внесения изменений в локальном записывать их MPSD как описано в разделе **как: Обновление многопользовательской**.
5.  Получение нового **MultiplayerSession** объект из MPSD, имеющими много полей заполнены.
6.  Объект нового сеанса, в дальнейшем использовать и удалить старую копию, которая содержит скрытый запрос на создание нового сеанса.

### <a name="example"></a>Пример

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>Задайте арбитра для сеанса MPSD




Заголовок использует следующую процедуру для установки арбитра для сеанса, который уже был создан.

| Примечание.                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Маркеры устройств для членов (потенциальных узлов) недоступны, пока присоединен сеанса и включены их адреса устройство обеспечения безопасности элементов. |

1.  Получить маркеры устройств для потенциальных компьютеров из MPSD с помощью **MultiplayerSession.Members свойство**.

| Примечание.                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Если сеанс создан с помощью SmartMatch подбор игроков, могут использовать клиенты кандидатов узлов, доступных из MPSD через **MultiplayerSession.HostCandidates свойство**. |

2.  Выберите необходимые узел из списка потенциальных компьютеров.
3.  Вызовите **MultiplayerSession.SetHostDeviceToken метод** задать маркер устройства в локальном кэше MPSD. Если вызов установить маркер устройства для узлов завершается успешно, маркер локального устройства заменяет маркер главного приложения.
4.  Если код состояния HTTP/412 получено при попытке установить маркер устройства для узлов, запроса данных сеанса и находится ли узел маркер устройства для локальной консоли. Если это не для локальной консоли еще одна консоль обозначается как арбитр.

| Примечание.                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Клиент должен обрабатывать код состояния HTTP/412 отдельно от остальных кодов HTTP, так как HTTP/412 не указывают на стандартный сбой. Для получения дополнительных сведений о коде состояния см. в разделе [коды состояния сеанса многопользовательскую](multiplayer-session-status-codes.md). |

5.  Обновите этот сеанс в MPSD, как описано в **как: Обновление многопользовательской**.

| Примечание.                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Если у вас нет эффективный алгоритм, клиент можно реализовать "жадный" алгоритм, в котором каждого кандидата узел пытается задать себя в качестве узла, если никто другой не сделали это. Дополнительные сведения см. в разделе [арбитр сеанса](mpsd-session-details.md). |

## <a name="manage-title-activation"></a>Управление title активации

Xbox One активируется **CoreApplicationView.Activated событий** во время активации протокола. В контексте многопользовательских API это событие возникает, когда пользователь принимает приглашение, или присоединяет другого пользователя. Эти действия триггера активации, заголовок должен реагировать на них по приведению подключающегося пользователя в игры с целевым пользователем.

| Примечание.                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| Название следует ожидать, что новые аргументы активации в любое время и никогда не должны содержать в коде для длины. |

Заголовок необходимо выполнить следующие основные шаги для обработки заголовка активации.

1.  Настроить обработчик событий для **CoreApplicationView.Activated событий**. Этот обработчик запускает каждый раз при активации протокола происходит, даже если заголовок уже выполняется.
2.  При активации title начать сеанс, Подпишитесь на уведомления об изменении сеанса. См. в разделе **как: Подпишитесь на уведомления об изменении сеанса MPSD**.
3.  Присоединение пользователя к сеансу как активные. См. в разделе **как: Веб-трансляции MPSD от активации Title**.
4.  Установка сеанса основной действия сеанса, предоставляются через Интерфейс профиля. См. в разделе **как: Задать текущее действие пользователя**.
5.  Присоединение пользователя к игр сеанса как активные. Теперь пользователь может соединиться с коллегами и введите игры или холле.

Блок-схеме ниже показана обработка активации title.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>Сделать пользователя присоединяемая

Чтобы назначить пользователя присоединяемая, заголовок необходимо выполнить следующие:

1.  Создать объект сеанса и изменения атрибутов при необходимости.
2.  Присоединение пользователя к сеансу как активные. См. в разделе **как: Веб-трансляции MPSD от активации Title**.
3.  Определите, если пользователь, обозначенный как арбитр сеанса.
4.  Если пользователь не входит в группу concurrentreceivergroup, перейдите к шагу 7.
5.  Если пользователь является данный арбитр, вызвать **MultiplayerSession.SetHostDeviceToken метод**.
6.  Попытки записи сеанса, используя вызов **MultiplayerService.TryWriteSessionAsync метод**.
7.  Сеанс в качестве активного сеанса. См. в разделе **как: Задать текущее действие пользователя**.

Следующий блок-схеме показаны шаги, чтобы разрешить пользователю объединить с другими пользователями во время игры.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>Отправка приглашений в игры

Заголовок можно включить проигрывателя для отправки игры приглашает одним из следующих способов:

-   Отправьте приглашения для сеанса основной.
-   Отправьте приглашения, с помощью универсального платформы Xbox пригласить пользовательского интерфейса со ссылкой на игры сеанса.

Для отправки приглашает игры для проигрывателя, заголовок необходимо выполнить следующие:

1.  Сделать его приглашающий игр присоединяемую. См. в разделе **как: Сделать пользователя присоединяемая**.
2.  Определить потребность в приглашениях к отправке через сеанс основной или с помощью приглашения пользовательского интерфейса.
3.  Если к сеансу lobby отправлять приглашения, используя вызов **MultiplayerService.SendInvitesAsync метод**. Этот метод может потребоваться создание списка пользовательского интерфейса в игре с помощью **метод SystemUI.ShowPeoplePickerAsync** или **PartyChat.GetPartyChatViewAsync метод**.
4.  Если использовать сообщение invite пользовательского интерфейса, вызвать **SystemUI.ShowSendGameInvitesAsync метод** Показать сообщение invite пользовательского интерфейса.
5.  Обрабатывать **RealTimeActivityService.MultiplayerSessionChanged событий** для локальный исполнителя после соединения удаленного проигрывателя.
6.  Для удаленных проигрывателей Реализуйте код активации title. См. в разделе **как: Управление активации Title**.

Блок-схеме ниже показано, как отправлять приглашения.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>Присоединиться к сеансу из сеанса основной игр

Сеансы игровой процесс на устройствах Windows 10 должен иметь `userAuthorizationStyle` присвоено возможность **true** не больших сеансов. Это означает, что `joinRestriction` свойство не может быть «none», это означает, что сеанс не может быть открыто присоединяемая напрямую.

Распространенным сценарием является создание сеанса основной для сбора игроков и затем переместить эти игроков в игровой процесс, для сеанса или сеанса подбор игроков. Но если сеанс игровой процесс не является публично присоединяемая, то игр клиенты не смогут присоединиться к сеансу, игровой процесс, только если они отвечают требованиям `joinRestriction` параметр, который является слишком строгим, в большинстве случаев для этого сценария.

Решение позволяет связать основной сеанса и игр сеанса передачи дескриптора.  Заголовок это можно сделать следующим образом:

1. При создании игр сеанса, используйте `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API для создания передачи дескриптор, который связывает сеанс основной и игр сеанса.
2. Store дескриптор передача GUID в сеансе lobby вместо ссылки на сеанс игр сеанса.
3. Когда заголовок требуется для перемещения элементов из основной сеанса к сеансу игр, каждый клиент будет производить передачу дескриптор из сеанса основной веб-трансляции игр с помощью `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API.
4. Убедитесь, что члены, попытка веб-трансляции игр с помощью передачи дескриптора также в сеансе lobby сеанс lobby ищет MPSD.
5. Если элементы в сеансе lobby, они смогут доступ к сеансу, игр.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>Веб-трансляции MPSD от активации заголовка

Когда пользователь нажимает на присоединение к друга действий или принять приглашение, с помощью оболочки Xbox пользовательского интерфейса, заголовок активируется с параметрами, которые указывают, какой сеанс пользователя хочу присоединиться к. Заголовок должен обрабатывать эту активацию и добавить пользователя в соответствующий сеанс.

Ниже приведены шаги, которые следует выполнить заголовок.

1.  Реализуйте обработчик события для **CoreApplicationView.Activated событий**. Это событие сообщает число активаций для заголовка.
2.  Когда обработчик запускает, изучите **IActivatedEventArgs.Kind свойство**. Если он имеет значение протокола, приводится к типу аргументы события **ProtocolActivatedEventArgs класс**.
3.  Изучите **ProtocolActivatedEventArgs** объекта. При наличии соответствующей URI в **ProtocolActivatedEventArgs.Uri свойство** соответствует либо inviteHandleAccept (соответствующий приглашение принятые) или activityHandleJoin (соответствующие для соединения с помощью оболочки пользовательского интерфейса), анализа строки запроса URI, который форматируется как обычную строку запроса URI с парами "ключ значение", извлечение следующие поля:
    -   Для принятого приглашения:
        1.  Дескриптор
        2.  invitedXuid
        3.  senderXuid
    -   Для соединения:
        1.  Дескриптор
        2.  joinerXuid
        3.  joineeXuid

4.  Запустите многопользовательские code title, который должен содержать вызов **MultiplayerService.EnableMultiplayerSubscriptions метод**.
5.  Создайте локальную **класс MultiplayerSession** объекта, использование **MultiplayerSession конструктор (XboxLiveContext)**.
6.  Вызовите **метод MultiplayerSession.Join (String, Boolean, Boolean)** для подключения к сеансу. Таким образом, чтобы соединение устанавливается как активные, используйте значения следующих параметров:
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  Вызовите **MultiplayerSession.SetSessionChangeSubscription метод** для shoulder отвода при изменении сеанса после присоединения.
8.  Вызовите **MultiplayerService.WriteSessionByHandleAsync метод**, используя дескриптор, полученный, как описано в шаге 3. Теперь пользователь является членом сеанса и данные в сеансе можно использовать для подключения к игре.

## <a name="set-the-users-current-activity"></a>Задать текущее действие пользователя

Текущее действие пользователя отображается в Xbox панель мониторинга с пользователями для заголовка. Можно задать для пользователя через сеанс или с помощью активации заголовка. В последнем случае пользователь вводит сеансу через партнеров или с запуска игры.

| Примечание.                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Действие, которые задаются в сеансе можно удалить с помощью вызова **MultiplayerService.ClearActivityAsync метод**. |

Для установки текущего действия пользователя, вызовы title **MultiplayerService.SetActivityAsync метод**, передавая ссылку сеанса для сеанса.

Чтобы задать текущее действие пользователя через заголовок активации, см. в разделе **как: Веб-трансляции MPSD от активации Title**.

## <a name="update-an-mpsd-session"></a>Обновить сеанс MPSD

| Примечание.                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Когда заголовок вашей обновляет существующий сеанс с помощью многопользовательские API, помните, что он работает с локальной копией, пока он осуществляет вызов для записи сеанса. |

Чтобы обновить существующий сеанс, заголовок должен:

1.  Вносить изменения в текущий сеанс, при необходимости, например, путем вызова **MultiplayerSession.Leave метод**.
2.  При внесении всех изменений, записи локальные изменения MPSD, с помощью любого из этих методов:

    -   **Метод MultiplayerService.WriteSessionAsync**
    -   **Метод MultiplayerService.WriteSessionByHandleAsync**.
    -   **Метод MultiplayerService.TryWriteSessionAsync**
    -   **Метод MultiplayerService.TryWriteSessionByHandleAsync**

    Задайте режим записи **SynchronizedUpdate** Если запись в общей части другие заголовки можно изменить. См. в разделе [синхронизировать обновления сеанса](multiplayer-session-directory.md) Дополнительные сведения.

    Метод write записывает соединения на сервер и получает последнего сеанса, из которых должны быть найдены другими участниками сеанса и безопасные устройства адресов (SDAs) консоли. Дополнительные сведения о подключения к сети среди этих консолей см. в разделе **введение Winsock на Xbox One**.

3.  Удаляет объект старый локальный сеанс и использовать объект сеанса только что полученный таким образом, чтобы будущие действия основаны на известный сеанс последнее состояние.

## <a name="leave-an-mpsd-session"></a>Оставьте сеанс MPSD

Чтобы разрешить пользователю оставить сеанс, заголовок необходимо выполните следующие действия.

1.  Вызовите **MultiplayerSession.Leave метод** для игр сеанса.
2.  Обновите игр сеанса в MPSD, как описано в **как: Обновление многопользовательской**.
3.  При необходимости вызовите **оставьте** метод для сеанса основной и обновление этого сеанса.
4.  При необходимости для сеанса основной, завершение работы многопользовательской API, отменив регистрацию **событий RealTimeActivityService.MultiplayerSubscriptionsLost** и  **Событие RealTimeActivityService.MultiplayerSessionChanged**.

Следующий блок-схема показывает, как выйти из сеанса и завершает работу.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>Заливка слотов открытый сеанс во время подбор игроков

Для заполнения открытых слотов билет сеанса во время подбор игроков, заголовок, выполните действия, следующего вида:

1.  Доступ к актуального состояния сеанса для билета сеанса, созданные во время подбор игроков.
2.  Добавьте доступные проигрывателей для игры из сеанса основной.
3.  Определите, если билет сеанса заполнен.
4.  Если сеанс заполнен, по-прежнему игры.
5.  Если сеанс не заполнен, создать билет соответствия, как описано в разделе **как: Создайте запрос в службу соответствия**. Не забудьте создать запрос в службу *preserveSession* параметр значение всегда.
6.  Продолжайте подбор игроков. См. в разделе [с помощью Координирующему SmartMatch](using-smartmatch-matchmaking.md).

Следующий блок-схема показывает, как для заполнения слотов открытый сеанс во время подбор игроков.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>Создайте запрос в службу соответствия

Создать билет совпадение, подбор игроков scout необходимо:

1.  Вызовите **MatchmakingService.CreateMatchTicketAsync метод**, передавая ссылку билет сеанса. Метод считывает билет сеанса из MPSD и начинает подбор игроков для пользователей в сеансе. Внутренне вызывает метод **POST (/hoppers/ /serviceconfigs/ {scid} {hoppername})**.
2.  Задайте *preserveSession* параметр никогда, если служба подбор игроков в соответствии с членами сеанса в новый сеанс или другого существующего сеанса. Задайте *preserveSession* параметр всегда, чтобы разрешить это имя, чтобы повторно использовать существующий сеанс игр билет сеанса для продолжения игры. Подбор игроков службы затем позволяет гарантировать, отправленной сеанса сохраняется и все соответствующие игроков добавляются к этому сеансу.

3.  Используйте **свойство CreateMatchTicketResponse.EstimatedWaitTime** возвращаются в **CreateMatchTicketResponse класс** объекта обозначить ожидания пользователей времени подбор игроков.
4.  Используйте **CreateMatchTicketResponse.MatchTicketId свойство** возвращается в объекте ответа для отмены координирующему для сеанса, при необходимости, удалив билета. Билета использует удаления **MatchmakingService.DeleteMatchTicketAsync метод**.

## <a name="get-match-ticket-status"></a>Получение состояния билет соответствия

Название должно выполните следующие действия для получения сведений о состоянии соответствия билета.

1.  Получить **MultiplayerSession класс** объекта билет сеанса.
2.  Используйте **свойство MultiplayerSession.MatchmakingServer** для доступа к **MatchmakingServer класс** объект, используемый в подбор игроков.
3.  Проверьте **MatchmakingServer** объектом, чтобы определить состояние процесса подбор игроков, время ожидания, типичные для сеанса и ссылка на целевой объект сеанса, если не было найдено совпадение.