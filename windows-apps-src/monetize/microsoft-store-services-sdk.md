---
Description: Пакет Microsoft Store Services SDK предоставляет библиотеки и средства, которые вы можете использовать для добавления в приложения функций, помогающие заработать и привлечь пользователей.
title: Привлечение пользователей с помощью Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: 679dde6802a2c0d27444fbcda040f2ba19039457
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852798"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Привлечение пользователей с помощью Microsoft Store Services SDK

Пакет SDK для служб Microsoft Store Services предоставляет функции, помогающие привлекать клиентов в приложениях универсальная платформа Windows (UWP), таких как отправка целевых уведомлений в приложения и выполнение экспериментов/B в приложениях. Этот пакет SDK является расширением Visual Studio 2015 и более поздних версий Visual Studio.

> [!NOTE]
> Для отображения рекламы в приложениях UWP используйте [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) вместо Microsoft Store Services SDK. Рекламные библиотеки были перемещены из Microsoft Store Services SDK в Microsoft Advertising SDK. Дополнительные сведения см. в статье [Показ рекламы в приложениях](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Сценарии, поддерживаемые Microsoft Store Services SDK

Microsoft Store Services SDK в настоящее время поддерживает следующие сценарии для приложений UWP. Справочную документацию по API см. в [справочнике по API пакета Microsoft Store Services SDK](https://docs.microsoft.com/uwp/api/overview/engagement).

|  Сценарий  |  Описание   |
|------------|----------------|
|  [Выполнение экспериментов в приложении UWP с помощью тестирования A/B](run-app-experiments-with-a-b-testing.md)    |  Проводите A/B-тестирование в своем приложении для универсальной платформы Windows (UWP), чтобы оценить эффективность функций на некоторых пользователях перед выпуском этих функций для всех пользователей. Определив эксперимент в центре партнеров, используйте класс [сторесервицесекспериментвариатион](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) для получения вариантов эксперимента в приложении, используйте эти данные для изменения поведения тестируемой функции, а затем используйте метод [логфорвариатион](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) для отправки событий просмотра и преобразования событий в центр партнеров. Наконец, используйте центр партнеров для просмотра результатов и управления экспериментом.  |
|  [Запуск центра отзывов из приложения UWP](launch-feedback-hub-from-your-app.md)    |  Используйте класс [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) в своем приложении UWP, чтобы направлять пользователей Windows 10 в Центр отзывов, где они смогут сообщать о проблемах, делиться предложениями и голосовать за комментарии других пользователей. Затем вы сможете проанализировать все эти данные в [отчете об отзывах](../publish/feedback-report.md) в Центре партнеров. |
|  [Настройка приложения UWP для получения push-уведомлений центра партнеров](configure-your-app-to-receive-dev-center-notifications.md)    |  Используйте класс [сторесервицесенгажементманажер](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) в приложении UWP для регистрации приложения, чтобы получать целевые push-уведомления, отправляемые клиентам с помощью центра партнеров.  |
|   [Регистрация пользовательских событий в приложении UWP для отчета об использовании в центре партнеров](log-custom-events-for-dev-center.md)   |  Используйте класс [сторесервицескустомевентлогжер](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) в приложении UWP для регистрации пользовательских событий, связанных с вашим приложением в центре партнеров. Затем просмотрите общее число вхождений для пользовательских событий в разделе **настраиваемые события** [отчета об использовании](https://docs.microsoft.com/windows/uwp/publish/usage-report) в центре партнеров.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Предварительные требования

Microsoft Store Services SDK предъявляет следующие требования.

* Visual Studio 2015 или более поздняя версия.
* Инструменты Visual Studio для универсальных приложений для Windows, установленные в вашей версии Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Установка пакета SDK

Существует два варианта установки Microsoft Store Services SDK на компьютер разработки.

* **Установщик MSI.** &nbsp;&nbsp;Можно установить пакет SDK с помощью установщика MSI, доступного [здесь](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK).
* **Пакет NuGet.** &nbsp;&nbsp;SDK можно установить в качестве пакета NuGet.

Корпорация Майкрософт периодически выпускает новые версии пакета Microsoft Store Services SDK для повышения производительности и расширения возможностей. Если у вас уже есть проекты, в которых используется этот пакет SDK, и вы хотите использовать его последнюю версию, скачайте и установите последнюю версию SDK на свой компьютер разработки.

<span id="install-msi" />

### <a name="install-via-msi"></a>Установка с помощью MSI

Установка Microsoft Store Services SDK с помощью установщика MSI.

1.  Закройте все экземпляры Visual Studio.

2. Если вы ранее устанавливали Microsoft Store Engagement and Monetization SDK, Universal Ad Client SDK или расширение Ad Mediator, теперь необходимо удалить эти версии пакетов SDK. Также можно открыть окно **командной строки** и выполнить эти команды для удаления всех более ранних версий пакетов рекламных SDK, которые могли быть установлены вместе с Visual Studio, но, возможно, не отображаются в списке установленных программ на компьютере:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Скачайте и установите пакет [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Установка может занять несколько минут. Обязательно дождитесь завершения процесса.

4.  Перезапустите Visual Studio.

5.  Если у вас есть существующий проект, который ссылается на библиотеки из какой-либо более ранней версии пакетов Microsoft Store Services SDK, Microsoft Advertising SDK, Universal Ad Client SDK или Microsoft Store Engagement and Monetization SDK, рекомендуется открыть этот проект в Visual Studio, очистить и заново собрать проект (в **Обозревателе решений** щелкните правой кнопкой мыши по узлу проекта и выберите пункт **Очистить**, а затем снова щелкните правой кнопкой мыши по узлу проекта и выберите **Пересобрать**).

  В противном случае, если вы используете пакет SDK в первый раз в своем проекте, теперь вы готовы [добавить ссылку на сборку в свой проект](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Установка с помощью NuGet

Установка библиотек Microsoft Store Services SDK с помощью NuGet.

1.  Закройте все экземпляры Visual Studio.

2. Если вы ранее устанавливали Microsoft Store Engagement and Monetization SDK, Universal Ad Client SDK или расширение Ad Mediator, теперь необходимо удалить эти версии пакетов SDK. Также можно открыть окно **командной строки** и выполнить эти команды для удаления всех более ранних версий пакетов рекламных SDK, которые могли быть установлены вместе с Visual Studio, но, возможно, не отображаются в списке установленных программ на компьютере:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Запустите Visual Studio и откройте проект, в котором вы хотите использовать Microsoft Store Services SDK.
    > [!NOTE]
    > Если ваш проект уже содержит ссылки на библиотеки из предыдущей установки пакета SDK с помощью MSI, удалите эти ссылки из проекта. Рядом с этими ссылками будут расположены предупреждающие значки, поскольку библиотеки, на которые они ссылаются, были удалены ранее.

4. В Visual Studio щелкните **Проект** и выберите параметр **Управление пакетами NuGet**.

5. В поле поиска введите **Microsoft.Services.Store.Engagement** и установите пакет Microsoft.Services.Store.Engagement. Когда установка пакета завершится, сохраните решение.
    > [!NOTE]
    > Если в окне **Вывод** содержится ошибка *Install-Package*, указывающая, что заданный путь слишком длинный, вам может потребоваться настроить NuGet, чтобы извлечь пакеты в альтернативное расположение с более коротким путем, чем расположение по умолчанию. Для этого добавьте значение `repositoryPath` в файл nuget.config на компьютере и задайте ему более короткий путь к папке, куда можно извлечь пакеты NuGet. Дополнительные сведения см. в [этой статье](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) в документации NuGet. Кроме того можно попробовать переместить проект Visual Studio в другую папку с более коротким путем. Проблема также может быть вызвана тем, что длина пути к глобальным пакетам слишком велика. В этом случае добавьте `globalPackagesFolder` значение в файл NuGet. config.

6. Закройте решение Visual Studio, содержащее проект, и снова откройте его.

7.  Если проект уже содержит ссылки на библиотеки из более ранней версии Microsoft Store Services SDK, которая была установлена с помощью NuGet, и вы обновили свой проект до более нового выпуска SDK, рекомендуется очистить и пересобрать проект (в **Обозревателе решений** щелкните правой кнопкой мыши по узлу проекта и выберите пункт **Очистить**, затем снова щелкните правой кнопкой мыши по узлу проекта и выберите **Пересобрать**).

  В противном случае, если вы используете пакет SDK в первый раз в своем проекте, теперь вы готовы [добавить ссылку на сборку в свой проект](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Добавьте ссылку на сборку в проект

После установки Microsoft Store Services SDK с помощью установщика MSI или NuGet следуйте этим инструкциям для добавления ссылок на сборку SDK в свой проект UWP.

1. Откройте свой проект в Visual Studio.
    > [!NOTE]
    > Если ваш проект является приложением JavaScript и направлен на работу на **Любом ЦП**, обновите его, чтобы он использовал результаты сборки, предназначенные для определенной архитектуры (например, **x86**).

2. В **обозревателе решений**щелкните правой кнопкой мыши пункт **Ссылки** и выберите **Добавить ссылку...** .

3. В **Диспетчере ссылок** разверните **универсальные для Windows**, нажмите **Расширения** и затем установите флажок рядом с пунктом **Microsoft Engagement Framework**. Это позволяет использовать API в пространстве имен [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

3. Нажмите кнопку **ОК**.

> [!NOTE]
> Если вы установили библиотеки пакета SDK через NuGet, проект будет содержать ссылку на **Microsoft.Services.Store.Engagement**. Ссылка на **Microsoft.Services.Store.Engagement** представляет пакет NuGet (а не библиотеки в нем), и вы можете проигнорировать ее.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Понимание пакетов платформы в SDK

Библиотека Microsoft.Services.Store.Engagement.dll в Microsoft Store Services SDK настроена как *пакет платформы*. Эта библиотека содержит API-интерфейсы в пространстве имен [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

Поскольку библиотека представляет собой пакет платформы, это означает, что после установки пользователем версии вашего приложения, которое применяет эту библиотеку, библиотека будет автоматически обновляться на устройстве пользователя через Центр обновления Windows, когда мы опубликуем новую версию библиотеки с исправлениями и улучшенной производительностью. Это позволяет гарантировать, что ваши клиенты всегда будут иметь последнюю доступную версию библиотеки на своих устройствах.

Если мы выпустим новую версию SDK с новыми API или функциями в этой библиотеке, вам придется установить последнюю версию пакета SDK, чтобы использовать их. В этом случае вам также понадобится опубликовать обновленное приложение в Магазине.

## <a name="related-topics"></a>Связанные разделы

* [Справочник по API SDK для служб Microsoft Store Services](https://docs.microsoft.com/uwp/api/overview/engagement)
* [Проводите эксперименты с использованием A/B-тестирования](run-app-experiments-with-a-b-testing.md)
* [Запуск Центра отзывов из приложения](launch-feedback-hub-from-your-app.md)
* [Настройка приложения для получения push-уведомлений Центра партнеров](configure-your-app-to-receive-dev-center-notifications.md)
* [Ведение журнала пользовательских событий для Центра партнеров](log-custom-events-for-dev-center.md)
