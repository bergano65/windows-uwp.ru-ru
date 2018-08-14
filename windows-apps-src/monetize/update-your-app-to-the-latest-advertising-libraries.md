---
author: mcleanbyron
description: Узнайте, как обновить ваше приложение для использования последних поддерживаемых версий библиотек Microsoft Advertising, чтобы приложение продолжало получать рекламные баннеры.
title: Обновление приложения для использования последних рекламных библиотек для баннеров
ms.author: mcleans
ms.date: 08/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, рекламные объявления, реклама, AdControl, AdMediatorControl, переход
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: 76581de948a4bb62597443e389122298f69c561d
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654813"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>Обновление приложения для использования последних рекламных библиотек для баннеров

С 1 апреля 2017 г. мы больше не предоставляем баннеры для приложений, использующих неподдерживаемый выпуск пакета SDK для рекламы. Если вы используете **AdControl** для показа рекламных баннеров в приложении универсальной платформы Windows (UWP), используйте сведения в этой статье, чтобы определить, используете ли вы неподдерживаемый рекламный SDK, и перевести ваше приложение на поддерживаемый пакет SDK.

## <a name="overview"></a>Обзор

Приложения UWP, показывающие рекламные баннеры, должны использовать **AdControl** из рекламных библиотек, распространяемых в составе [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp). Этот пакет SDK поддерживает минимальный набор рекламных возможностей, включая возможность обслуживать мультимедиа HTML5 по [спецификации Mobile Rich-media Ad Interface Definitions (MRAID) 1.0](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf), определенной Некоммерческим партнерством содействия развитию интерактивной рекламы (IAB). Эти возможности интересуют многих из наших рекламодателей, поэтому нам нужно, чтобы разработчики приложений использовали один из этих выпусков SDK, это поможет сделать нашу экосистему приложений более привлекательной для рекламодателей и в конечном итоге повысить ваши доходы.

До выпуска этого пакета SDK ранее предоставлялся класс **AdControl** в нескольких старых выпусках рекламного SDK. Предыдущие выпуски пакета SDK для рекламы больше не поддерживаются, так как они не поддерживают минимальные рекламные возможности, описанные выше. С 1 апреля 2017 г. мы больше не предоставляем баннеры для приложений, использующих неподдерживаемый выпуск пакета SDK для рекламы. Если у вас есть приложение, по-прежнему использующее неподдерживаемый выпуск пакета SDK для рекламы, вы увидите следующее поведение:

* В элементах управления **AdControl** в вашем приложении больше не будут отображаться рекламные баннеры, и вы не будете получать доход от рекламы от этих элементов управления.

* Когда элемент управления **AdControl** в вашем приложении запрашивает новую рекламу, создается событие **ErrorOccurred** элемента управление, и свойство **ErrorCode** аргументов события будет иметь значение **NoAdAvailable**.

* Любые группы объявлений, связанные с вашим приложением, будут отключены. Невозможно удалить отключенные группы объявлений из учетной записи Центра разработки. Если вы обновляете свое приложение для использования [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp), игнорируйте эти группы объявлений и создавайте новые.

* Баннеры тоже больше не будут предоставляться для любой группы объявлений, которая используется более чем в одном приложении. Убедитесь, что каждая ваша группа объявлений используется только в одном приложении.

Если у вас есть приложение (уже находящееся в Microsoft Store или на стадии разработки), отображающее баннеры с помощью **AdControl**, и вы не можете точно определить, какой пакет SDK используется вашим приложением, следуйте инструкциям из этой статьи, чтобы выяснить, нужно ли вам обновлять приложение до поддерживаемого пакета SDK. Если у вас возникнут какие-либо проблемы или понадобится помощь, [обратитесь в службу поддержки](http://go.microsoft.com/fwlink/?LinkId=393643).

> [!NOTE]
> Если ваше приложение уже использует [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) (для приложений UWP), вам не требуется вносить изменения в приложение.

## <a name="prerequisites"></a>Что вам понадобится

* Полный исходный код и файлы проекта Visual Studio для приложения, в котором используются элементы управления **AdControl**.
* Пакет AppX для приложения.

> [!NOTE]
> Если у вас больше нет пакета AppX для вашего приложения, однако есть компьютер разработки с версией Visual Studio и рекламным SDK, который использовался для создания приложения, вы можете заново создать пакет AppX в Visual Studio.

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>Часть 1. Определите, нужно ли вам обновлять приложение UWP

Следуйте инструкциям в следующих разделах, чтобы определить, нужно ли вам обновлять ваше приложение.

1. Создайте копию пакета AppX для своего приложения, чтобы не повредить исходный пакет, переименуйте копию так, чтобы у нее было расширение ZIP, и извлеките содержимое файла.

2. Проверьте извлеченное содержимое пакета приложения:
  * Если вы видите файл Microsoft.Advertising.dll, в вашем приложении используется старый SDK, и вам необходимо обновить проект, следуя инструкциям в разделах ниже. Переходите к [части 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).
  * Если вы не видите файла Microsoft.Advertising.dll, в вашем приложении UWP уже используется последний доступный рекламный SDK, и вносить изменения в проект не нужно.


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>Часть 2. Установите последнюю версию SDK

Если в вашем приложении используется старый выпуск SDK, выполните следующие действия, чтобы установить на свой компьютер разработки последнюю версию SDK.

1. Убедитесь, что на вашем компьютере разработки есть Visual Studio 2015 или более поздняя версия.
    > [!NOTE]
    > Если система Visual Studio на компьютере разработки открыта, закройте ее, прежде чем выполнять следующие действия.

1.  Удалите все предыдущие версии Microsoft Advertising SDK и Ad Mediator SDK с компьютера разработки.

2.  Откройте окно **Командная строка** и выполните следующие команды, чтобы удалить все версии рекламных SDK, которые могли быть установлены вместе с Visual Studio, но, возможно, не отображаются в списке установленных программ на компьютере:
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Установите [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp).

## <a name="part-3-update-your-project"></a>Часть 3. Обновите проект

Удалите из проекта все существующие ссылки на рекламные библиотеки Microsoft и следуйте [этим инструкциям](install-the-microsoft-advertising-libraries.md#reference), чтобы добавить необходимые ссылки. Это позволит гарантировать, что в вашем проекте используются правильные библиотеки. Вы можете сохранить существующую разметку и код.

## <a name="part-4-test-and-republish-your-app"></a>Часть 4. Протестируйте свое приложение и заново опубликуйте его

Протестируйте свое приложение, чтобы убедиться, что рекламные баннеры отображаются должным образом.

Если предыдущая версия вашего приложения уже доступна в Магазине, [создайте новую отправку](../publish/app-submissions.md) для обновленного приложения в информационной панели Центра разработки, чтобы опубликовать приложение заново.