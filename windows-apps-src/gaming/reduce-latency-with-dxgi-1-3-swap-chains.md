---
title: Сокращение задержки с помощью цепочек буферов DXGI 1.3
description: DXGI 1.3 позволяет сократить фактическую задержку кадров путем ожидания от цепочки буферов сигнала о подходящем моменте для начала отрисовки нового кадра.
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, задержка, dxgi, цепочки буферов, directx
ms.localizationpriority: medium
ms.openlocfilehash: dd414c3ea65d30253d54cd335ed0b85d151b6dff
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258436"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>Сокращение задержки с помощью цепочек буферов DXGI 1.3



DXGI 1.3 позволяет сократить фактическую задержку кадров путем ожидания от цепочки буферов сигнала о подходящем моменте для начала отрисовки нового кадра. Обычно в играх необходимо обеспечить минимальную задержку с момента получения ввода от игрока до момента отклика игры (обновления экрана). В этом разделе описан метод, доступный, начиная с Direct3D 11.2, при помощи которого можно свести к минимуму фактическую задержку кадров в игре.

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>Каким образом ожидание заднего буфера сокращает задержку?


Если у нас есть цепочка буферов на основе модели отражений, «отражения» заднего буфера ставятся в очередь каждый раз, когда игра вызывает функцию [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present). Когда цикл отрисовки вызывает Present(), система блокирует поток, пока не закончится отображение предыдущего кадра, освобождая место в очереди для нового кадра, до его фактического отображения. Такая логика создает дополнительную задержку между моментом отрисовки кадра в игре и моментом, когда система разрешит отобразить этот кадр. Во многих случаях система достигает устойчивого равновесия, при котором в игре всегда имеется период ожидания между временем отрисовки и временем отображения каждого кадра величиной почти в полный дополнительный кадр. Лучше подождать, пока система будет готова принять новый кадр, затем выполнить отрисовку этого кадра по текущим данным и сразу поставить его в очередь.

Создайте цепочку ожидающих подкачки с помощью [**DXGI\_ПЕРЕключить цепочку\_\_флаг\_рамки\_задержку**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)\_\_ный флаг объекта. Созданные таким способом цепочки буферов могут уведомлять цикл отрисовки, когда система фактически готова принять новый кадр. В итоге игра может выполнить отрисовку по текущим данным и затем сразу поставить результирующий кадр в очередь на отображение.

## <a name="step-1-create-a-waitable-swap-chain"></a>Шаг 1. Создание цепочки буферов с ожиданием


При вызове [**креатесвапчаинфоркоревиндов**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)укажите [ **\_\_флага цепочки\_рамки\_** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) ,\_задержка\_время ожидания.\_

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> **Примечание** .   в отличие от некоторых флагов, этот флаг нельзя добавить или удалить с помощью [**ресизебуфферс**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers). DXGI возвращает код ошибки, если значение этого флага отличается от установленного во время создания цепочки буферов.

 

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>Шаг 2. Настройка задержки кадров


Задайте задержку кадров с помощью API [**IDXGISwapChain2::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency) вместо вызова [**IDXGIDevice1::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency).

По умолчанию задержка кадров для цепочек буферов с ожиданием установлена равной 1, что обеспечивает минимальную задержку, но также снижает степень параллелизма ЦП и GPU. Если вам требуется более высокая степень такого параллелизма для достижения скорости 60 кадров/с, то есть если ЦП и GPU в отдельности затрачивают менее 16,7 мс на отрисовку кадров, но их общее суммарное время превышает 16,7 мс, задайте задержку кадров равной 2. Это позволяет GPU выполнять задачи, поставленные в очередь ЦП во время предыдущего кадра, а ЦП — независимо отправлять команды для отрисовки текущего кадра.

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>Шаг 3. Получение объекта ожидания от цепочки буферов


Вызовите [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) для получения дескриптора ожидания. Дескриптор ожидания — это указатель на объект ожидания. Сохраните этот дескриптор — он будет использоваться циклом отрисовки.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>Шаг 4. Ожидание перед отрисовкой каждого кадра


Прежде чем начинать отрисовку каждого кадра, цикл отрисовки должен дождаться сигнала от цепочки буферов, передаваемого через объект ожидания. Это относится и к первому кадру, отрисованному с использованием цепочки буферов. Используйте функцию [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex), предоставляя дескриптор ожидания, полученный в шаге 2, для указания начала каждого кадра.

В следующем примере показан цикл отрисовки из примера DirectXLatency:

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

В следующем примере показан вызов WaitForSingleObjectEx из примера DirectXLatency:

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>Что должна делать игра во время ожидания представления цепочки буферов?


Если в игре нет никаких задач, пересекающихся с циклом отрисовки, ей следует ожидать представления цепочки буферов, поскольку это помогает сэкономить электроэнергию, что особенно важно на мобильных устройствах. В противном случае можно использовать многопоточность для выполнения задач, пока игра ожидает представления цепочки буферов. Вот примеры задач, которые может выполнять игра:

-   Обработка сетевых событий
-   Обновление искусственного интеллекта
-   Обработка физических эффектов на ЦП
-   Отрисовка с отложенным контекстом (на поддерживаемых устройствах)
-   Загрузка ресурсов

Дополнительные сведения о многопоточном программировании в Windows см. в следующих разделах по теме.

## <a name="related-topics"></a>См. также


* [Пример Директкслатенци](https://code.msdn.microsoft.com/windowsapps/DirectXLatency-sample-a2e2c9c3)
* [**IDXGISwapChain2:: Жетфрамелатенциваитаблеобжект**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows. System. Threading**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)
* [Асинхронное программирование вC++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)
* [Процессы и потоки](https://docs.microsoft.com/windows/desktop/ProcThread/processes-and-threads)
* [Синхронизиру](https://docs.microsoft.com/windows/desktop/Sync/synchronization)
* [Использование объектов событий (Windows)](https://docs.microsoft.com/windows/desktop/Sync/using-event-objects)

 

 




