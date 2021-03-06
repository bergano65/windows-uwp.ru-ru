---
description: В этой статье объясняется, как обеспечить поддержку контракта отправки данных в приложении универсальной платформы Windows (UWP).
title: Предоставление общего доступа к данным
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 08dbe9ed7aaa732172d488712aa47d6d3631508a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317706"
---
# <a name="share-data"></a>Предоставление общего доступа к данным


В этой статье объясняется, как обеспечить поддержку контракта отправки данных в приложении универсальной платформы Windows (UWP). Контракт отправки данных — это простой способ, которым приложения могут быстро предоставлять друг другу общий доступ к данным, например к тексту, ссылкам, фотографиям и видео. Например, пользователю может быть необходимо поделиться со своими друзьями ссылкой на веб-страницу в приложении социальной сети или сохранить ее в приложении для заметок, чтобы вернуться к ней позже.

## <a name="set-up-an-event-handler"></a>Настройка обработчика событий

Добавьте обработчик событий [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested), который будет срабатывать при каждом вызове функции общего доступа пользователем. Это может происходить, когда пользователь касается элемента управления в приложении (например, кнопки или команды на панели приложения), или автоматически в определенном сценарии (если, например, пользователь заканчивает уровень и получает рекордный результат).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

При возникновении события [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) приложение получает объект [**DataRequest**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest). Этот объект содержит объект [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage), который можно использовать, чтобы предоставить содержимое, которым пользователь хочет поделиться. Вы должны указать название и данные, к которым необходимо предоставить общий доступ. Описание необязательно, но рекомендуется.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>Выбор данных

Можно делиться разными типами данных, включая:

-   Обычный текст
-   Универсальные коды ресурсов (URI)
-   HTML
-   Форматированный текст
-   Растровые изображения
-   Файлы
-   Пользовательские данные, определенные разработчиком

Объект [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) может содержать один или несколько указанных форматов в любом сочетании. В примере ниже показано, как предоставить общий доступ к тексту.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>Задание свойств

При подготовке данных пакета к совместному использованию можно установить ряд свойств, дающих дополнительную информацию о содержимом, предназначенном для общего доступа. Благодаря этим свойствам конечное приложение может улучшить взаимодействие с пользователем. Например, предоставление описания может быть полезным, если пользователь предоставляет общий доступ более чем одному приложению. Добавление эскиза при совместном использовании рисунка или ссылки на веб-страницу устанавливает визуальную связь с пользователем. Подробнее об этом см. в разделе [**DataPackagePropertySet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Все свойства, кроме заголовка, не являются обязательными. Свойство заголовка является обязательным и должно быть задано.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>Запуск пользовательского интерфейса общего доступа

Пользовательский интерфейс для общего доступа предоставляется системой. Чтобы запустить его, вызовите метод [**ShowShareUI**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>Обработка ошибок

В большинстве случаев предоставление общего доступа к содержимому — линейный процесс. Тем не менее всегда существует вероятность непредвиденных событий. Например, приложению может потребоваться, чтобы пользователь выбрал содержимое для общего доступа, но при этом пользователь ничего не выбирает. Для обработки таких ситуаций используйте метод [**FailWithDisplayText**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_), который отобразит соответствующее сообщение для пользователя, если что-то пойдет не так.

## <a name="delay-share-with-delegates"></a>Задержка общего доступа с помощью делегатов

Готовить данные для общего доступа сразу не всегда целесообразно. Например, если ваше приложение поддерживает отправку большого файла изображения в нескольких различных графических форматах, то создавать все эти изображения до того, как пользователь сделает свой выбор, малоэффективно.

Чтобы решить эту проблему, [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) может содержать делегат – функцию, которая вызывается, когда принимающее приложение запрашивает данные. Рекомендуется использовать делегат, когда пользователь хочет предоставить общий доступ к ресурсоемким данным.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>См. также 

* [Связь между приложениями](index.md)
* [Получение данных](receive-data.md)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [DataRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 

