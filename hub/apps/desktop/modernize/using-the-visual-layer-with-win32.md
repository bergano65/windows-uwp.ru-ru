---
title: Использование на визуальном уровне с Win32
description: Использование слоя визуализации для улучшения пользовательского интерфейса из классического приложения Win32.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, Подготовка к просмотру, композиции, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215190"
---
# <a name="using-the-visual-layer-with-win32"></a>Использование на визуальном уровне с Win32

Вы можете использовать API композиции среды выполнения Windows (также называется [визуальный слой](/windows/uwp/composition/visual-layer)) в приложениях Win32 для создания современных приложений, света, для пользователей Windows 10.

Полный код для этого руководства можно найти в GitHub: [Пример Win32 HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Универсальные приложения Windows, которым требуется точный контроль над их создание пользовательского интерфейса иметь доступ к [Windows.UI.Composition](/uwp/api/windows.ui.composition) пространства имен на точный контроль над как составляется и подготовке к просмотру их пользовательского интерфейса. Этот API композиции тем не менее не ограничивается приложений универсальной платформы Windows. Классическим приложениям Win32 можно воспользоваться преимуществами современных композиции систем в универсальной платформы Windows и Windows 10.

## <a name="prerequisites"></a>Предварительные требования

UWP, интерфейс API размещения имеет эти условия.

- Мы предполагаем, что у вас есть некоторый опыт работы с разработкой приложений, с помощью Win32 и UWP. См. также:
  - [Начало работы с Win32 иC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Начало работы с приложениями Windows 10](/windows/uwp/get-started/)
  - [Давайте расширим приложение рабочего стола для Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 версии 1803 или более поздней версии
- Пакет SDK Windows 10 17134 или более поздней версии

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Как использовать интерфейсы API композиции из классического приложения Win32

В этом руководстве создается простой Win32 C++ приложения и добавьте в него элементы композиции универсальной платформы Windows. Фокус установлен на правильно настройки проекта, создания код взаимодействия и Рисование простой, с помощью интерфейсов API композиции Windows. Готовое приложение выглядит следующим образом.

![Выполняющееся приложение пользовательского интерфейса](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Создание C++ проекта Win32 в Visual Studio

Первым шагом является создание проекта приложения Win32 в Visual Studio.

Чтобы создать новый проект приложения Win32 в C++ с именем _HelloComposition_:

1. Откройте Visual Studio и выберите **файл** > **New** > **проекта**.

    **Новый проект** откроется диалоговое окно.
1. В разделе **установленные** категории, разверните **Visual C++**  узел, а затем выберите **Windows Desktop**.
1. Выберите **классическое приложение Windows** шаблона.
1. Введите имя _HelloComposition_, затем нажмите кнопку **ОК**.

    Visual Studio создаст проект и открывает редактор для файла основного приложения.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Настройка проекта для использования API среды выполнения Windows

Для использования среды выполнения Windows (WinRT) API-интерфейсов в приложении Win32, мы используем C++/WinRT. Необходимо настроить проект Visual Studio, чтобы добавить C++/WinRT поддержки.

(Дополнительные сведения см. в разделе [приступить к работе с C++/WinRT - изменить проект приложения Windows Desktop, чтобы добавить C++поддержки /WinRT](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. Из **проекта** меню, откройте свойства проекта (_HelloComposition свойства_) и настройте следующие параметры для указанных значений:

    - Для **конфигурации**выберите _все конфигурации_. Для **платформы**выберите _всех платформ_.
    - **Свойства конфигурации** > **Общие** > **версия пакета SDK для Windows** = _10.0.17763.0_ или более поздней версии

    ![Установите версию пакета SDK](images/visual-layer-interop/sdk-version.png)

    - **C /C++** > **языка**  >   **C++ стандартом языка** = _ISO C++ стандарт 17 (/ stf:c ++ 17)_

    ![Набор языковой стандарт](images/visual-layer-interop/language-standard.png)

    - **Компоновщик** > **ввода** > **Дополнительные зависимости** должен включать "_windowsapp.lib_«. Если оно не указано в списке, добавьте его.

    ![Добавьте зависимость компоновщика](images/visual-layer-interop/linker-dependencies.png)

1. Обновление файла предкомпилированного заголовка

    - Переименуйте `stdafx.h` и `stdafx.cpp` для `pch.h` и `pch.cpp`, соответственно.
    - Установите свойство проекта **C/C++** > **предкомпилированные заголовки** > **файла предкомпилированного заголовка** для *pch.h*.
    - Поиск и замена `#include "stdafx.h"` с `#include "pch.h"` во всех файлах.

        (**Изменить** > **поиск и замена** > **поиск в файлах**)

        ![Поиск и замена stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - В `pch.h`, включают `winrt/base.h` и `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Это хороший способ построения проекта на этом этапе, чтобы убедиться в отсутствии ошибок перед переходом.

## <a name="create-a-class-to-host-composition-elements"></a>Создание класса для узла композиции элементов

Для размещения содержимого можно создавать с помощью визуальном уровне, создайте класс (_CompositionHost_) для управления взаимодействия и создания элементов композиции. Это, где выполняется большая часть конфигурации для размещения API композиции, включая:

- Начало [композитор](/uwp/api/windows.ui.composition.compositor), который создает и управляет объектами в [Windows.UI.Composition](/uwp/api/windows.ui.composition) пространства имен.
- Создание [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) для управления задачами для API-интерфейсы WinRT.
- Создание [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) и контейнер композиции для отображения объектов композиции.

Мы делаем это класса одноэлементный во избежание потоками. К примеру только можно создать одной очереди диспетчера каждого потока, поэтому создание экземпляров второй экземпляр CompositionHost в одном потоке приведет к ошибке.

> [!TIP]
> Если вам нужно, проверьте полный код в конце учебника, чтобы убедиться, что весь код находится в нужных местах во время работы с учебником.

1. Добавьте новый файл класса в проект.
    - В **обозревателе решений**, щелкните правой кнопкой мыши _HelloComposition_ проекта.
    - В контекстном меню выберите **добавить** > **класс...** .
    - В **Добавление класса** диалоговое окно, имя класса _CompositionHost.cs_, затем нажмите кнопку **добавить**.

1. Включить заголовки и _директивы using_ необходимые для взаимодействия композиции.
    - В CompositionHost.h, добавьте следующие _включает в себя_ в верхней части файла.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - В CompositionHost.cpp, добавьте следующие _директивы using_ в верхней части файла, после любых _включает в себя_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Отредактируйте класс использовать одноэкземплярный шаблон.
    - В CompositionHost.h сделайте конструктор закрытым.
    - Объявить как public static _GetInstance_ метод.

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - В CompositionHost.cpp, добавьте определение параметра _GetInstance_ метод.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. В CompositionHost.h объявите закрытый член переменные для компоновщика, DispatcherQueueController и DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Добавьте открытый метод для инициализации объектов взаимодействия композиции.
    > [!NOTE]
    > В _инициализировать_, вы вызываете _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_, и _CreateCompositionRoot_ методы. При создании этих методов на следующих шагах.

    - В CompositionHost.h, объявлять открытый метод с именем _инициализировать_ , принимающий HWND в качестве аргумента.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - В CompositionHost.cpp, добавьте определение параметра _инициализировать_ метод.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Создайте очередь dispatcher в потоке, который будет использоваться композиции Windows.

    Компоновщик должны создаваться в потоке, который имеет очередь dispatcher, поэтому этот метод вызывается первой во время инициализации.

    - В CompositionHost.h, объявить закрытый метод с именем _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - В CompositionHost.cpp, добавьте определение параметра _EnsureDispatcherQueue_ метод.

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. Зарегистрируйте окно приложения как адресат композиции.
    - В CompositionHost.h, объявить закрытый метод с именем _CreateDesktopWindowTarget_ , принимающий HWND в качестве аргумента.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - В CompositionHost.cpp, добавьте определение параметра _CreateDesktopWindowTarget_ метод.

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. Создайте корневой визуальный контейнер для размещения визуальных объектов.
    - В CompositionHost.h, объявить закрытый метод с именем _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - В CompositionHost.cpp, добавьте определение параметра _CreateCompositionRoot_ метод.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

Постройте проект сейчас, чтобы убедиться в отсутствии ошибок.

Эти методы, настроить компоненты, необходимые для взаимодействия между визуальном уровне универсальной платформы Windows и API-интерфейсов Win32. Теперь можно добавить содержимое в приложение.

### <a name="add-composition-elements"></a>Добавьте элементы композиции

С помощью инфраструктуры на месте можно приступить к созданию содержимого композиции, который вы хотите показать.

В этом примере добавляется код, который получается квадрат случайным образом цветные [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) с анимацию, которая приводит к разрыву после короткой задержки.

1. Добавьте элемент композиции.
    - В CompositionHost.h, объявлять открытый метод с именем _AddElement_ , принимающий 3 **float** значения в качестве аргументов.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - В CompositionHost.cpp, добавьте определение параметра _AddElement_ метод.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Создание и открытие окна ""

Теперь можно добавить кнопку и содержимое композиции универсальной платформы Windows для пользовательского интерфейса Win32.

1. В HelloComposition.cpp, в верхней части файла, включать _CompositionHost.h_, определение BTN_ADD и получения экземпляра CompositionHost.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. В `InitInstance` метод, измените размер окна, который создается. (В этой строке измените `CW_USEDEFAULT, 0` для `900, 672`.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. Добавьте в функцию WndProc `case WM_CREATE` для _сообщение_ блок switch. В этом случае инициализация CompositionHost и кнопка "Создать".

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. Также в функции WndProc обработать нажатие кнопки для добавления элемента композиции в пользовательский интерфейс. 

    Добавить `case BTN_ADD` для _wmId_ блок switch внутри блока WM_COMMAND.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

Теперь можно построить и запустить приложение. Если вам нужно, проверьте полный код в конце учебника, чтобы убедиться, что весь код находится в нужных местах.

Когда вы запустите приложение и нажмите кнопку, вы увидите анимированных квадратов, добавляется к UI.

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Пример Win32 HelloComposition (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Начало работы с Win32 иC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Начало работы с приложениями Windows 10](/windows/uwp/get-started/) (UWP)
- [Давайте расширим приложение рабочего стола для Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Пространство имен Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Полный код

Ниже приведен полный код для класса CompositionHost и InitInstance-метод.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (частично)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```