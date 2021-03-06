---
title: Приступая к подключению приложений Node. js к базе данных
description: Приступая к подключению приложений Node. js к базе данных в Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Майкрософт, Learning NodeJS, Node в Windows, Node на WSL, Node в Linux в Windows, Установка узла в Windows, NodeJS с VS Code, разработка с помощью узла в Windows, разработка с помощью NodeJS в Windows, установка node на WSL, NodeJS в Windows Подсистема для Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517820"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Приступая к работе с MongoDB или PostgreSQL с Node. js в Windows

Приложениям Node. js часто приходится сохранять данные, которые могут происходить через файлы, локальное хранилище, облачные службы или базы данных. Это пошаговое руководство поможет приступить к подключению приложения Node. js к базе данных. Мы решили сосредоточиться на двух популярных вариантах: MongoDB и PostgreSQL.

## <a name="prerequisites"></a>Необходимые условия

В этом руководстве предполагается, что вы уже выполнили действия по [настройке среды разработки Node. js с помощью WSL 2](./setup-on-wsl2.md), включая:

- Установите Windows 10 Insider Preview Build 18932 или более поздней версии.
- Включите функцию WSL 2 в Windows.
- Установите дистрибутив Linux (Ubuntu 18,04 для наших примеров). Это можно проверить с помощью: `wsl lsb_release -a`
- Убедитесь, что дистрибутив Ubuntu 18,04 работает в режиме WSL 2. (WSL может запускать дистрибутивы в режиме v1 или v2.) Это можно проверить, открыв PowerShell и введя: `wsl -l -v`.
- С помощью PowerShell в качестве дистрибутива по умолчанию устанавливается Ubuntu 18,04 с: `wsl -s ubuntu 18.04`.

## <a name="differences-between-mongodb-and-postgresql"></a>Различия между MongoDB и PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Установка MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Поддержка VS Code MongoDB

VS Code поддерживает работу с базами данных MongoDB с помощью [расширения Azure CosmosDB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), вы можете создавать, управлять базами данных MongoDB и выполнять запросы из них в VS Code.

Дополнительные сведения см. в VS Code документах: [Работа с MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Дополнительные сведения см. в документах MongoDB:

- [Общие сведения об использовании MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Создание пользователей](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Подключение к экземпляру MongoDB на удаленном узле](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: создание, чтение, обновление, удаление](https://docs.mongodb.com/manual/crud/)
- [Справочные документы](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Установка PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Поддержка VS Code PostgreSQL

VS Code поддерживает работу с базами данных PostgreSQL через [расширение PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), вы можете создавать, подключать, управлять базами данных PostgreSQL и запрашивать их из VS Code.

## <a name="set-up-profile-aliases"></a>Настройка псевдонимов профилей

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
