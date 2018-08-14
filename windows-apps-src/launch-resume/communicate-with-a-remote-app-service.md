---
author: PatrickFarley
title: Обмен данными с удаленной службой приложения
description: Обмен сообщениями со службой приложения на удаленном устройстве с помощью Project Rome.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a40b48df9f9d8fe740795d6af0d71ba70a34c469
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2018
ms.locfileid: "1572771"
---
# <a name="communicate-with-a-remote-app-service"></a>Обмен данными с удаленной службой приложения

Помимо запуска приложения на удаленном устройстве с помощью универсального кода ресурса (URI), можно также запускать на удаленных устройствах *службы приложений* и обмениваться данными с этими службами. Любое устройство на основе Windows можно использовать как клиентское или главное устройство. Это дает практически неограниченное количество способов взаимодействия с подключенными устройствами без необходимости переноса приложения на передний план.

## <a name="set-up-the-app-service-on-the-host-device"></a>Настройка службы приложений на главном устройстве
Чтобы запустить службу приложений на удаленном устройстве, необходимо, чтобы на нем уже был установлен поставщик этой службы приложений. В этом руководстве используется версия [образца службы приложений "генератор случайных чисел"](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices) на CSharp, которая доступна в [репозитории примеров универсальных приложений Windows](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Инструкции по созданию собственной службы приложений см. в разделе [Создание и использование службы приложений](how-to-create-and-consume-an-app-service.md).

Используется ли готовая служба приложения или создается собственная служба, необходимо внести несколько изменений, чтобы сделать службу совместимой с удаленными системами. В Visual Studio перейдите в проект поставщика службы приложения (в примере он называется AppServicesProvider) и выберите соответствующий файл _Package.appxmanifest_. Щелкните правой кнопкой мыши и выберите **Перейти к коду** для просмотра всего содержимого файла. Найдите элемент **Расширение**, определяющий проект как службу приложения и задающий имя родительского проекта.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Добавьте атрибут **SupportsRemoteSystems**, если его еще нет:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Выполните сборку проекта поставщика услуг приложения и разверните проект на главных устройствах.

## <a name="target-the-app-service-from-the-client-device"></a>Выбор службы приложений с клиентского устройства
На устройстве, с которого должна вызываться удаленная служба приложений, необходимо приложение с поддержкой функциональности удаленных систем. Эту функциональность можно добавить в приложение, которое обеспечивает работу службы приложений на главном устройстве (в этом случае на оба устройства устанавливается одно и то же приложение), или реализовать в совершенно другом приложении.

Чтобы код из этого раздела работал в приведенном виде, необходимы следующие операторы **using**:

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Сначала необходимо создать экземпляр объекта [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection), точно так же, как и при локальном вызове службы приложения. Этот процесс подробно описан в разделе [Создание и использование службы приложений](how-to-create-and-consume-an-app-service.md). В этом примере в качестве целевой службы приложений используется служба генератора случайных чисел.

> [!NOTE]
> Предполагается, что объект [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) уже каким-то образом получен в коде, который будет вызывать следующий метод. Инструкции относительно того, как это сделать, см. в разделе [Запуск удаленного приложения](launch-a-remote-app.md).

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Далее создается объект [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) для требуемого удаленного устройства. Затем он используется для открытия **AppServiceConnection** с этим устройством. Обратите внимание, что для краткости в приведенном ниже примере обработка ошибок и отчетность значительно упрощены.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

На этом этапе у вас должно иметься открытое подключение к службе приложений на удаленном компьютере.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Обмен специфичными для службы сообщениями по удаленному подключению

Отсюда можно отправлять в службу и получать из службы сообщения в форме объектов [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset) (дополнительные сведения см. в разделе [Создание и использование службы приложения](how-to-create-and-consume-an-app-service.md)). Служба генератора случайных чисел принимает 2 целых числа с ключами `"minvalue"` и `"maxvalue"` в качестве входных данных, случайным образом выбирает целое число внутри диапазона и возвращает это число в вызывающий процесс с ключом `"Result"`.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Теперь вы подключены к службе приложений на целевом главном устройстве, выполнили операцию на этом устройстве и получили данные на клиентском устройстве в ответном сообщении.

## <a name="related-topics"></a>Связанные разделы

[Обзор подключенных приложений и устройств (Project Rome)](connected-apps-and-devices.md)  
[Запуск удаленного приложения](launch-a-remote-app.md)  
[Создание и использование службы приложений](how-to-create-and-consume-an-app-service.md)  
[Справочник по API удаленных систем](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Пример удаленных систем](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)