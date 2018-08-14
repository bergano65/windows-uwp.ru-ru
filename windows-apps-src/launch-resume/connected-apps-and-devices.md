---
author: PatrickFarley
title: Подключенные приложения и устройства (Project Rome)
description: В этом разделе описывается, как использовать платформу удаленных систем для обнаружения удаленных устройств, запуска приложения на удаленном устройстве и обмена данными со службой приложений на удаленном устройстве.
ms.author: pafarley
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: ff5871c444166770f2512e4e00b638a8fc57a9dd
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018488"
---
# <a name="connected-apps-and-devices-project-rome"></a>Подключенные приложения и устройства (Project Rome)

В этом разделе объясняется, как подключать приложения на различных устройствах и платформах с использованием платформы Project Rome. Узнайте, как обнаруживать удаленные устройства, запускать приложение на удаленном устройстве и обмениваться данными со службой приложений на удаленном устройстве.

У большинства пользователей имеется несколько устройств, и они часто начинают действие на одном устройстве, а завершают его на другом. Для учета такого поведения приложения должны работать на различных устройствах и платформах.

[API удаленных систем](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) появились в Windows 10 версии 1607 и позволяют создавать приложения, в которых пользователи могут начинать задачу на одном устройстве, а завершать ее на другом. В центре внимания остается задача, и пользователи могут выполнять свою работу на наиболее удобном устройстве. Например, в автомобиле пользователь может слушать радио по телефону, но, приехав домой, захочет продолжить прослушивание на системе Xbox One, подключенной к домашней стереосистеме.

Платформу Project Rome можно также использовать для сопутствующих устройств или сценариев удаленного управления. Используйте интерфейсы API служб сообщений приложения для создания канала приложения между двумя устройствами, чтобы отправлять и получать настраиваемые сообщения. Например, можно создать приложение для телефона, которое управляет воспроизведением на телевизоре, или сопутствующее приложение, предоставляющее сведения о героях телевизионного шоу, которое вы смотрите в другом приложении.  

Устройства можно подключать локально через Bluetooth и по беспроводной сети, или удаленно через облако; устройства связываются между собой учетной записью Майкрософт пользователя (MSA).

В разделе [Пример удаленных систем для UWP](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) приведены примеры обнаружения удаленной системы, запуска приложения в удаленной системе и использования службы приложений для передачи сообщений между приложениями, работающими в двух системах.

Дополнительные сведения о Project Rome, в том числе ресурсы для кросс-платформенной интеграции, можно найти в разделе [aka.ms/project-rome](https://aka.ms/project-rome).

| Статья | Описание |
|-------|-------------|
| [Запуск приложения на удаленном устройстве](launch-a-remote-app.md) | Узнайте, как запустить приложение на удаленном устройстве. В этом разделе рассматриваются простейший случай использования и предварительная настройка.  |
| [Обнаружение удаленных устройств](discover-remote-devices.md)  | Узнайте, как обнаруживать устройства, к которым можно подключиться. |
| [Обмен данными с удаленной службой приложений](communicate-with-a-remote-app-service.md) | Узнайте, как взаимодействовать с приложением на удаленном устройстве. |
| [Подключение устройств с помощью удаленных сеансов](remote-sessions.md) | Предоставляйте общие возможности на нескольких устройствах за счет их объединения через удаленный сеанс. |