---
title: Расширение образца игры
description: Узнайте, как реализовать наложение XAML в игре UWP на базе DirectX.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 39fc465a38aa31e86d5c6162c4b333517c9efb4d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367693"
---
# <a name="extend-the-game-sample"></a>Расширение образца игры

Итак, вы уже знакомы с основными компонентами трехмерной игры универсальной платформы Windows (UWP) на базе DirectX. Вы можете настроить структуру игры, включая поставщик представлений и конвейер визуализации, а также реализовать основной цикл игры. Вы также можете создать базовое наложение пользовательского интерфейса, добавить звуки и реализовать элементы управления. Вы практически готовы к созданию своей собственной игры, но если вам нужна дополнительная информация и помощь, воспользуйтесь ресурсами ниже.

-   [Графика и игры на основе DirectX](https://docs.microsoft.com/windows/desktop/directx)
-   [Direct3D 11 Обзор](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews)
-   [Direct3D 11 ссылки](https://docs.microsoft.com/windows/desktop/direct3d11/d3d11-graphics-reference)

## <a name="using-xaml-for-the-overlay"></a>Использование XAML для создания наложения


Мы еще не рассматривали подробно такую альтернативу, как создание наложения с помощью XAML вместо [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal). С точки зрения рисования элементов пользовательского интерфейса XAML обеспечивает ряд преимуществ по сравнению с Direct2D. Самым важным преимуществом является, поэтому включение внешний вид Windows 10 в игры DirectX более удобным. Поскольку большое число общих элементов, стилей и функций, присущих приложениям UWP, интегрировано в модель XAML, задача разработчиков игр значительно облегчается. Если вы разрабатываете для своей игры сложный пользовательский интерфейс, советуем вам использовать XAML вместо Direct2D.

Используя XAML, можно сделать игру с интерфейсом, похожим на интерфейс на базе Direct2D, который мы создали раньше.

### <a name="xaml"></a>XAML
![Наложение XAML](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![Наложение D2D](./images/simple-dx-game-extend-d2d.PNG)

Несмотря на схожий конечный результат, между реализацией интерфейсов на базе Direct2D и XAML существует ряд различий.

Компонент | XAML| Direct2D
:----------|:----------- | :-----------
Определение наложения | Определяется в XAML-файле (`\*.xaml`). Как только вы разберетесь в XAML, создавать и конфигурировать сложные наложения будет проще, чем при использовании Direct2D.| Определяется в виде коллекции примитивов Direct2D и строк [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal), вручную помещаемых и записываемых в целевой буфер Direct2D. 
Элементы пользовательского интерфейса | Элементы пользовательского интерфейса на языке XAML происходят от стандартных элементов, входящих в состав API среды выполнения Windows на языке XAML, включая [**Windows::UI::Xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) и [**Windows::UI::Xaml::Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls). Код, обрабатывающий поведение элементов пользовательского интерфейса на языке XAML, определен в файле кода программной части Main.xaml.cpp. | Простые фигуры можно рисовать как прямоугольники и эллипсы.
Изменение размеров окон | События изменения размеров и состояния представления обрабатываются естественным образом, и наложение трансформируется в соответствии с ними | Необходимо вручную указывать, как будут перерисовываться компоненты наложения.


Другое существенное различие связано с [цепочкой буферов](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains). Цепочку буферов не нужно присоединять к объекту [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). Вместо этого приложение DirectX, в котором используется XAML, соединяется с цепочкой буферов при создании нового объекта [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel). 

В следующем фрагменте кода показано, как объявить разметку XAML для **SwapChainPanel** в файле [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml).
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">


    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

Объект **SwapChainPanel** задается как свойство [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) объекта текущего окна, создаваемого [при запуске](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) singleton-объектом приложения.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


Чтобы привязать настроенную цепочку буферов к экземпляру [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel), определенному разметкой XAML, необходимо получить указатель на базовую реализацию интерфейса [**ISwapChainPanelNative**](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) и вызвать для него метод [**ISwapChainPanelNative::SetSwapChain**](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain), передав ему настроенную цепочку буферов. 

Следующий фрагмент кода из примера [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) демонстрирует, как это делается для взаимодействия DirectX/XAML:

```cpp
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

Подробнее об этой процедуре см. в разделе [Межпрограммное взаимодействие DirectX и XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Пример

Версию этой игры, в которой для создания наложения используется XAML, можно скачать в статье [Образец игры-шутера для Direct3D (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).


(В отличие от версии игры, рассматриваемой в остальных статьях, XAML-версия определяет платформу в файлах [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) и [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) вместо файлов [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) и [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp) соответственно.)