---
title: Создание консольного приложения для универсальной платформы Windows
description: В этом разделе описывается методика создания приложения UWP, работающего в окне консоли.
keywords: консоль uwp
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c2dba15d78301c84f4064bcd6548d44e3c17beb2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366355"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Создание консольного приложения для универсальной платформы Windows

В этом разделе описывается создание [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) или C++/CX консольное приложение универсальной платформы Windows (UWP).

Начиная с Windows 10 версии 1803, можно написать C++/WinRT или C++/CX UWP консольных приложений, работающих в окне консоли, таких как окно консоли DOS или PowerShell. Консольные приложения использовать окно консоли для ввода и вывода и можно использовать [универсальная среда выполнения C](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) функции, такие как **printf** и **getchar**. Консольные приложения UWP могут публиковаться в Microsoft Store. Им выделяется запись в списке приложений и основная плитка, которую можно закрепить в меню «Пуск». Консольные приложения универсальной платформы Windows можно запустить из меню «Пуск», хотя обычно будет запускать их из командной строки.

Чтобы просмотреть его в действие, вот видео о создании консольного приложения универсальной платформы Windows.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Используйте шаблон консольного приложения UWP 

Чтобы создать консольное приложение UWP, сначала установите **Шаблоны проектов консольного приложения (универсального)** , доступные в [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal). Установленные шаблоны будут доступны в разделе **новый проект** > **установленные** > **другие языки**  >  **Visual C++**   >  **Windows Universal** как **консольное приложение C++/WinRT (универсальной Windows)** и **консольное приложение C++/CX (универсальной Windows)** .

## <a name="add-your-code-to-main"></a>Добавьте свой код в main()

Шаблоны добавляют файл **Program.cpp**, содержащий функцию `main()`. Здесь начинается выполнение в консольном приложении UWP. Доступ к аргументам командной строки осуществляется с помощью параметров `__argc` и `__argv`. Консольное приложение UWP завершит работу, когда управление возвращается из `main()`.

В следующем примере **обрабатывает Program.cpp** добавляется путем **c консоли приложения + +/ WinRT** шаблона:

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>Поведение консольного приложения UWP

Консольное приложение UWP может получать доступ к файловой системе из каталога, в котором оно выполняется, и к нижестоящим элементам. Это возможно, поскольку шаблон добавляет расширение [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) в файл Package.appxmanifest вашего приложения. Это расширение также позволяет пользователю вводить имя приложения в окне консоли, чтобы запустить приложение. Приложение не требуется указывать в системном пути для запуска.

Можно предоставить обширный доступ к файловой системе консольному приложению UWP, добавив ограниченную возможность `broadFileSystemAccess`, как описано в разделе [Права доступа к файлам](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). Эта возможность работает с API-интерфейсами в пространстве [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

Одновременно могут выполняться несколько экземпляров консольного приложения UWP, поскольку шаблон добавляет возможность [SupportsMultipleInstances](multi-instance-uwp.md) в файл Package.appxmanifest вашего приложения.

Шаблон также добавляет возможность `Subsystem="console"` в файл Package.appxmanifest, и это обозначает, что это приложение UWP является консольным. Обратите внимание на префиксы `desktop4` и iot2 `namespace`. Консольные приложения UWP поддерживаются только в проектах для настольных ПК и Интернета вещей (IoT).

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Дополнительные рекомендации для консольных приложений UWP

- Только C++/WinRT и C++/CX приложений универсальной платформы Windows может быть консольных приложений.
- Консольные приложения UWP должны быть предназначены для проектов для настольных ПК или Интернета вещей.
- Консольные приложения UWP не может создать окно. Они не могут использовать MessageBox(), или Location() или другие API, создать окно по любой причине, такие как запросы согласия пользователя.
- Консольные приложения UWP не могут использовать фоновые задачи и не могут выступать в качестве фоновой задачи.
- Помимо [активации в командной строке](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97) консольные приложения UWP не поддерживают контракты активации, включая сопоставление файлов, протокол связи и т. д.
- Несмотря на то что консольные приложения UWP поддерживают создание нескольких экземпляров, они не поддерживают [перенаправления нескольких экземпляров](multi-instance-uwp.md)
- Список API-интерфейсов Win32, доступных приложениям UWP, см. в разделе [API-интерфейсы Win32 и COM для приложений UWP](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)