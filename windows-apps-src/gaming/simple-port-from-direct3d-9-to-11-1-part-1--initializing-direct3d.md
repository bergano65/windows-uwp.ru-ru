---
title: Инициализация Direct3D 11
description: Здесь показано, как перенести код инициализации Direct3D 9 в Direct3D 11, в том числе как получить дескрипторы и контекст устройства Direct3D, а также как использовать DXGI для создания цепочки буферов.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, direct3d 11, инициализация, портирование, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: c5a7f33ddbc6d70af5293b92165892c2098e452d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368036"
---
# <a name="initialize-direct3d-11"></a>Инициализация Direct3D 11



**Сводка**

-   Часть 1. Инициализация Direct3D 11
-   [Часть 2. Преобразовать платформу для отображения](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [Часть 3. Порт цикл игры](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Здесь показано, как перенести код инициализации Direct3D 9 в Direct3D 11, в том числе как получить дескрипторы и контекст устройства Direct3D, а также как использовать DXGI для создания цепочки буферов. [Пошаговое руководство: портирование простого приложения с Direct3D 9 на DirectX 11 и на универсальную платформу для Windows (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md), часть 1.

## <a name="initialize-the-direct3d-device"></a>Инициализация устройства Direct3D


В Direct3D 9 мы создавали дескриптор устройства Direct3D, вызывая метод [**IDirect3D9::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice). Мы начинали с получения указателя на [**интерфейс IDirect3D9**](https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9) и задавали ряд параметров для управления конфигурацией устройства Direct3D и цепочки буферов. Перед этим мы вызывали функцию [**GetDeviceCaps**](https://docs.microsoft.com/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps), чтобы убедиться, что не просим устройство сделать то, на что оно неспособно.

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

В Direct3D 11 контекст устройства и графическая инфраструктура считаются отдельными от устройства. Инициализация разделена на множество шагов.

Сначала мы создаем устройство. При этом мы получаем список уровней компонентов, которые поддерживает устройство — это большая часть того, что нам нужно знать о графическом процессоре. Кроме того, нам не надо создавать интерфейс только для того, чтобы обращаться к Direct3D. Вместо этого мы используем базовый API [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). От него мы получаем дескриптор и непосредственный контекст устройства. Контекст устройства используется для установки состояния конвейера и генерации команд отрисовки.

Создав устройство Direct3D 11 и его контекст, мы можем воспользоваться функциональностью COM-указателей для получения последней версии интерфейсов, которые содержат дополнительные возможности – это всегда рекомендуется.

> **Примечание**    D3D\_ФУНКЦИЯ\_уровень\_9\_1 (который соответствует модели построителя текстуры 2.0) — это минимальный уровень игры Microsoft Store необходима для поддержки. (Пакеты ARM свою игру завершится ошибкой сертификации, если не поддерживают 9\_1.) Если игры также включает в себя путь отрисовки для функций модели 3 шейдера, то следует включить D3D\_ФУНКЦИЯ\_уровень\_9\_3 в массиве.

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>Создание цепочки буферов


В Direct3D 11 имеется API устройства под названием DXGI (DirectX Graphics Infrastructure). Интерфейс DXGI позволяет нам (среди прочего) управлять конфигурацией цепочки буферов и настраивать общий доступ к устройствам. На этом шаге инициализации Direct3D мы создадим цепочку буферов с использованием DXGI. Так как мы создали устройство, мы можем проследовать назад по цепочке интерфейсов до адаптера DXGI.

Устройство Direct3D реализует COM-интерфейс для DXGI. Первым делом нам нужно получить этот интерфейс и с его помощью запросить адаптер DXGI, в котором содержится устройство. Затем мы воспользуемся адаптером DXGI для создания фабрики DXGI.

> **Примечание**    это COM-интерфейсов, поэтому необходимо использовать первые [ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Но вместо него следует использовать интеллектуальные указатели [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class). После этого просто вызовите метод [**As()** ](https://docs.microsoft.com/previous-versions/br230426(v=vs.140)), передав ему пустой COM-указатель на надлежащий тип интерфейса.

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

Теперь, когда у нас есть фабрика DXGI, мы можем с ее помощью создать цепочку буферов. Определимся с параметрами цепочки буферов. Нам нужно указать формат поверхности; мы выберем [ **DXGI\_ФОРМАТ\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) так, как она стала совместима с Direct2D. Масштабирование дисплея, множественную дискретизацию и стереоскопическую отрисовку мы отключим, так как они не используются в этом примере. Поскольку работа идет непосредственно в CoreWindow, мы можем оставить ширину и высоту равными 0 и получить полноэкранные значения автоматически.

> **Примечание**    набор всегда *SDKVersion* параметр D3D11\_SDK\_версии для приложений универсальной платформы Windows.

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

Чтобы убедиться, мы не чаще, чем экрана фактически может отображать визуализации, мы устанавливаем задержкой кадров 1 и используйте [ **DXGI\_ЗАМЕНЫ\_эффект\_ПЕРЕВЕРНУТЬ\_SEQUENTIAL** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect). Тем самым экономится энергия, а кроме того это требуется для сертификации. Подробнее о представлении на экране вы узнаете из части 2 этой пошаговой инструкции.

> **Примечание**    можно использовать многопоточность (например, [ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading) рабочие элементы) для продолжения другую работу, пока поток отрисовки заблокирован.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

Теперь мы можем настроить задний буфер для отрисовки.

## <a name="configure-the-back-buffer-as-a-render-target"></a>Настройка заднего буфера в качестве цели отрисовки


Сначала нам необходимо получить дескриптор заднего буфера. (Обратите внимание, что принадлежит заднего буфера цепочки буферов DXGI, тогда как в DirectX 9 он принадлежал устройства Direct3D.) Затем мы указываем устройства Direct3D для использования в качестве целевого объекта отрисовки, создав целевой объект отрисовки *представление* с помощью задний буфер.

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

Теперь в игру вступает контекст устройства. Мы предписываем Direct3D работать с только что созданным представлением цели отрисовки, используя интерфейс контекста устройства. Чтобы в качестве окна просмотра могло служить все экранное окно, мы считываем ширину и высоту заднего буфера. Обратите внимание, что задний буфер привязан к цепочке буферов, так что если размер экранного окна изменится (например, пользователь перетащит окно игры на другой монитор), размер заднего буфера также нужно будет скорректировать, и кое-какую подготовительную работу придется выполнить заново.

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

Теперь, когда у нас есть дескриптор устройства и полноэкранный буфер отрисовки, все готово к загрузке и рисованию геометрического содержимого. По-прежнему [часть 2: Подготовка к просмотру](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md).

 

 




