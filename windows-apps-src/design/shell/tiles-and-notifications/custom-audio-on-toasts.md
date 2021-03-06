---
Description: Сведения об использовании пользовательских аудио на всплывающие уведомления.
title: Настраиваемый звук всплывающих уведомлениях
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10 uwp, всплывающее уведомление, настраиваемый звук, уведомление, аудио, звук
ms.localizationpriority: medium
ms.openlocfilehash: 982340901d13f17945c1e7ffa11099f52732f619
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644069"
---
# <a name="custom-audio-on-toasts"></a>Настраиваемый звук всплывающих уведомлениях

Всплывающие уведомления могут использовать настраиваемый звук, что позволяет приложению воспроизводить уникальные звуковые эффекты. Например, приложение для обмена сообщениями может собственные звуки сообщений для своих всплывающих уведомлений, чтобы пользователи могли сразу понять, что они получили уведомление от вашего приложения.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Установка пакета набора средств сообщества UWP из NuGet

Для создания уведомлений в коде мы настоятельно рекомендуем использовать библиотеку уведомлений набора средств сообщества UWP, которая предоставляет объектную модель для XML-содержимого уведомлений. Вы можете вручную создать XML-содержимое уведомления, но это сложно и может привести к ошибкам. Библиотеку уведомлений в наборе средств сообщества UWP создала и поддерживает команда, которая отвечает за уведомления в корпорации Майкрософт.

Установите [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) из NuGet (в этой документации мы используем версию 1.0.0).


## <a name="add-namespace-declarations"></a>Добавление объявлений пространств имен

`Windows.UI.Notifications` включает в себя плиток и всплывающих API. `Microsoft.Toolkit.Uwp.Notifications` включает библиотеку уведомления.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Создание уведомления

Содержимое всплывающего уведомления включает в себя текст и изображения, а также кнопки и входные данные. Полный фрагмент код см. в разделе [Отправка локального всплывающего уведомления](send-local-toast.md).

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Добавление настраиваемых аудиозаписи

ОС Windows Mobile всегда поддерживала настраиваемый звук для всплывающих уведомлений. Однако в ОС для настольных компьютеров поддержка настраиваемых звуков была реализована только в версии 1511 (сборка 10586). При отправке всплывающего уведомления, содержащего настраиваемый звук, на настольный компьютер с версией ОС до 1511, звук всплывающего уведомления не воспроизводится. Поэтому для настольных компьютеров с ОС версией до 1511 вам не следует включать настраиваемый звук для всплывающего уведомления, чтобы уведомление использовало по крайней мере звуковой сигнал по умолчанию.

**Известная проблема**: При использовании версии 1511 рабочего стола, аудио пользовательского всплывающего работает только если приложение устанавливается с помощью Store. Это означает, что вы не сможете локально протестировать настраиваемый звук до отправки приложения в Store, но звук будет работать после установки из Store. Мы исправили это в юбилейном обновлении, чтобы настраиваемый звук из локально развернутого приложения работал правильно.

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Поддерживаемые типы звуковых файлов...

- AAC
- .flac
- M4A
- MP3
- WAV
- WMA


## <a name="send-the-notification"></a>Отправка уведомления

После завершения содержимое всплывающего уведомления вы легко можете его отправить.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Статьи по теме

- [Полный образец кода на GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Отправка локального всплывающее уведомление](send-local-toast.md)
- [Содержимое документации всплывающее уведомление](adaptive-interactive-toasts.md)