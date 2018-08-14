---
author: v-angraf
title: API REST портала устройств Xbox
description: Справочник по API для UWP на Xbox One.
ms.author: v-angraf
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: 5fc2ff964fdd5a03f2d507bcb38337fd4b370655
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921162"
---
# <a name="xbox-device-portal-rest-api"></a>API REST портала устройств Xbox

Этот раздел содержит справочные статьи об API REST портала устройств Xbox, который используется для удаленной настройки и администрирования консоли.

| Универсальный код ресурса (URI)        | Описание |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| Регистрирует приложение, содержащееся в свободной папке. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| Передает всю папку на консоль. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| Удаленная очистка всех доверенных PIN-кодов SSH. Требует выполнения повторного связывания по PIN-коду для разработки UWP в Visual Studio. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| Запрашивает сведения о развертывании для одного или нескольких установленных пакетов. |
|[/ext/fiddler](wdp-fiddler-api.md)| Включение и отключение трассировки сети Fiddler. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Получение трафика HTTP от приложения в фокусе на Xbox. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| Добавление, удаление или обновление сетевых учетных данных. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Удаленная отправка данных ввода с клавиатуры, мыши или контроллера на Xbox. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| Получите число подключенных физических контроллеров или отключите все физические контроллеры. |
|[/ext/screenshot](wdp-media-capture-api.md)| Захватывает представление экрана, отображаемого в данный момент на консоли, в формате PNG. |
|[/ext/settings](wdp-xboxsettings-api.md)| Предоставляет доступ к параметрам разработчика Xbox One. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| Предоставляет доступ к папке разработчика на консоли через проводник на компьютере, где ведется разработка. |
|[/ext/user](wdp-user-management.md)| Управляет пользователями на консоли Xbox One. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Предоставляет сведения об устройстве Xbox One. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Управляет песочницей Xbox Live. |

## <a name="see-also"></a>См. также

- [UWP на Xbox One](index.md)
- [Портал устройств Windows](../debug-test-perf/device-portal.md)