---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: Обеспечение быстрого отклика потока пользовательского интерфейса
description: Пользователи, независимо от типа компьютера, ожидают от приложений быстрого отклика во время вычислений.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b9a129e8b780e85df2c38c50ab712641d3849a34
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339851"
---
# <a name="keep-the-ui-thread-responsive"></a>Обеспечение быстрого отклика потока пользовательского интерфейса


Пользователи, независимо от типа компьютера, ожидают от приложений быстрого отклика во время вычислений. Для разных приложений это подразумевает разные вещи. Это может означать предоставление более реалистичной физики, более быструю загрузку данных с диска или из Интернета, мгновенное представление сложных сцен и навигацию по страницам, быстрый поиск направлений или быструю обработку данных. Независимо от типа вычислений пользователи хотят, чтобы приложение реагировало на ввод данных, и удаляют экземпляры, которые престают отвечать во время вычислений.

Ваше приложение событийное — это означает, что ваш код выполняет действия в ответ на событие, а затем будет неактивен до следующего. Код платформы для пользовательского интерфейса (макета, ввода данных, создания событий и т. д.) и код вашего приложения для пользовательского интерфейса выполняются в одном потоке пользовательского интерфейса. В этом потоке одновременно может выполняться только одна инструкция, поэтому если код приложения обрабатывает событие слишком долго, то платформа не может выполнить макет или создать новые события, представляющие взаимодействие с пользователем. Скорость отклика приложения зависит от доступности потока пользовательского интерфейса для обработки заданий.

Чтобы внести изменения в поток пользовательского интерфейса, включая создание типов пользовательского интерфейса и получение доступа к их членам, необходимо использовать поток пользовательского интерфейса. Невозможно обновить интерфейс из фонового потока, но можно создать сообщение с помощью [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync), чтобы вызвать код для выполнения.

> **Обратите внимание** ,  единственным исключением является отдельный поток отрисовки, который может применить изменения пользовательского интерфейса, которые не влияют на обработку входных данных или базовый макет. Например, многие анимации и переходы, которые не влияют на макет, могут выполняться в этом потоке обработки.

## <a name="delay-element-instantiation"></a>Создание экземпляра элемента задержки

Некоторые наиболее медленные этапы в приложении могут включать запуск и переключение между представлениями. Не выполняйте лишнюю работу при выводе на экран пользовательского интерфейса, который изначально виден пользователю. Например, не создавайте пользовательский интерфейс для открывающегося по порядку пользовательского интерфейса и содержимого всплывающих окон.

-   Создавайте экземпляры элементов задержки с помощью [атрибута x:Load](../xaml-platform/x-load-attribute.md) или [x: DeferLoadStrategy](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute).
-   Вставьте программным путем элементы в дерево по запросу.

Очереди [**CoreDispatcher. рунидлеасинк**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync) работают для обработки в ПОТОКЕ пользовательского интерфейса, когда он не занят.

## <a name="use-asynchronous-apis"></a>Использование асинхронных API

Для поддержания нормальной скорости отклика вашего приложения платформа предоставляет асинхронные версии многих API. Асинхронный API гарантирует, что активный поток выполнения никогда не будет заблокирован в течение продолжительного времени. При вызове API из потока пользовательского интерфейса используйте по возможности асинхронную версию. Дополнительную информацию о программировании с использованием шаблонов **async** см. в разделе [Асинхронное программирование](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps) или [Вызов асинхронных API в C# и Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic).

## <a name="offload-work-to-background-threads"></a>Разгрузка выполнения в фоновые потоки

Напишите код для обработчика событий, чтобы он быстро возвращался. В случаях, когда необходимо выполнить нестандартный объем работы, запланируйте выполнение в фоновом потоке и его возвращение.

Запланировать задание можно асинхронно с помощью оператора **await** в C#, оператора **Await** в Visual Basic или делегатов в C++. Но это не гарантирует, что запланированное задание будет выполняться в фоновом потоке. Многие API универсальной платформы Windows (UWP) планируют выполнение в фоновом потоке, но если код приложения вызывается только с помощью **await** или делегата, этот делегат или метод будет выполнен в потоке пользовательского интерфейса. Необходимо явным образом указать, что требуется выполнение кода приложения в фоновом потоке. В C# и Visual Basic это можно сделать, передав код в [**Task. Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run).

Помните, что доступ к элементам пользовательского интерфейса можно получить только из потока. Используйте поток пользовательского интерфейса для получения доступа к элементам пользовательского интерфейса, прежде чем запустить фоновую работу и/или использовать [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) или [**CoreDispatcher.RunIdleAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync) в фоновом потоке.

В качестве примера кода, который можно выполнить в фоновом потоке, возьмем вычисление искусственного интеллекта компьютера в игре. На выполнение кода, вычисляющего следующий ход компьютера, может потребоваться достаточно много времени.

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

В этом примере обработчик `NextMove_Click` возвращается в **await**, чтобы обеспечить быстроту отклика потока пользовательского интерфейса. Однако выполнение снова обращается к этому обработчику после завершения `ComputeNextMove` (которое выполняется в фоновом потоке). Оставшийся код в обработчике обновляет пользовательский интерфейс с учетом результатов.

> **Обратите внимание** ,  для UWP также существует API [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) и [**ThreadPool**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer) , который можно использовать в подобных сценариях. Дополнительную информацию см. в разделе [Потоки и асинхронное программирование](https://docs.microsoft.com/windows/uwp/threading-async/index).

## <a name="related-topics"></a>Статьи по теме

* [Настраиваемые взаимодействия с пользователем](https://docs.microsoft.com/windows/uwp/design/layout/index)
