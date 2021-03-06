---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Обзор заметок о выпуске библиотек Microsoft Advertising.
title: Заметки о выпуске библиотек Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, рекламные объявления, реклама, заметки о выпуске
ms.localizationpriority: medium
ms.openlocfilehash: 377069522c6b31a55028bf35bf9c71ac50c90608
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506848"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Заметки о выпуске библиотек Microsoft Advertising

>[!WARNING]
> Начиная с 1 июня 2020 г. платформа Microsoft AD монетизацию для приложений Windows UWP будет выключена. [Подробнее](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Этот раздел содержит заметки о текущем выпуске библиотек Microsoft Advertising. Эти библиотеки поддерживают приложения XAML и JavaScript и HTML для Windows 10, Windows 8.1, Windows Phone 8,1 и Windows Phone 8.

## <a name="installation"></a>Установка


Библиотеки рекламы Microsoft теперь доступны в [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Дополнительные сведения об установке SDK см. в разделе [Установка Microsoft Advertising SDK](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Удаление предыдущих версий

Перед установкой последней версии Microsoft SDK настоятельно рекомендуется удалить все предыдущие экземпляры SDK. Подробные сведения см. в статье [Установка Microsoft Advertising SDK](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Сборки для определенных архитектур

При использовании библиотек Microsoft Libraries невозможно ориентироваться в проекте на **Любой ЦП**. Если проект ориентирован на платформу **Любой ЦП**, может отобразиться предупреждение после добавления ссылки в библиотеки Microsoft Advertising. Чтобы убрать это предупреждение, обновите проект, чтобы использовать сборку определенной архитектуры (например, **x86**). Подробнее см. в статье [Известные проблемы](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Поддержка C++

Библиотеки Microsoft Advertising (которые содержат классы **AdControl** и **InterstitialAd**) поддерживают приложения на C++ и DirectX благодаря взаимодействию с компонентами среды выполнения Windows (*interop*). Дополнительные сведения и примеры программирования с использованием XAML b C++ см. в разделе [Система типов](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Отсутствие элемента управления на панели инструментов

В текущем выпуске библиотек Microsoft Advertising в пакете [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) отсутствует элемент управления для перетаскивания классов **AdControl** или **InterstitialAd** в область конструктора приложения. Инструкции о добавлении этих элементов управления в разметку и код см. в разделе [Пошаговые руководства для разработчиков](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Свойства широты и долготы более недоступны

Класс **AdControl** более не имеет свойств **Latitude** и **Longitude** для приложений UWP. Вместо этого код, встроенный в элемент управления рекламным объявлением, обнаруживает и отправляет эти значения на серверы рекламы от имени приложения.


 

 
