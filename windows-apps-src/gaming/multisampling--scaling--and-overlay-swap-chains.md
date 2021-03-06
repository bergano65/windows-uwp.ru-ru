---
title: Наложения и масштабирование цепочки буферов
description: Узнайте, как создавать масштабируемые цепочки буферов для более быстрой отрисовки на мобильных устройствах и как использовать наложения цепочки буферов (если они есть) для повышения качества визуального восприятия.
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, масштабирование цепочки буферов, наложения, directx
ms.localizationpriority: medium
ms.openlocfilehash: 77e5dac007654449fcfbe2053fda65b123e073fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368347"
---
# <a name="swap-chain-scaling-and-overlays"></a>Наложения и масштабирование цепочки буферов



Узнайте, как создавать масштабируемые цепочки буферов для более быстрой отрисовки на мобильных устройствах и как использовать наложения цепочки буферов (если они есть) для повышения качества визуального восприятия.

## <a name="swap-chains-in-directx-112"></a>Цепочка буферов в DirectX 11.2


Direct3D 11.2 позволяет создавать приложения универсальной платформы Windows (UWP) с цепочками буферов с более высоким разрешением по сравнению с неосновными (пониженными) разрешениями, что увеличивает скорость заполнения. Direct3D 11.2 также включает API для отрисовки с аппаратными наложениями, чтобы вы могли представить пользовательский интерфейс в другой цепочке буферов с основным разрешением. Благодаря этому игра может отрисовывать пользовательский интерфейс с полным основным разрешением и при этом сохранять высокую частоту кадров, тем самым позволяя максимально эффективно использовать мобильное устройство и экраны с высоким DPI (например, 3840 × 2160). В этой статье объясняется, как использовать перекрывающиеся цепочки буферов.

Direct3D 11.2 также предлагает новую возможность сокращения задержки с помощью цепочек буферов модели переключения. Более подробно читайте в статье о [сокращении задержек с помощью цепочек буферов DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md).

## <a name="use-swap-chain-scaling"></a>Использование масштабирования цепочки буферов


Если игра запущена на оборудовании прежних версий или на оборудовании с оптимизацией энергосбережения, имеет смысл отрисовывать содержимое игры в реальном времени в более низком разрешении по сравнению с фактическими возможностями экрана. Для этого цепочка буферов, используемая для отрисовки содержимого игры, должна быть меньше, чем основное разрешение, или следует использовать подобласть цепочки буферов.

1.  Во-первых, создайте цепочку буферов с высоким основным разрешением.

    ```cpp
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

    swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
    swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
    swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
    swapChainDesc.Stereo = false;
    swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
    swapChainDesc.SampleDesc.Quality = 0;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
    swapChainDesc.Flags = 0;
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
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

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  Затем выберите подобласть цепочки буферов, масштаб которой будет увеличен, настроив исходный размер для более низкого разрешения.

    В примере цепочек буферов переднего плана DX рассчитывается уменьшенный размер на основании процентного соотношения:

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  Создайте окно просмотра, соответствующее подобласти цепочки буферов.

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  Если используется Direct2D, необходимо откорректировать преобразование поворота для компенсации исходной области.

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>Создание цепочки буферов с аппаратным наложением для элементов пользовательского интерфейса


Изменение масштаба цепочки буферов имеет один недостаток: также уменьшается масштаб пользовательского интерфейса, в результате чего элементы могут отображаться размыто и их может быть трудно использовать. На устройствах, аппаратно поддерживающих наложение цепочек буферов, эта проблема полностью решается за счет отрисовки пользовательского интерфейса с основным разрешением в цепочке буферов, отделенной от содержимого игры в реальном времени. Обратите внимание, что этот способ применяется только к цепочкам буферов [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) и его невозможно использовать при взаимодействии с XAML.

Чтобы создать цепочку буферов переднего плана, использующую возможность аппаратного наложения, выполните следующие действия. Эти действия выполняются после создания цепочки буферов для содержимого игры в реальном времени, как описано выше.

1.  Сначала определите, поддерживает ли адаптер DXGI наложения. Получите выходной адаптер DXGI из цепочки буферов:

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    Адаптер DXGI поддерживает наложения, если выходной адаптер возвращает значение «true» для [**SupportsOverlays**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays).

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **Примечание**    Если DXGI адаптер поддерживает наложение, перейдите к следующему шагу. Если устройство не поддерживает наложения, отрисовки с несколькими цепочками буферов будет недостаточно. Вместо этого отрисуйте пользовательский интерфейс с более низким разрешением в той же цепочке буферов, что и содержимое игры в реальном времени.

     

2.  Создайте цепочку буферов переднего плана с помощью [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow). Необходимо задать следующие параметры в [ **DXGI\_ЗАМЕНЫ\_ЦЕПОЧКИ\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) передаваемое *параметре pDesc* параметр:

    -   Укажите [ **DXGI\_ЗАМЕНЫ\_ЦЕПОЧКИ\_ФЛАГ\_переднего ПЛАНА\_СЛОЙ** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) замены цепочки флаг, указывающий цепочку обмена переднего плана.
    -   Используйте [ **DXGI\_альфа-канал\_режим\_PREMULTIPLIED** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_alpha_mode) флаг альфа-режима. Цепочки буферов переднего плана всегда предварительно умножены.
    -   Задайте [ **DXGI\_МАСШТАБИРОВАНИЕ\_NONE** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_scaling) флаг. Цепочки буферов переднего плана всегда выполняются с основным разрешением.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **Примечание**    задать [ **DXGI\_ЗАМЕНЫ\_ЦЕПОЧКИ\_ФЛАГ\_переднего ПЛАНА\_СЛОЙ** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) попытку каждые время, когда изменяется цепочки буферов.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  При использовании двух цепочек буферов увеличьте максимальную задержку кадров до 2, чтобы у адаптера DXGI было время на представление обеих цепочек буферов одновременно (в одном и том же интервале VSync).

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  Цепочки буферов переднего плана всегда используются с предварительным умножением альфа-канала. Ожидается, что значения цвета каждого пикселя уже умножены перед представлением кадра. Например, 100 % белый пиксель BGRA при альфа-канале 50 % имеет значение (0,5, 0,5, 0,5, 0,5).

    Альфа-premultiplication можно сделать на этапе средство слияния вывода путем применения к состоянию приложения blend (см. в разделе [ **ID3D11BlendState**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11blendstate)) с [ **D3D11\_ ВИЗУАЛИЗАЦИИ\_ЦЕЛЕВОЙ\_BLEND\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_blend_desc) структуры **SrcBlend** поле "значение" **D3D11\_SRC\_Альфа-канал**. Также можно использовать ресурсы с предварительно умноженными альфа-каналами.

    Если предварительное умножение альфа-канала не выполнено, цвета в цепочке буферов переднего плана будут ярче, чем ожидается.

5.  В зависимости от того, была ли создана цепочка буферов переднего плана, может потребоваться связать поверхность рисования Direct2D для элементов пользовательского интерфейса с цепочкой буферов переднего плана.

    Создание представлений однобуферной прорисовки:

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    Создание поверхности рисования Direct2D:

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  Представьте цепочку буферов переднего плана вместе с масштабируемой цепочкой буферов, используемой для содержимого игры в реальном времени. Поскольку для задержки кадров установлено значение 2 для обеих цепочек буферов, DXGI может представить их в одном и том же интервале VSync.

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 




