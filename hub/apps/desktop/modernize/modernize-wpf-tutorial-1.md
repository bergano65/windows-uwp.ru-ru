---
description: В этом руководстве показано, как добавлять пользовательские интерфейсы XAML UWP, создавать пакеты MSIX и внедрять в приложение WPF другие современные компоненты.
title: Перенос приложения Contoso Expenses в .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, о-ва XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317105"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Часть 1. Перенос приложения Contoso Expenses в .NET Core 3

Это первая часть учебника, в котором показано, как модернизировать пример классического приложения WPF с именем Contoso расходы. Общие сведения о руководстве, предварительных требованиях и инструкциях по скачиванию примера приложения см [. в разделе Учебник. Модернизировать приложение](modernize-wpf-tutorial.md)WPF.
  
В этой части руководства вы перенесете все приложение contoso "расходы" из .NET Framework 4.7.2 в [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Прежде чем начать эту часть руководства, убедитесь, что вы [откроете и создадите пример контосоекспенсес](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) в Visual Studio 2019.

> [!NOTE]
> Дополнительные сведения о переносе приложения WPF из .NET Framework в .NET Core 3 см. в [этой серии блогов](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Перенос проекта Контосоекспенсес в .NET Core 3

В этом разделе вы перенесите проект Контосоекспенсес в приложение "расходы Contoso" на .NET Core 3. Это можно сделать, создав новый файл проекта, содержащий те же файлы, что и существующий проект Контосоекспенсес, но предназначенный для .NET Core 3 вместо .NET Framework 4.7.2. Это позволяет поддерживать одно решение с .NET Framework и версиями .NET Core приложения.

1. Убедитесь, что в настоящее время проект Контосоекспенсес предназначен для .NET Framework 4.7.2. В обозреватель решений щелкните правой кнопкой мыши проект **контосоекспенсес** , выберите пункт **свойства**и убедитесь, что свойство **Целевая платформа** на вкладке **приложение** имеет значение .NET Framework 4.7.2.

    ![.NET Framework версии 4.7.2 для проекта](images/wpf-modernize-tutorial/NETFramework472.png)

3. В проводнике Windows перейдите в папку **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** и создайте новый текстовый файл с именем **контосоекспенсес. Core. csproj**.

4. Щелкните файл правой кнопкой мыши, выберите пункт **Открыть с помощью**и откройте его в любом текстовом редакторе, например в блокноте, Visual Studio Code или Visual Studio.

5. Скопируйте приведенный ниже текст в файл и сохраните его.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Закройте файл и вернитесь к решению **контосоекспенсес** в Visual Studio.

7. Щелкните правой кнопкой мыши решение **контосоекспенсес** и выберите **Добавить-> существующий проект**. Выберите файл **контосоекспенсес. Core. csproj** , который вы только что создали `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` в папке, чтобы добавить его в решение.

**Контосоекспенсес. Core. csproj** включает следующие элементы:

* Элемент **Project** указывает версию пакета SDK для **Microsoft. NET. SDK. виндовсдесктоп**. Это относится к приложениям .NET для Windows Desktop и включает компоненты для приложений WPF и Windows Forms.
* Элемент **PropertyGroup** содержит дочерние элементы, указывающие, что выходной файл проекта является исполняемым (а не библиотекой DLL), предназначен для .NET Core 3 и использует WPF. Для Windows Forms приложения вместо элемента **усевпф** следует использовать элемент **усевинформс** .

> [!NOTE]
> При работе с форматом CSPROJ, который появился в .NET Core 3,0, все файлы в той же папке, что и CSPROJ, считаются частью проекта. Поэтому не нужно указывать каждый файл, включаемый в проект. Необходимо указать только те файлы, для которых необходимо определить настраиваемое действие сборки или которые необходимо исключить.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Перенесите проект Контосоекспенсес. Data в .NET Standard

Решение **контосоекспенсес** включает библиотеку классов **контосоекспенсес. Data** , которая содержит модели и интерфейсы для служб и нацелены на .NET 4.7.2. Приложения .NET Core 3,0 могут использовать библиотеки .NET Framework, если они не используют интерфейсы API, которые недоступны в .NET Core. Однако наилучший путь модернизации заключается в перемещении библиотек в .NET Standard. Благодаря этому ваша библиотека будет полностью поддерживаться приложением .NET Core 3,0. Кроме того, библиотеку можно повторно использовать с другими платформами, такими как веб-сайт (с помощью ASP.NET Core) и Mobile (через Xamarin).

Чтобы выполнить миграцию проекта **контосоекспенсес. Data** в .NET Standard, выполните следующие действия.

1. В Visual Studio щелкните правой кнопкой мыши проект **контосоекспенсес. Data** и выберите команду **Выгрузить проект**. Снова щелкните проект правой кнопкой мыши и выберите пункт **изменить контосоекспенсес. Data. csproj**.

2. Удалите все содержимое файла проекта.

3. Скопируйте и вставьте следующий XML-код и сохраните файл.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Щелкните правой кнопкой мыши проект **контосоекспенсес. Data** и выберите **Перезагрузить проект**.

## <a name="configure-nuget-packages-and-dependencies"></a>Настройка пакетов и зависимостей NuGet

При миграции проектов **контосоекспенсес. Core** и **контосоекспенсес. Data** из предыдущих разделов вы удалили ссылки на пакет NuGet из проектов. В этом разделе вы добавите эти ссылки обратно.

Чтобы настроить пакеты NuGet для проекта **контосоекспенсес. Data** , выполните следующие действия.

1. В проекте **контосоекспенсес. Data** разверните узел **зависимости** . Обратите внимание, что отсутствует раздел **NuGet** .

    ![Пакеты NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    При открытии **файла Packages. config** в **Обозреватель решений** вы найдете "старые" ссылки на пакеты NuGet, которые использовали проект при использовании полной .NET Framework.

    ![Зависимости и пакеты](images/wpf-modernize-tutorial/Packages.png)

    Ниже приведено содержимое файла **Packages. config** . Вы увидите, что все пакеты NuGet предназначены для полной .NET Framework 4.7.2:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. В проекте **контосоекспенсес. Data** удалите файл **Packages. config** .

4. В проекте **контосоекспенсес. Data** щелкните правой кнопкой мыши узел **зависимости** и выберите пункт **Управление пакетами NuGet**.

  ![Управление пакетами NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. В окне **Диспетчер пакетов NuGet** нажмите кнопку **Обзор**. `Bogus` Найдите пакет и установите последнюю стабильную версию.

    ![Фиктивный пакет NuGet](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB` Найдите пакет и установите последнюю стабильную версию.

    ![Пакет NuGet Литедб](images/wpf-modernize-tutorial/LiteDB.png)

    Возможно, вас интересует, где хранится список пакетов NuGet, так как проект больше не содержит файл Packages. config. Пакеты NuGet, на которые указывают ссылки, хранятся непосредственно в файле. csproj. Это можно проверить, просмотрев содержимое файла проекта **контосоекспенсес. Data. csproj** в текстовом редакторе. В конце файла будут добавлены следующие строки:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Вы также можете заметить, что вы устанавливаете те же пакеты для этого проекта .NET Core 3, которые используются .NET Framework проектами 4.7.2. Пакеты NuGet поддерживают нацеливание на несколько версий. Авторы библиотек могут включать различные версии библиотеки в одном пакете, скомпилированные для разных архитектур и платформ. Эти пакеты поддерживают полную .NET Framework, а также .NET Standard 2,0, совместимые с проектами .NET Core 3. Дополнительные сведения о различиях .NET Framework, .NET Core и .NET Standard см. в разделе [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Чтобы настроить пакеты NuGet для проекта **контосоекспенсес. Core** , выполните следующие действия.

1. В проекте **контосоекспенсес. Core** откройте файл **Packages. config** . Обратите внимание, что в настоящее время он содержит следующие ссылки, предназначенные для .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    На следующих шагах вы .NET Standard версии `MvvmLightLibs` пакетов и. `Unity` Два других — зависимости, автоматически загружаемые NuGet при установке этих двух библиотек.

2. В проекте **контосоекспенсес. Core** удалите файл **Packages. config** .

3. Щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите пункт **Управление пакетами NuGet**.

4. В окне **Диспетчер пакетов NuGet** нажмите кнопку **Обзор**. `Unity` Найдите пакет и установите последнюю стабильную версию.

    ![Пакет Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10` Найдите пакет и установите последнюю стабильную версию. Это .NET Standard версия `MvvmLightLibs` пакета. Для этого пакета автор выбрал упаковку .NET Standard версии библиотеки в отдельном пакете, отличном от версии .NET Framework.

    ![Пакет Мввмлигхтслибс](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. В проекте **контосоекспенсес. Core** щелкните правой кнопкой мыши узел **зависимости** и выберите команду **Добавить ссылку**.

7. В категории **проект > решение** выберите **контосоекспенсес. Data** и нажмите кнопку **ОК**.

    ![Добавление ссылки](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Отключить автоматически созданные атрибуты сборки

На этом этапе процесса миграции, если вы попытаетесь выполнить сборку проекта **контосоекспенсес. Core** , вы увидите некоторые ошибки.

![Создание новых ошибок в .NET Core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Эта проблема возникает из-за того, что новый формат CSPROJ, появившийся в .NET Core 3,0, хранит сведения о сборке в файле проекта, а не в файле **AssemblyInfo.CS** . Чтобы устранить эти ошибки, отключите это поведение и позвольте проекту продолжить использовать файл **AssemblyInfo.CS** .

1. В Visual Studio щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите команду **Выгрузить проект**. Снова щелкните проект правой кнопкой мыши и выберите пункт **изменить контосоекспенсес. Core. csproj**.

1. Добавьте следующий элемент в раздел **PropertyGroup** и сохраните файл.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    После добавления этого элемента раздел **PropertyGroup** должен выглядеть следующим образом:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите **Перезагрузить проект**.

4. Щелкните правой кнопкой мыши проект **контосоекспенсес. Data** и выберите команду **Выгрузить проект**. Снова щелкните проект правой кнопкой мыши и выберите пункт **изменить контосоекспенсес. Data. csproj**.

5. Добавьте ту же запись в раздел **PropertyGroup** и сохраните файл.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    После добавления этого элемента раздел **PropertyGroup** должен выглядеть следующим образом:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Щелкните правой кнопкой мыши проект **контосоекспенсес. Data** и выберите **Перезагрузить проект**.

## <a name="add-the-windows-compatibility-pack"></a>Добавление пакета совместимости Windows

Если вы попытаетесь скомпилировать проекты **контосоекспенсес. Core** и **контосоекспенсес. Data** , вы увидите, что предыдущие ошибки теперь исправлены, но по-прежнему есть ошибки в библиотеке **контосоекспенсес. Data** , аналогичной этой.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Эти ошибки являются результатом преобразования проекта **контосоекспенсес. Data** из библиотеки .NET Framework (которая относится к Windows) к библиотеке .NET Standard, которая может выполняться на нескольких платформах, включая Linux, Android, iOS и т. д. Проект **контосоекспенсес. Data** содержит класс с именем **регистрисервице**, который взаимодействует с реестром, концепцией только для Windows.

Чтобы устранить эти ошибки, установите пакет NuGet для [обеспечения совместимости Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) . Этот пакет обеспечивает поддержку многих интерфейсов API Windows, используемых в библиотеке .NET Standard. После использования этого пакета библиотека больше не будет работать на разных платформах, но она будет по-прежнему нацелиться .NET Standard. 

1. Щелкните правой кнопкой мыши проект **контосоекспенсес. Data** .
2. Выберите **Управление пакетами NuGet**.
3. В окне **Диспетчер пакетов NuGet** нажмите кнопку **Обзор**. `Microsoft.Windows.Compatibility` Найдите пакет и установите последнюю стабильную версию.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Теперь повторите попытку компиляции проекта, щелкнув правой кнопкой мыши проект **контосоекспенсес. Data** и выбрав пункт **Сборка**.

На этот раз процесс сборки завершится без ошибок.

## <a name="test-and-debug-the-migration"></a>Тестирование и отладка миграции

Теперь, когда проекты успешно построены, вы можете запустить и протестировать приложение, чтобы проверить наличие ошибок времени выполнения.

1. Щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите команду **Назначить запускаемым проектом**.

2. Нажмите клавишу F5, чтобы запустить проект **контосоекспенсес. Core** в отладчике. Вы увидите исключение, аналогичное приведенному ниже.

    ![Исключение, отображаемое в Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Это исключение возникает из-за того, что при удалении содержимого из CSPROJ-файла в начале миграции были удалены сведения о **действии сборки** для файлов изображений. Чтобы устранить эту проблему, выполните следующие действия.

3. Останавливает отладчик.

4. Щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите команду **Выгрузить проект**. Снова щелкните проект правой кнопкой мыши и выберите пункт **изменить контосоекспенсес. Core. csproj**.

5. Перед закрывающим элементом **проекта** добавьте следующую запись:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите **Перезагрузить проект**.

7. Чтобы назначить приложению Contoso. ico, щелкните правой кнопкой мыши проект **контосоекспенсес. Core** и выберите пункт **свойства**. На открывшейся странице щелкните раскрывающийся список в разделе **значок** и `Images\contoso.ico`выберите.

    ![Значок Contoso в свойствах проекта](images/wpf-modernize-tutorial/ContosoIco.png)

8. Нажмите кнопку **Сохранить**.

9. Нажмите клавишу F5, чтобы запустить проект **контосоекспенсес. Core** в отладчике. Убедитесь, что приложение теперь запущено.

## <a name="next-steps"></a>Следующие шаги

На этом этапе работы с руководством вы успешно перенесли приложение contoso "расходы" в .NET Core 3. Теперь вы готовы [к части 2: Добавьте элемент управления UWP, используя острова](modernize-wpf-tutorial-2.md)XAML.

> [!NOTE]
> Если у вас есть экран с высоким разрешением, вы можете заметить, что приложение выглядит очень маленьким. Эту проблему можно решить на следующем шаге руководства.
