---
title: Обнаружение удаленных устройств
description: Узнайте, как обнаруживать удаленные устройства из приложения с помощью платформы Project Rome.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: устройства Windows 10, универсальной платформы Windows, подключенных удаленных систем, Рим, рим проекта
ms.localizationpriority: medium
ms.openlocfilehash: 83230e17548f022767dcdde6b17fa9a93d6d9304
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320967"
---
# <a name="discover-remote-devices"></a>Обнаружение удаленных устройств
Ваше приложение может использовать беспроводную сеть, Bluetooth и облачное подключение для обнаружения устройств с Windows, вход с которых выполнен с той же учетной записью Майкрософт, что и у обнаруживающего устройства. Обнаружение удаленных устройств возможно без установки на них какого-либо специального программного обеспечения.

> [!NOTE]
> В этом руководстве предполагается, что вы уже получили доступ к функции удаленных систем, выполнив шаги раздела [Запуск удаленного приложения](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Фильтрация набора устройств, доступных для обнаружения
Набор доступных для обнаружения устройств можно ограничить, используя [**RemoteSystemWatcher**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystemWatcher) с фильтрами. Фильтры могут определять тип обнаружения (подключение близкого радиуса действия или локальная сеть или облачное подключение), типа устройства (компьютер, мобильное устройство, Xbox, Hub и Holographic) и состояние доступности (состояние доступности устройства для использования функций удаленной системы).

Объекты фильтра должны быть созданы до или во время инициализации объекта **RemoteSystemWatcher**, поскольку они передаются как параметры в его конструктор. Следующий код создает фильтры каждого доступного типа, затем добавляет их в список.

> [!NOTE]
> В коде из этих примеров требуется, чтобы в вашем файле имелся оператор `using Windows.System.RemoteSystems`.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> Значение фильтра "proximal" не гарантирует степень физической близости. Для сценариев, которым требуется надежная физическая близость, используйте значение [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) в своем фильтре. В настоящее время этот фильтр допускает только устройства, которые обнаружены с помощью Bluetooth. Так как новые механизмы обнаружения и протоколы, которые гарантируют физическую близость, поддерживаются, они также будут включены здесь.  
Имеется также свойство в [ **RemoteSystem** ](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem) класс, который указывает, располагается ли обнаружено устройство, на самом деле внутри физического местонахождения: [**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Если требуется обнаруживать устройства по локальной сети (определяется фильтром выбора типа обнаружения), в вашей сети необходимо использовать профиль «частная» или «доменная». Устройство не будет обнаруживать другие устройства в «общедоступной» сети.

После создания списка объектов [**IRemoteSystemFilter**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.IRemoteSystemFilter) его можно передать в конструктор объекта **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

При вызове метода [**Start**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemwatcher.start) этого наблюдателя он инициирует событие [**RemoteSystemAdded**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemwatcher.remotesystemadded) только в случае обнаружения устройства, которое соответствует всем указанным ниже критериям.
* Оно может быть обнаружено с помощью подключения близкого радиуса действия
* Это компьютер или телефон
* Оно классифицируется как доступное

С этого момента процедура обработки событий, извлечения объектов [**RemoteSystem**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem) и подключения к удаленным устройствам полностью совпадает с процедурой из раздела [Запуск удаленного приложения](launch-a-remote-app.md). Если коротко, объекты **RemoteSystem** хранятся в виде свойств объектов [**RemoteSystemAddedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), которые передаются с каждым событием **RemoteSystemAdded**.

## <a name="discover-devices-by-address-input"></a>Обнаружение устройств путем ввода адреса
Некоторые устройства могут быть не связаны с пользователем или могут не обнаруживаться сканированием, но с ними все равно можно установить связь, если обнаруживающее приложение использует прямой адрес. Класс [**HostName**](https://docs.microsoft.com/uwp/api/windows.networking.hostname) используется для представления адреса удаленного устройства. Он часто хранится в виде IP-адреса, но допускается также ряд других форматов (см. раздел [**Конструктор HostName**](https://docs.microsoft.com/uwp/api/windows.networking.hostname.-ctor)).

Объект **RemoteSystem** извлекается, если указан действительный объект **HostName**. Если сведения об адресе неправильные, возвращается ссылка на объект `null`.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Запрос возможности на удаленной системе

Несмотря на то что возможности запрашивания устройств отделены от фильтрации обнаружения, они могут быть важной частью процесса обнаружения. С помощью метода [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) вы можете запрашивать обнаруженные удаленные системы для поддержки некоторых возможностей, например, подключения удаленного сеанса или совместного использования пространственных (голографических) объектов. Список запрашиваемых возможностей см. в классе [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities).

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Обнаружение между пользователями

Разработчики могут указать обнаружение _всех_ устройств по близости к клиентскому устройству, а не только к устройствам, зарегистрированным на одного и того же пользователя. Это реализуется через специальный **IRemoteSystemFilter**, [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter). Это реализуется как и другие типы фильтра:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Значение [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) параметра **Anonymous** будет разрешать обнаружение всех ближайших устройств, даже устройств от ненадежных пользователей.
* Значение **SameUser** фильтрует обнаружение только до устройств, зарегистрированных на одного и того же пользователя как клиентского устройства. Такая реакция на события используется по умолчанию.

### <a name="checking-the-cross-user-sharing-settings"></a>Проверка параметров общего доступа между пользователями

Кроме указанного выше фильтра, который указывается в приложении обнаружения, само клиентское устройство необходимо настроить на разрешение совместного использования с устройств, зарегистрированных с другими пользователями. Это параметр системы, который можно запросить с помощью статического метода в классе **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Чтобы изменить этот параметр, пользователь должен открыть приложение **Параметры**. В меню **Система** > **Общие возможности** > **Общий доступ между устройствами** имеется раскрывающийся список, где пользователь может указать, каким устройствам система может предоставлять общий доступ.

![страница параметров общих возможностей](images/shared-experiences-settings.png)

## <a name="related-topics"></a>См. также
* [Подключенные приложения и устройства (рим проекта)](connected-apps-and-devices.md)
* [Запуск удаленного приложения](launch-a-remote-app.md)
* [Удаленный Справочник по API системы](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)
* [Пример удаленных систем](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
