---
title: Перечисление PermissionId
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " Перечисление PermissionId"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, игры, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594689"
---
# <a name="permissionid-enumeration"></a>Перечисление PermissionId
Сведения о перечислении PermissionId.
Разрешение идентификаторов можно использовать с URL-адреса, проверки разрешения:

   * [Получение (/users/ {requestorId} / разрешение/проверять)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/ {requestorId} / разрешение/проверять)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

Эти идентификаторы включают прямой проверки для конкретного значения параметра для пользователя, таких как проверка режим конфиденциальности одного целевого объекта или от одного прав субъекта. Кроме того есть разрешение идентификаторов, которые можно использовать с разрешением API и встраивать проверок несколько параметров для действий конкретного пользователя.

<a id="ID4EIB"></a>


## <a name="permissions"></a>Разрешения

Это значения, которые вызывающий объект можно использовать для проверки, можно ли выполнить определенное действие. В отличие от параметры, приведенные выше они инкапсулируют политики, определенной службой и нельзя изменить непосредственно пользователями, хотя в большинстве случаев эти политики основаны на один или несколько параметров, значения которых определяются пользователями. Это обычно составной проверки более чем один параметр, определенный выше. Пример. <b>ViewProfile</b> разрешение выполняющий проверку целевого объекта <b>ShareProfile</b> уровнем конфиденциальности и запрашивающей <b>AllowProfileViewing</b> прав доступа.

Как правило рекомендуется, вызывающим объектам запрашивать код разрешения для действий, которые должны быть проверены, вместо того чтобы непосредственно проверкой параметры конфиденциальности и права доступа. Благодаря этому политики конфиденциальности изменяемый согласованно через службу как включены новые проверки.

| Название разрешения| Описание|
| --- | --- |
| CommunicateUsingText| Проверьте ли пользователь может отправить сообщение с содержимым текста для целевого пользователя|
| CommunicateUsingVideo| Проверьте, может ли пользователь взаимодействовать с помощью видео с целевым пользователем|
| CommunicateUsingVoice| Проверьте, может ли пользователь взаимодействовать с помощью голоса с целевым пользователем|
| ViewTargetProfile| Проверьте ли пользователь может просмотреть профиль целевого пользователя|
| ViewTargetGameHistory| Проверьте ли пользователь может просмотреть историю игр целевого пользователя|
| ViewTargetVideoHistory| Проверьте ли пользователь может просмотреть подробную историю видео у целевого пользователя|
| ViewTargetMusicHistory| Проверьте ли пользователь может просмотреть журнал прослушивания музыки подробные целевого пользователя|
| ViewTargetExerciseInfo| Проверьте ли пользователь может просмотреть сведения упражнения целевого пользователя|
| ViewTargetPresence| Проверьте ли пользователь может просмотреть состояние целевого пользователя|
| ViewTargetVideoStatus| Проверьте ли пользователь может просмотреть сведения о видео состоянии целевых объектов (расширенные присутствии)|
| ViewTargetMusicStatus| Проверьте ли пользователь может просмотреть сведения о состоянии музыки целевых объектов (расширенные присутствии)|
| ViewTargetUserCreatedContent| Проверьте ли пользователь может просмотреть созданные пользователем содержимое других пользователей|