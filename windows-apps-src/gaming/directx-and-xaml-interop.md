---
title: Взаимодействие DirectX и XAML
description: Вы можете использовать одновременно и XAML, и Microsoft DirectX в своей игре универсальной платформы Windows (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, directx, взаимодействие с xaml
ms.localizationpriority: medium
ms.openlocfilehash: 174cb7f2608c1da89ebacc21e5032d03f7701f15
ms.sourcegitcommit: 0179e2ccb59a14abc1676da0662e2def54af24ea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72796222"
---
# <a name="directx-and-xaml-interop"></a>Взаимодействие DirectX и XAML

Вы можете использовать одновременно и XAML, и Microsoft DirectX в своей игре или приложении универсальной платформы Windows (UWP). Сочетание XAML и DirectX позволяет создавать гибкие структуры пользовательского интерфейса, взаимодействующие с содержимым, обрабатываемым DirectX, что особенно удобно для приложений с интенсивным использованием графики. В этом разделе описывается структура приложения UWP, использующего DirectX, и вводятся важные типы, которые применяются при сборке приложения UWP для обеспечения работы с DirectX.

Если в вашем приложении в основном используется двухмерная отрисовка, возможно, вам стоит применять библиотеку [Win2D](https://github.com/microsoft/win2d) среды выполнения Windows. Эта библиотека, поддерживаемая корпорацией Microsoft, создана на основе базовых технологий Direct2D. Она значительно упрощает использование двумерной графики и включает удобные абстракции для некоторых методов, описанных в данном документе. Для получения дополнительных сведений см. страницу проекта. В этом документе приводится руководство для разработчиков приложений, которые *не* используют Win2D.

> [!NOTE]
> API-интерфейсы DirectX не определяются как типы Среда выполнения Windows, но вы можете использовать [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) для разработки компонентов XAML UWP, взаимодействующих с DirectX. Кроме того, вы можете создавать приложения UWP, использующие DirectX, на C# и XAML, если инкорпорируете вызовы DirectX в отдельный файл метаданных среды выполнения Windows.

## <a name="xaml-and-directx"></a>XAML и DirectX

DirectX предоставляет две мощные библиотеки для двух- и трехмерной графики: Direct2D и Microsoft Direct3D. Хотя язык XAML поддерживает базовые двухмерные примитивы и эффекты, многим приложениям (например, приложениям для моделирования или играм) необходима поддержка более сложной графики. В этих случаях библиотеки Direct2D и Direct3D можно использовать для обработки части или всей графики, а язык XAML — для всего остального.

Если вы реализуете особое межпрограммное взаимодействие между XAML и DirectX, вам нужно знать о двух следующих концепциях.

-   Общие поверхности — это области отображения заданного размера, определенные средствами языка XAML, на которых можно рисовать средствами DirectX (не напрямую) с помощью типов [Windows::UI::Xaml::Media::ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagesource). В случае общих поверхностей вы не управляете точной синхронизацией отображения нового содержимого на экране. Обновления общих поверхностей синхронизируются с обновлениями платформы XAML.
-   [Цепочки буферов](https://docs.microsoft.com/windows/desktop/direct3d9/what-is-a-swap-chain-) представляют собой коллекцию буферов, используемых для отображения графики с минимальной задержкой. Обычно цепочки буферов обновляются с частотой 60 кадров в секунду отдельно от потока пользовательского интерфейса. Однако цепочки буферов используют больше памяти и ресурсов процессора, чтобы обеспечить быстрое обновление. Кроме того, с ними сложнее работать, поскольку приходится управлять несколькими потоками.

Проанализируйте, для чего вы используете DirectX. Будет ли DirectX использоваться для формирования или анимации отдельного элемента управления, соответствующего размерам отображаемого окна? Будет ли DirectX содержать выходные данные, требующие обработки и управления в режиме реального времени, как, например, в игре? Если это так, вероятно, вам потребуется реализовать цепочку буферов. В противном случае следует использовать общую поверхность.

Определив способ использования DirectX, вы будете использовать один из типов среды выполнения Windows, чтобы включить отрисовку DirectX в свое приложение UWP:

-   Если вы хотите составить статическое изображение или рисовать сложное изображение через управляемые событиями интервалы, рисуйте их на общей поверхности с помощью типа [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource). Этот тип обрабатывает поверхности определенного размера, рисуемые средствами DirectX. Обычно данный тип используется при формировании изображения или текстуры, например точечного рисунка, для отображения в документе или элементе пользовательского интерфейса. Он плохо подходит для обеспечения взаимодействия в реальном времени, например в высокопроизводительных играх. Это связано с тем, что обновления объекта **SurfaceImageSource** синхронизированы с обновлениями пользовательского интерфейса XAML, из-за чего возможна задержка предоставляемой пользователю визуальной обратной связи, например флуктуация частоты кадров или заметное ухудшение реакции на ввод данных в реальном времени. Тем не менее, для динамических элементов управления или моделирования данных обновления выполняются достаточно быстро.

-   Если изображение больше предоставленного пространства на экране, пользователь может масштабировать его с помощью типа [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource). Данный тип обрабатывает нарисованную средствами DirectX поверхность определенного размера, которая больше экрана. Как и класс [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource), он используется при динамическом составлении сложного изображения или элемента управления. Кроме того, как и класс **SurfaceImageSource**, он плохо подходит для высокопроизводительных игр. К примерам элементов XAML, в которых может использоваться класс **VirtualSurfaceImageSource**, относятся элементы управления картой или средство просмотра документов с большим количеством изображений.

-   При использовании DirectX для представления графики, обновляемой в реальном времени, или в ситуации, когда обновления должны выполняться регулярно с низкой задержкой, используйте класс [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) для обновления графики без синхронизации с таймером обновления платформы XAML. Этот тип предоставляет прямой доступ к цепочке буферов графического устройства ([IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)) и слою XAML, наложенному на однобуферную прорисовку. Этот тип отлично подходит для игр и полноэкранных приложений DirectX, которым требуется пользовательский интерфейс на основе языка XAML. Чтобы использовать описанный подход, вы должны хорошо разбираться в DirectX, в том числе в технологиях Microsoft DirectX Graphics Infrastructure (DXGI), Direct2D и Direct3D. Дополнительные сведения см. в [Руководстве по программированию для Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews).

## <a name="surfaceimagesource"></a>SurfaceImageSource

[SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) предоставляет общие поверхности DirectX для рисования, а затем объединяет все биты в содержимое приложения.

Здесь описывается простой процесс создания и обновления объекта [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) в коде программной части.

1.  Определите размер общей поверхности, передав высоту и ширину конструктору [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource). Вы можете также указать, требуется ли поверхности поддержка альфа-режима (непрозрачности).

    Пример

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Получите указатель на интерфейс [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Приведите объект [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) к типу [IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (или **IUnknown**) и примените к нему метод **QueryInterface**, чтобы получить лежащую в основе реализацию интерфейса **ISurfaceImageSourceNativeWithD2D**. Методы, определенные в этой реализации, используются для настройки устройства и выполнения операций рисования.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Создайте устройства DXGI и D2D. Для этого сначала выполните вызов функций [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) и [D2D1CreateDevice](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice), а затем передайте устройство и контекст методу [ISurfaceImageSourceNativeWithD2D::SetDevice](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice). 

    > [!NOTE]
    > Если вы будете рисовать в **SurfaceImageSource** из фонового потока, убедитесь также, что на устройстве DXGI разрешен многопоточный доступ. Это действие необходимо выполнить для повышения производительности, только если вы будете рисовать из фонового потока.

    Пример

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  Предоставьте указатель на объект [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) для метода [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw) и используйте возвращенный контекст рисования, чтобы заполнить содержимое нужного прямоугольника в **SurfaceImageSource**. Метод **ISurfaceImageSourceNativeWithD2D::BeginDraw** и команды рисования можно вызывать из фонового потока. Прорисовка будет производиться только в области, указанной для обновления в параметре *updateRect*.

    Этот метод возвращает смещение точки (x, y) обновленного целевого прямоугольника в параметре *offset*. Используйте это смещение, чтобы определить местоположение, где необходимо отрисовать обновленное содержимое с объектом **ID2D1DeviceContext**.

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. Вызовите метод [ISurfaceImageSourceNativeWithD2D::EndDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw) для завершения формирования растрового изображения. Растровое изображение можно использовать в XAML в качестве источника для [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) или [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush). **ISurfaceImageSourceNativeWithD2D::EndDraw** следует вызывать только из потока пользовательского интерфейса.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > Вызов метода [SurfaceImageSource::SetSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) (наследуемого от **IBitmapSource::SetSource**) в настоящее время инициирует исключение. Не вызывайте его из вашего объекта [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource).

    > [!NOTE]
    > Приложения должны избегать рисования на объекте **SurfaceImageSource**, пока связанный с ними объект [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) скрыт, в противном случае API **ISurfaceImageSourceNativeWithD2D** завершится с ошибкой. Для этого зарегистрируйте событие [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) в качестве прослушивателя событий, чтобы отслеживать изменения видимости.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

Класс [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) расширяет класс [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource), когда содержимое может не поместиться на экране и поэтому требуется виртуализация содержимого для оптимального формирования изображения.

Класс [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) отличается от класса [SurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) тем, что использует обратный вызов [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded), реализуемый для обновления областей поверхности по мере их появления на экране. Скрытые области очищать не нужно, так как за это отвечает платформа XAML.

Здесь описывается простой процесс создания и обновления объекта [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) в коде программной части.

1.  Создайте экземпляр класса [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) нужного размера. Пример

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Получите указатели на интерфейсы [IVirtualSurfaceImageSourceNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) and [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Приведите объект [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) к типу [IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable) или [IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) и примените к нему метод [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)), чтобы получить лежащие в основе реализации интерфейсов **IVirtualSurfaceImageSourceNative** и **ISurfaceImageSourceNativeWithD2D**. Методы, определенные в этих реализациях, используются для настройки устройства и выполнения операций рисования.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  Создайте устройства DXGI и D2D, вызвав функции **D3D11CreateDevice** и **D2D1CreateDevice**, а затем передайте устройство D2D методу **ISurfaceImageSourceNativeWithD2D::SetDevice**.

    > [!NOTE]
    > Если вы будете рисовать в **VirtualSurfaceImageSource** из фонового потока, убедитесь также, что на устройстве DXGI разрешен многопоточный доступ. Это действие необходимо выполнить для повышения производительности, только если вы будете рисовать из фонового потока.

    Пример

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Вызовите метод [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded), передав ссылку на свою реализацию интерфейса [IVirtualSurfaceUpdatesCallbackNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative).

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    Платформа вызывает вашу реализацию [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded), когда необходимо обновить область [VirtualSurfaceImageSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource).

    Такое может произойти, если платформа определяет область, которую необходимо нарисовать (например, если пользователь масштабирует вид поверхности), или после применения к такой области метода [IVirtualSurfaceImageSourceNative::Invalidate](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate), вызванного приложением.

5.  В методе [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded) с помощью методов [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) и [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects) определите, какие области поверхности необходимо нарисовать.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  И наконец, для каждой области, которую нужно обновить, выполните следующее:

    1.  Предоставьте указатель на объект **ID2D1DeviceContext** для метода **ISurfaceImageSourceNativeWithD2D::BeginDraw** и используйте возвращенный контекст рисования, чтобы заполнить содержимое нужного прямоугольника в **SurfaceImageSource**. Метод **ISurfaceImageSourceNativeWithD2D::BeginDraw** и команды рисования можно вызывать из фонового потока. Прорисовка будет производиться только в области, указанной для обновления в параметре *updateRect*.

        Этот метод возвращает смещение точки (x, y) обновленного целевого прямоугольника в параметре *offset*. Используйте это смещение, чтобы определить местоположение, где необходимо отрисовать обновленное содержимое с объектом **ID2D1DeviceContext**.

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  Рисуйте конкретное содержимое в этой области, но не выходите за пределы областей, чтобы не снизить производительность.

    3.  Вызовите метод **ISurfaceImageSourceNativeWithD2D::EndDraw**. Результатом станет растровое изображение.

> [!NOTE]
> Приложения должны избегать рисования на объекте **SurfaceImageSource**, пока связанный с ними объект [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) скрыт, в противном случае API **ISurfaceImageSourceNativeWithD2D** завершится с ошибкой. Для этого зарегистрируйте событие [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) в качестве прослушивателя событий, чтобы отслеживать изменения видимости.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel и игры


[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) — это тип среды выполнения Windows, разработанный для поддержки высокопроизводительных игр и графики, в котором вы управляете цепочкой буферов напрямую. В данном случае вы создаете собственную цепочку буферов DirectX и управляете представлением своего отображаемого содержимого.

Тип [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) имеет определенные ограничения, благодаря которым обеспечивается хорошая производительность:

-   Допускается только 4 экземпляра [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) на приложение.
-   Необходимо задать высоту и ширину цепочки буфера обмена DirectX (в [DXGI \_SWAP \_CHAIN \_DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) к текущим измерениям элемента цепочки подкачки. В противном случае отображаемое содержимое будет масштабироваться (с помощью **DXGI \_SCALING \_STRETCH**).
-   Необходимо задать режим масштабирования цепочки буфера обмена DirectX (в режиме [dxgi \_SWAP \_CHAIN \_DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) **\_SCALING DXGI \_STRETCH**.
-   Необходимо создать цепочку буферов DirectX путем вызова [IDXGIFactory2::CreateSwapChainForComposition](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition).

[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) обновляется в зависимости от потребностей приложения, а не обновлений платформы XAML. Если обновления **SwapChainPanel** необходимо синхронизировать с обновлениями платформы XAML, зарегистрируйте событие [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering). В противном случае необходимо учитывать все возможные проблемы с разными потоками при попытке обновления элементов XAML не из того потока, который обновляет **SwapChainPanel**.

Если для **SwapChainPanel** необходимо получить ввод указателя с низкой задержкой, используйте метод [SwapChainPanel::CreateCoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Этот метод возвращает объект [CoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.core.coreindependentinputsource), который можно использовать для получения событий ввода с минимальной задержкой в фоновом потоке. Обратите внимание, что после вызова этого метода обычные события ввода указателя XAML не будут задействованы для **SwapChainPanel**, так как весь ввод будет перенаправлен в фоновый поток.


> **Примечание.** В целом ваши приложения DirectX должны создавать цепочки буферов в альбомной ориентации, а их размер должен быть равен размеру окна отображения (как правило, он определяется собственным разрешением экрана в большинстве игр Microsoft Store). Таким образом гарантируется, что ваше приложение использует оптимальную реализацию цепочки буферов при отсутствии видимого наложения XAML. Если ориентация приложения меняется на книжную, ваше приложение должно вызывать [IDXGISwapChain1::SetRotation](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) в существующей цепочке буферов, при необходимости применить к содержимому преобразование, а затем вызывать метод [SetSwapChain](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) еще раз в той же цепочке буферов. Схожим образом ваше приложение должно снова вызывать **SetSwapChain** в той же цепочке буферов, когда размер этой цепочки изменяется посредством вызова [IDXGISwapChain::ResizeBuffers](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers).


 

Здесь описывается простой процесс создания и обновления объекта [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) в коде программной части.

1.  Получите экземпляр панели цепочки буферов для своего приложения. Экземпляры указываются в XAML-коде с помощью тега `<SwapChainPanel>`.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    Пример использования тега `<SwapChainPanel>`.

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Получите указатель на интерфейс [ISwapChainPanelNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative). Приведите объект [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) к типу [IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (или **IUnknown**) и примените к нему метод **QueryInterface**, чтобы получить лежащую в основе реализацию интерфейса **ISwapChainPanelNative**.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Создайте устройство DXGI и цепочку буферов. Цепочку буферов настройте в [ISwapChainPanelNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative), передав ее методу [SetSwapChain](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain).

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Нарисуйте цепочку буферов DirectX и выполните ее представление, чтобы отобразить содержимое.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Элементы XAML обновляются, когда логика макета или отрисовки среды выполнения Windows сигнализирует об обновлении.

## <a name="related-topics"></a>Статьи по теме

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [сурфацеимажесаурце](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [виртуалсурфацеимажесаурце](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [исвапчаинпанелнативе](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Рекомендации по программированию для Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews)