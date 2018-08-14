---
author: normesta
Description: Distribute a packaged desktop app (Desktop Bridge)
Search.Product: eADQiWindows 10XVcnh
title: Опубликуйте ваше упакованное классическое приложение в Store Windows или загрузите его в неопубликованном виде на одно или несколько устройств.
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.localizationpriority: medium
ms.openlocfilehash: 682d7dfcef1ea8037b113499362f0664c388d987
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989628"
---
# <a name="distribute-a-packaged-desktop-app-desktop-bridge"></a>Распространение упакованного классического приложения (мост для классических приложений)

Опубликуйте ваше упакованное классическое приложение в Store Windows или загрузите его в неопубликованном виде на одно или несколько устройств.  

> [!NOTE]
> У вас есть план, как перевести пользователей на упакованное приложение? Перед распространением ознакомьтесь с разделом [Перевод пользователей на упакованное приложение](#transition-users) данного руководства, чтобы получить несколько идей.

## <a name="distribute-your-app-by-publishing-it-to-the-microsoft-store"></a>Распространение приложения за счет его публикации в Microsoft Store

[Microsoft Store](https://www.microsoft.com/store/apps)— это самый удобный для пользователей способ получить ваше приложение.

Опубликуйте ваше приложение в Store, чтобы охватить самую широкую аудиторию. Кроме того, корпоративные клиенты могут приобрести ваше приложение через [Microsoft Store для бизнеса](https://www.microsoft.com/business-store), чтобы распространять его внутри своих организаций.

Если вы планируете публиковать в Microsoft Store, вам будет предложено ответить на несколько дополнительных вопросов в ходе процесса отправки. Это означает, что манифест пакета объявляет возможность с ограниченным доступом под названием **runFullTrust**, и мы должны утвердить использование этой возможности приложением. Подробнее об этом сценарии требовании см. здесь [Ограниченные возможности](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#restricted-capabilities.html).

Подписывать приложение перед отправкой в Store не требуется.

>[!IMPORTANT]
> Если вы планируете опубликовать приложение в Microsoft Store, проверьте исправность его работы на устройствах под управлением Windows 10 S. Это требование Store. См. статью [Тестирование приложения для Windows на Windows 10 S](desktop-to-uwp-test-windows-s.md).

<a id="side-load" />

## <a name="distribute-your-app-without-placing-it-onto-the-microsoft-store"></a>Распространение приложения без его размещения в Microsoft Store

Если вы не хотите использовать Store, можно распространять приложения на одно или несколько устройств вручную.

Это имеет смысл, если вам необходим более жесткий контроль над процессом распространения, либо если вы не хотите участвовать в процессе сертификации Microsoft Store.

Чтобы распространить приложение на другие устройства, не размещая его в Store, необходимо получить сертификат, подписать приложение с помощью этого сертификата, а затем загрузить неопубликованное приложение на эти устройства.

Вы можете [создать сертификат](../packaging/create-certificate-package-signing.md) или получить его от известного поставщика, такого как [Verisign](https://www.verisign.com/).

Если ваше приложение будет распространяться на устройства под управлением Windows10S, оно должно быть подписано Microsoft Store, поэтому вам придется отправить его в Store перед распространением.

Если вы создаете сертификат, его необходимо установить в хранилище сертификатов **Доверенные корневые** или **Доверенные лица** на каждом устройстве, которое запускает ваше приложение. Если сертификат получен от известного поставщика, в других системах необходимо установить только ваше приложение.  

> [!IMPORTANT]
> Убедитесь, что имя издателя на сертификате совпадает с именем издателя вашего приложения.

Чтобы подписать приложение с помощью этого сертификата, см. статью [Подписание пакета приложения с помощью SignTool](../packaging/sign-app-package-using-signtool.md).

Для загрузки неопубликованного приложения на другие устройства см. статью [Загрузка неопубликованных бизнес-приложений в Windows10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10).

**Видео**

|Публикация приложения в Microsoft Store |Распространение корпоративного приложения  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Перевод пользователей на ваше упакованное приложение

Перед распространением приложения рекомендуется добавить несколько расширений в ваш манифест пакета, чтобы помочь пользователям привыкнуть использовать ваше упакованное приложение. Вот пара советов, что можно сделать.

* Определите существующие плитки начального экрана и кнопки на панели задач в качестве указателей на упакованное приложение.
* Создайте ассоциацию вашего упакованного приложения с набором типов файлов.
* Обязуйте ваше упакованное приложение открывать определенные типы файлов по умолчанию.

Полный перечень расширений и рекомендации по их использованию см. в разделе [Переход пользователей на ваше приложение](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Кроме того, в упакованное приложение рекомендуется добавить код, который выполняет следующие задачи:

* Переносит пользовательские данные, связанные с классическим приложением, в соответствующие расположения папок упакованного приложения.
* Позволяет пользователям удалить классическую версию вашего приложения.

Рассмотрим каждую из этих задач. Начнем с переноса пользовательских данных.

### <a name="migrate-user-data"></a>Перенос пользовательских данных

Если вы собираетесь добавить код, который переносит пользовательские данные, то лучше выполнить его только при первом запуске приложения. Прежде чем переносить данные, отобразите пользователю диалоговое окно, в котором сообщите о том, что происходит, почему этим рекомендуется воспользоваться и что случится с их имеющимися данными.

Вот пример того, как это можно выполнить в упакованном приложении на основе .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Удаление классической версии вашего приложения

Желательно не удалять классическое приложение, предварительно не запросив разрешение у пользователя. Отобразите диалоговое окно с запросом соответствующего разрешения. Пользователи могут решить не удалять классическую версию. В этом случае вам необходимо решить, блокировать использование классического приложения или поддерживать параллельное использование обоих.

Вот пример того, как это можно выполнить в упакованном приложении на основе .NET.

Полный контекст этого фрагмента см. в файле **MainWindow.cs** примера [Средство просмотра изображений WPF с переходом, переносом и удалением](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop App is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the app here.
            }
        }
    }

}
```

### <a name="video"></a>Видео

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Дальнейшие действия

**Поиск ответов на вопросы**

Есть вопросы? Задайте их на Stack Overflow. Наша команда следит за этими [тегами](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопросы [здесь](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Если при публикации вашего приложения в Store возникнут проблемы, вы найдете полезные советы в этой [записи в блоге](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/).

**Оставьте отзыв или предложите новые возможности для реализации**

См. раздел [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)