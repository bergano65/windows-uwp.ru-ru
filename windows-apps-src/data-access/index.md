---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Доступ к данным
description: В этом разделе описывается хранение данных на устройстве в частной базе данных и использование объектно-реляционного отображения в приложениях универсальной платформы Windows (UWP).
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, данные, база данных, реляционная, таблицы, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: 80cf97a7336fe912baf5cda151560dacfa12c2e2
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339712"
---
# <a name="data-access"></a>Доступ к данным

Данные можно хранить на устройстве пользователя, используя базу данных SQLite. Можно также подключить приложение непосредственно к базе данных SQL Server, не используя какой-либо уровень служб.

| Раздел | Описание|
|-------|------------|
| [Использование базы данных SQLite в приложении UWP](sqlite-databases.md) | Описывается, как можно использовать SQLite для хранения и извлечения данных из упрощенной базы данных на устройстве пользователя. SQLite – это бессерверное встраиваемое ядро СУБД. |
| [Использование базы данных SQL Server в приложении UWP](sql-server-databases.md) | Показывается, как ваше приложение может подключаться напрямую к базе данных SQL Server и затем хранить и извлекать данные с помощью классов в пространстве имен [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient). Уровень служб не требуется. |

## <a name="related-topics"></a>Статьи по теме

* [Пример базы данных Customer Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
