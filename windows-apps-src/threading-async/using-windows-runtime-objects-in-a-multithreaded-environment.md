---
title: Использование объектов среды выполнения Windows во многопоточной среде | Документы Майкрософт
description: В этой статье описывается, как .NET Framework обрабатывает вызовы из C# и Visual Basic кода в объекты, предоставляемые среда выполнения Windows или среда выполнения Windows компонентами.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: windows 10, uwp, таймер, потоки
ms.localizationpriority: medium
ms.openlocfilehash: 948b16c4e093f7b638ba9abc5bf18e30da8047a2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259799"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Использование объектов среды выполнения Windows в многопоточной среде
В этой статье описывается, как .NET Framework обрабатывает вызовы из C# и Visual Basic кода в объекты, предоставляемые среда выполнения Windows или среда выполнения Windows компонентами.

В .NET Framework по умолчанию можно обращаться к любому объекту из нескольких потоков без специальной обработки. Все, что для этого нужно — ссылка на объект. В среде выполнения Windows такие объекты называются *гибкими*. Большинство классов среды выполнения Windows являются гибкими, хотя и не все, и даже для гибких классов может потребоваться специальная обработка.

Везде, где возможно, среда CLR обрабатывает объекты из других источников, например из среды выполнения Windows, как объекты .NET Framework.

- Если объект реализует интерфейс [IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) или имеет атрибут [MarshalingBehaviorAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingbehaviorattribute.aspx) с типом [MarshalingType.Agile](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingtype.aspx), среда CLR рассматривает его как гибкий.

- Если среда CLR может маршалировать вызов из потока, где он был произведен, к контексту потока целевого объекта, это происходит прозрачно.

- Если объект имеет атрибут [MarshalingBehaviorAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingbehaviorattribute.aspx) с типом [MarshalingType.None](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingtype.aspx), класс не предоставляет информацию о маршалинге. Среда CLR не может маршалировать вызов, поэтому создается исключение [InvalidCastException](/dotnet/api/system.invalidcastexception) с сообщением о том, что объект можно использовать только в том контексте потоков, в котором он был создан.

В следующих разделах описаны последствия такого поведения для объектов из различных источников.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Объекты из компонента среды выполнения Windows, написанного на языке C# или Visual Basic
Все типы в компоненте, которые можно активировать, гибкие по умолчанию.

> [!NOTE]
>  Гибкость не подразумевает потокобезопасность. Как в среде выполнения Windows, так и в .NET Framework большинство классов не являются потокобезопасными, поскольку потокобезопасность снижает производительность, а большинство объектов никогда не используются сразу несколькими потоками. Более эффективно синхронизировать доступ к отдельным объектам (или использовать потокобезопасные классы) только при необходимости.

При создании компонента среды выполнения Windows значения по умолчанию можно переопределить. См. статьи об интерфейсах [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) и [IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject).

## <a name="objects-from-the-windows-runtime"></a>Объекты из среды выполнения Windows
Большинство классов в среде выполнения Windows являются гибкими, и среда CLR обрабатывает их как гибкие. В документации к этим классам среди атрибутов классов указан MarshalingBehaviorAttribute(Agile). Однако члены некоторых из этих гибких классов, например элементы управления XAML, создают исключения, если они не вызываются в потоке пользовательского интерфейса. Например, следующий код пытается использовать фоновый поток для задания свойства кнопки, которую нажали. Свойство кнопки [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) вызывает исключение.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

Безопасный доступ к кнопке можно получить, воспользовавшись ее свойством [Dispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.dispatcher.aspx) или свойством `Dispatcher` любого объекта, существующего в контексте потока пользовательского интерфейса (например, страницы, на которой находится эта кнопка). В приведенном ниже коде метод [RunAsync](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) объекта [CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) используется для перенаправления вызова в поток пользовательского интерфейса.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  При вызове свойства `Dispatcher` из другого потока исключение не возникает.

Время существования объекта среды выполнения Windows, который создается в потоке пользовательского интерфейса, ограничено временем существования этого потока. Не пытайтесь обращаться к объектам в потоке пользовательского интерфейса после закрытия окна.

При создании собственного элемента управления путем наследования от элемента управления XAML или путем объединения нескольких элементов управления XAML полученный элемент управления будет гибким, поскольку он является объектом .NET Framework. Однако если он вызывает члены своего базового класса или классов-составляющих, а также при вызове унаследованных членов, эти члены будут создавать исключения, когда они вызываются из любого потока, отличного от потока пользовательского интерфейса.

### <a name="classes-that-cant-be-marshaled"></a>Классы, которые невозможно маршалировать
У классов среды выполнения Windows, которые не предоставляют данные о маршалинге, есть атрибут [MarshalingBehaviorAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingbehaviorattribute.aspx) с типом [MarshalingType.None](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.marshalingtype.aspx). В документации к этим классам среди атрибутов классов указан MarshalingBehaviorAttribute(None).

Приведенный ниже код создает объект [CameraCaptureUI](https://msdn.microsoft.com/library/windows/apps/windows.media.capture.cameracaptureui.aspx) в потоке пользовательского интерфейса, а затем пытается задать свойство объекта из потока пула потоков. Среда CLR не может маршалировать вызов, поэтому создается исключение [System.InvalidCastException](/dotnet/api/system.invalidcastexception) с сообщением о том, что объект можно использовать только в том контексте потоков, в котором он был создан.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

В документации к [CameraCaptureUI](https://msdn.microsoft.com/library/windows/apps/windows.media.capture.cameracaptureui.aspx) среди атрибутов класса также указан ThreadingAttribute(STA), поскольку он должен создаваться в однопотоковом контексте, например в потоке пользовательского интерфейса.

Если необходимо обратиться к объекту [CameraCaptureUI](https://msdn.microsoft.com/library/windows/apps/windows.media.capture.cameracaptureui.aspx) из другого потока, можно кэшировать объект [CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) потока пользовательского интерфейса и использовать его в дальнейшем для перенаправления вызова в этот поток. Либо можно получить диспетчер из объекта XAML, например страницы, как показано в следующем примере кода.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Объекты из компонента среды выполнения Windows, написанного на C++
Классы в компоненте, который может быть активирован, являются гибкими по умолчанию. Однако C++ предоставляет значительные возможности контроля над моделями потоков и маршалингом. Как указано выше в этой статье, среда CLR распознает гибкие классы, пытается маршалировать вызовы, когда классы не являются гибкими, и создает исключение [System.InvalidCastException](/dotnet/api/system.invalidcastexception), когда класс не содержит данных о маршалинге.

Для объектов, которые работают в потоке пользовательского интерфейса и создают исключения при вызове из потоков, отличных от потоков пользовательского интерфейса, можно использовать объект потока пользовательского интерфейса [CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) для перенаправления вызова.

## <a name="see-also"></a>См. также
[Руководство по C#](/dotnet/csharp/)

[Visual Basic Guide](/dotnet/visual-basic/)
