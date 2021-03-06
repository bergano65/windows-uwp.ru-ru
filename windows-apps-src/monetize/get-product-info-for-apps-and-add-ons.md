---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Узнайте, как использовать пространство имен Windows.Services.Store, чтобы получить связанные с Магазином сведения о продукте для текущего приложения или одной из его надстроек.
title: Получение информации о продукте для приложений и надстроек
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp, покупки из приложения, IAP, надстройки, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 672435961c689d1596b52d4c96e1d1e91266268c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358790"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Получение информации о продукте для приложений и надстроек

В этой статье рассказывается, как использовать методы класса [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) из пространства имен [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) для получения информации, связанной с Microsoft Store, для текущего приложения или одной из его надстроек.

Полный пример приложения см. в разделе [Пример для Магазина](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Пространство имен **Windows.Services.Store** впервые появилось в Windows 10 версии 1607 и может использоваться только в проектах, предназначенных для **Windows 10 Anniversary Edition (10.0; сборка 14393)** или более поздней версии в Visual Studio. Если приложение предназначено для предыдущих версий Windows 10, необходимо использовать пространство имен [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store), а не пространство имен **Windows.Services.Store**. Дополнительные сведения см. в [этой статье](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Предварительные требования

Для этих примеров необходимо выполнение следующих предварительных условий:
* Создан проект Visual Studio для приложения универсальной платформы Windows (UWP), предназначенный для **Windows 10 Anniversary Edition (10.0; сборка 14393)** и более поздних выпусков.
* У вас есть [создан Отправка приложения](https://docs.microsoft.com/windows/uwp/publish/app-submissions) в центре партнеров и это приложение публикуется в Store. При необходимости можно настроить приложение, чтобы его нельзя было найти в Магазине, пока вы его тестируете. Подробнее см. в нашем [руководстве по тестированию](in-app-purchases-and-trials.md#testing).
* Если вы хотите получить сведения о продукте для надстройки для приложения, необходимо также [Создайте надстройку в центре партнеров](../publish/add-on-submissions.md).

В коде из этих примеров предполагается следующее:
* Код выполняется в контексте страницы [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), которая содержит [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) с именем ```workingProgressRing``` и [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) с именем ```textBlock```. Эти объекты используются для индикации выполнения асинхронной операции и отображения выводимых сообщений, соответственно.
* Файл кода содержит оператор **using** для пространства имен **Windows.Services.Store**.
* Приложение — однопользовательское и выполняется только в контексте пользователя, запустившего его. Подробнее см. в разделе [Покупки из приложения и пробные версии](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Если у вас есть классическое приложение, которое использует [мост для классических приложений](https://developer.microsoft.com/windows/bridges/desktop), вам может потребоваться добавить дополнительный код, не показанный в этих примерах, для настройки объекта [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext). Дополнительные сведения см. в разделе [Использование класса StoreContext в классическом приложение, в котором применяется мост для настольных компьютеров](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Получение информации для текущего приложения

Чтобы получить информацию о продукте Магазина для текущего приложения, используйте метод [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Этот асинхронный метод возвращает объект [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), который можно использовать для получения такой информации, как цена.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Получение информации для надстроек с известными кодами продукта в Store, связанных с текущим приложением

Чтобы получить информацию о продукте в Store для надстроек, связанных с текущим приложением, с уже известными [кодами продукта в Store](in-app-purchases-and-trials.md#store_ids), используйте метод [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Этот асинхронный метод возвращает коллекцию объектов [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), которые представляют каждую из надстроек. Помимо кодов продукта в Магазине, этому методу необходимо передать список строк, которые определяют типы надстроек. Список поддерживаемых строковых значений см. в описании свойства [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Метод **GetStoreProductsAsync** возвращает сведения о продукте для указанных надстроек, которые связаны с приложением, независимо от доступности надстроек покупки в настоящее время. Чтобы получить сведения для всех надстроек для текущего приложения, которые в настоящее время можно приобрести, вместо этого используйте метод **GetAssociatedStoreProductsAsync**, как описано в [следующем разделе](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app).

В этом примере извлекается информация для постоянных надстроек с заданными кодами продукта в Store, которые связаны с текущим приложением.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Получение информации для надстроек, доступных для покупки из текущего приложения

Чтобы получить информацию о продукте Store для надстроек, которые сейчас доступны для покупки из текущего приложения, используйте метод [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync). Этот асинхронный метод возвращает коллекцию объектов [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), которые представляют каждую из доступных надстроек. Этому методу необходимо передать список строк, определяющих типы надстроек, которые требуется извлечь. Список поддерживаемых строковых значений см. в описании свойства [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Если приложение содержит много доступных для приобретения надстроек, можно также использовать метод [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync), чтобы разбивать возвращаемые результаты для надстроек на страницы.

В следующем примере извлекается информации для всех постоянных надстроек, потребляемых надстроек, управляемых Store, и потребляемых надстроек, управляемых разработчиком, которые доступны для покупки из текущего приложения.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Получение информации для надстроек текущего приложения, приобретенных пользователем

Чтобы получить информацию о продукте Store для надстроек, купленных текущим пользователем, используйте метод [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync). Этот асинхронный метод возвращает коллекцию объектов [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), которые представляют каждую из надстроек. Этому методу необходимо передать список строк, определяющих типы надстроек, которые требуется извлечь. Список поддерживаемых строковых значений см. в описании свойства [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Если приложение содержит много надстроек, можно также использовать метод [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync), чтобы разбивать возвращаемые результаты для надстроек на страницы.

В следующем примере извлекается информация для постоянных надстроек с заданными [кодами продукта в Store](in-app-purchases-and-trials.md#store_ids).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>См. также

* [Покупки из приложения и пробные версии](in-app-purchases-and-trials.md)
* [Получить сведения о лицензии для приложений и надстроек](get-license-info-for-apps-and-add-ons.md)
* [Включить покупки из приложений, приложений и надстроек](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Включить покупки готовых к использованию надстройки](enable-consumable-add-on-purchases.md)
* [Реализуйте пробную версию приложения](implement-a-trial-version-of-your-app.md)
* [Пример Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
