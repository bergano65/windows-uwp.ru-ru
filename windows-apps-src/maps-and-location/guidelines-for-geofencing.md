---
Description: Следуйте этим рекомендациям по функции создания геозон в своем приложении.
title: Руководство по приложениям, использующим функцию геозон
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, карта, расположение, геозоны
ms.localizationpriority: medium
ms.openlocfilehash: 6b1f328d45e626e1c7eb633165aad3671f1645e5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260384"
---
# <a name="guidelines-for-geofencing-apps"></a>Руководство по приложениям, использующим функцию геозон




**Важные API**

-   [**Класс геозоны (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)
-   [**Класс геоуказателя (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)

Следуйте этим рекомендациям по [**созданию и настройке геозон**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing) в своем приложении.

## <a name="recommendations"></a>Рекомендации


-   Если при возникновении события [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) вашему приложению понадобится доступ к Интернету, возможно, вы захотите проверить наличие такого доступа перед созданием геозоны.
    -   Если у приложения на данный момент нет доступа к Интернету, вы можете предложить пользователю подключиться к Интернету до настройки геозоны.
    -   Если возможность подключения к Интернету отсутствует, проследите за тем, чтобы функция геозон не расходовала электроэнергию на проверки расположения.
-   Убедитесь в правильности уведомлений функции геозон. Для этого проверьте метку времени и текущее расположение, когда событие геозоны показывает изменение состояния [**Entered**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) или **Exited**. Дополнительную информацию см. далее в разделе **Проверка метки времени и текущего расположения**.
Дополнительные сведения см. далее в разделе (#меткавремени).
-   Создайте исключения для обработки ситуаций, когда устройство не может получить доступ к сведениям о расположении, и при необходимости настройте уведомления для пользователя. Сведения о расположении могут оказаться недоступными при отключенных разрешениях, отсутствии радиомодуля GPS в устройстве, блокировке GPS-сигнала или слабом сигнале Wi-Fi.
-   Обычно нет необходимости прослушивать события геозон одновременно и в фоновом режиме, и на переднем плане. Если вашему приложению все же необходимо прослушивать события геозон в обоих режимах:

    -   Вызовите метод [**ReadReports**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports), чтобы узнать, произошло ли событие геозоны.
    -   Отмените регистрацию прослушивателей геозон на переднем плане, если ваше приложение невидимо для пользователя, и повторно зарегистрируйте его, когда приложение снова станет видимым.

    Примеры кода и дополнительную информацию можно найти в разделе [Прослушиватели в фоновом режиме и на переднем плане](#background-and-foreground-listeners).

-   Не используйте более 1000 геозон на приложение. Фактически система поддерживает несколько тысяч геозон на приложение, но вы можете обеспечить хорошую производительность приложения, снижая потребление памяти при использовании не более 1000 геозон.
-   Не создавайте геозоны с размером менее 50 метров. Если приложению требуются небольшие геозоны, рекомендуйте пользователям использовать такое приложение на устройстве с радиомодулем GPS, чтобы обеспечить наилучшую производительность.

## <a name="additional-usage-guidance"></a>Дополнительные рекомендации по использованию

### <a name="checking-the-time-stamp-and-current-location"></a>Проверка метки времени и текущего расположения

Когда событие указывает на изменение состояния [**Entered**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) или **Exited**, следует проверять и метку времени события, и текущее расположение. На фактическую обработку события пользователем могут влиять различные факторы: например, у системы не хватает ресурсов на запуск фоновой задачи, пользователь не заметил уведомления или устройство перешло в режим ожидания (в Windows). К примеру, может произойти такая последовательность событий.

-   Ваше приложение создает геозону и отслеживает события входа и выхода из нее.
-   Пользователь перемещает устройство внутрь геозоны, в результате чего срабатывает событие входа.
-   Приложение отправляет пользователю уведомление о том, что он теперь находится в геозоне.
-   Пользователь в этот момент занят и замечает уведомление только 10 минут спустя.
-   За эти 10 минут он снова выходит за пределы геозоны.

По метке времени можно понять, что событие произошло в прошлом. По текущему расположению можно понять, что пользователь в данный момент находится за пределами геозоны. В зависимости от функционала вашего приложения вы, возможно, захотите отфильтровать это событие.

### <a name="background-and-foreground-listeners"></a>Прослушиватели в фоновом режиме и на переднем плане

Обычно вашему приложению не нужно прослушивать события [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) одновременно и в фоновом режиме, и на переднем плане. Но в случае, когда все же необходимо и то и другое, лучший способ решить проблему — обрабатывать уведомления в фоновом режиме. Если настроить прослушиватели геозон на переднем плане и в фоновом режиме, невозможно гарантировать, что какой-то из них сработает первым, поэтому необходимо вызывать метод [**ReadReports**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports), чтобы выяснить, произошло ли событие.

Если вы настроили прослушиватели геозон одновременно на переднем плане и в фоновом режиме, регистрация прослушивателей геозон на переднем плане должна отменяться, когда ваше приложение невидимо для пользователя, и выполняться повторно, когда оно становится видимым. Вот образец кода, который регистрирует события видимости.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

При изменении видимости можно включать или отключать обработчики событий на переднем плане, как показано здесь.

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>Определение размера геозон

Хотя GPS может предоставить наиболее точные сведения о местонахождении, для определения текущего расположения пользователя функция геозон может также использовать Wi-Fi или другие датчики местонахождения. Тем не менее использование других методов может повлиять на размер создаваемых геозон. Если степень точности невысока, бесполезно создавать маленькие геозоны. В общем случае рекомендуется не создавать геозоны с радиусом менее 50 метров. Кроме того, фоновые задачи геозон выполняются в Windows только периодически. Если вы используете маленькие геозоны, может случиться так, что вы вообще пропустите событие [**Enter**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) или **Exit**.

Если приложению требуются небольшие геозоны, рекомендуйте пользователям использовать такое приложение на устройстве с радиомодулем GPS, чтобы обеспечить наилучшую производительность.

## <a name="related-topics"></a>См. также


* [Настройка геозоны](https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)
* [Получение сведений о текущем расположении](https://docs.microsoft.com/windows/uwp/maps-and-location/get-location)
* [Пример расположения UWP (географическое расположение)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
