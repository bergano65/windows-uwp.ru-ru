---
author: normesta
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Упаковка приложения вручную (мост для классических приложений)
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.openlocfilehash: 7a9c413b8f0b79f9e6a331145d086e3e563bd989
ms.sourcegitcommit: 6382f751f09e2f4aa8406c1ceeb25b5189e23da3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "2410841"
---
# <a name="package-an-app-manually-desktop-bridge"></a>Упаковка приложения вручную (мост для классических приложений)

В этой статье описывается, как упаковать приложение без использования таких средств, как Visual Studio или Desktop App Converter (DAC).

Чтобы упаковать приложение вручную, создайте файл манифеста пакета, а затем запустите программу командной строки для создания пакета приложения для Windows.

Используйте ручную упаковку, если вы устанавливаете приложение с помощью команды xcopy либо если вам известно, какие изменения вносит в систему ваш установщик приложения, и вы хотите более детально управлять этим процессом.

Если вы не знаете точно, какие изменения вносит в систему ваш установщик, или если предпочитаете использовать автоматические средства для создания пакета манифеста, попробуйте воспользоваться любым из [этих](desktop-to-uwp-root.md#convert) параметров.

>[!IMPORTANT]
>Мост для классических приложений впервые появился в Windows 10 версии 1607 и может использоваться только в проектах, предназначенных для юбилейного обновления Windows 10 (10.0; сборка 14393) или более поздней версии в Visual Studio.

## <a name="first-prepare-your-application"></a>Сначала подготовьте свое приложение

Не забудьте изучить это руководство, прежде чем начинать создание пакета приложения: [Подготовка к упаковке приложения (мост для классических приложений)](desktop-to-uwp-prepare.md).

## <a name="create-a-package-manifest"></a>Создание пакета манифеста

Создайте файл, назовите его **appxmanifest.xml**, а затем добавьте в него XML-код, представленный ниже.

Это базовый шаблон, который содержит элементы и атрибуты, требуемые для вашего пакета. Мы добавим для них значения в следующем разделе.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Заполнение элементов уровня пакета для файла

Заполните этот шаблон сведениями о вашем пакете.

### <a name="identity-information"></a>Сведения об удостоверении

Вот пример элемента **Identity** с замещающим текстом для атрибутов. Вы можете задать атрибуту ``ProcessorArchitecture`` значение ``x64`` или ``x86``.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Если вы зарезервировали имя приложения в Store Windows, вы можете получить значения атрибутов Name (Имя) и Publisher (Издатель) с помощью информационной панели Центра разработки для Windows. Если планируется, что приложение будет загружено на другие системы в неопубликованном виде, вы можете указать для этих атрибутов собственные имена при условии, что имя издателя совпадает с именем, указанным в сертификате, который используется для подписания приложения.

### <a name="properties"></a>Свойства

Элемент [Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) имеет три обязательных дочерних элемента. Вот пример узла **Properties** с замещающим текстом для элементов. **DisplayName**— это имя приложения, которое вы резервируете в Store для отправляемых в этот Store приложений.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Ресурсы

Вот пример узла [Resources](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources).

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Зависимости

Для классических приложений, которые вы упаковываете с помощью моста для классических приложений, всегда устанавливайте для атрибута ``Name`` значение ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Возможности
Для классических приложений, которые вы упаковываете с помощью моста для классических приложений, потребуется добавить возможность ``runFullTrust``.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Заполните элементы уровня приложения

Заполните этот шаблон сведениями о вашем приложении.

### <a name="application-element"></a>Элемент приложения

Для классических приложений, которые вы упаковываете с помощью моста для классических приложений, атрибут ``EntryPoint`` элемента Application всегда имеет значение ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Визуальные элементы

Вот пример узла [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>Добавление специальных ресурсов без покрытия (необязательно)

Специальные ресурсы предназначены для значков и плиток, которые отображаются на панели задач Windows, в представлении задач, в окне, вызываемом сочетанием клавиш ALT+TAB, в Snap Assist и в правом нижнем углу плиток начального экрана. Подробнее об этом см. [здесь](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets#target-based-assets).

1. Получите правильные изображения 44x44 и скопируйте их в папку с изображениями (например, "Ресурсы").

2. Создайте копию каждого изображения 44x44 в той же папке и добавьте **.targetsize-44_altform-unplated** к имени файла. Вам потребуются две копии каждого значка, каждый из которых должен быть назван особым образом. Например, после завершения процесса папка ресурсов может содержать файлы **MYAPP_44x44.png** и **MYAPP_44x44.targetsize 44_altform unplated.png**.

   > [!NOTE]
   > В этом примере значок **MYAPP_44x44.png**— это значок, который вы укажете в атрибуте логотипа ``Square44x44Logo`` вашего пакета приложения для Windows.

3.  В пакете приложения для Windows задайте ``BackgroundColor`` для каждого значка, который должен быть прозрачным.

4. Перейдите к следующему подразделу для создания нового файла индекса ресурсов пакета.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>Создание файла индекса ресурсов пакета (PRI)

Если необходимо создать специальные ресурсы, как описано в приведенном выше разделе, или изменить какие-либо визуальные ресурсы приложения после создания пакета, необходимо создать новый файл PRI.

1.  Откройте **командную строку разработчика для VS 2017**.

    ![командная строка разработчика](images/desktop-to-uwp/developer-command-prompt.png)

2.  Смените каталог на корневую папку пакета и создайте файл priconfig.xml с помощью команды ``makepri createconfig /cf priconfig.xml /dq en-US``.

5.  Создайте файлы resources.pri с помощью команды ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Например, команда для вашего приложения может выглядеть так: ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Запакуйте приложение для Windows, руководствуясь инструкциями в следующем шаге.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Создание пакета приложений для Windows

Используйте **MakeAppx.exe**, чтобы создать пакет приложения для Windows для вашего проекта. Он входит в пакет Windows 10 SDK, и если у вас установлена Visual Studio, доступ к нему можно получить через командную строку разработчика для вашей версии Visual Studio.

См. раздел [Создание пакета приложения с помощью средства MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>Запуск упакованного приложения

Чтобы запустить приложение для локальной проверки, не обязательно получать сертификат и подписывать его. Просто запустите этот командлет PowerShell:

```Add-AppxPackage –Register AppxManifest.xml```

Чтобы обновить файлы .exe или .dll приложения, замените существующие файлы в пакете новыми, повысьте номер версии в AppxManifest.xml, а затем выполните указанную выше команду повторно.

> [!NOTE]
> Упакованное приложение всегда выполняется от лица текущего пользователя, а диск, на который производится установка приложения, должен быть отформатирован в NTFS.

## <a name="next-steps"></a>Дальнейшие действия

**Поиск ответов на вопросы**

Есть вопросы? Задайте их на Stack Overflow. Наша команда отслеживает эти [теги](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Вы также можете задать нам вопрос [здесь](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Оставьте отзыв или предложите новые возможности для реализации**

См. раздел [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Пошаговое выполнение кода, поиск и устранение проблем**

См. [Запуск, отладка и тестирование упакованного классического приложения (мост для классических приложений)](desktop-to-uwp-debug.md)

**Подписание и распространение приложения**

См. [Распространение упакованного классического приложения (мост для классических приложений)](desktop-to-uwp-distribute.md)