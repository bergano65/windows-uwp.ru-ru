---
Description: Предлагают набор готовых к использованию продуктов в приложении &\#8212; элементы, которые можно приобрести, использовать и еще раз приобрели &\#8212; через Store платформа электронной коммерции для предоставления своим клиентам покупку подключаемыми модулями, которое является одновременно надежные и надежность.
title: Поддержка покупки продуктов из приложения
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: UWP, потребляемые, надстройки, покупки из приложения, IAP, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 81c37e915b0efa320b1a2f359c873356ed83b6ba
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371825"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Поддержка покупки продуктов из приложения

Предоставьте пользователям возможность покупки из приложения потребляемых внутренних продуктов приложения (товаров, которые можно покупать, использовать и покупать снова) через Магазин. Покупка из приложения — удобный и надежный способ приобрести товар. Это особенно удобно при покупке виртуальной валюты для игр (например, золота или монет), которую можно потом использовать в процессе игры.

> [!IMPORTANT]
> В этой статье показано, как использовать элементы пространства имен [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) для включения покупок потребляемых продуктов в приложении. Это пространство имен больше не дополняется новыми функциями, и мы рекомендуем вместо него использовать пространство имен [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). **Windows.Services.Store** пространство имен поддерживает последние надстройки типы, например управляемые Store пригодных для использования надстроек и подписки и должна быть совместима с типами будущих продуктов и функций, поддерживаемых партнера Центр и Store. Пространство имен **Windows.Services.Store** впервые появилось в Windows 10 версии 1607 и может использоваться только в проектах, предназначенных для **Windows 10 Anniversary Edition (10.0; сборка 14393)** или более поздней версии в Visual Studio. Дополнительные сведения о включении возможности покупки потребляемых продуктов из приложения с помощью пространства имен **Windows.Services.Store** см. в [этой статье](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>предварительные требования

-   В этом разделе рассказывается о покупках потребляемых внутренних продуктов приложения и отчетах об исполнении сделок в ваших приложениях. Чтобы ознакомиться с продуктами из приложения, см. статью [Поддержка покупок продуктов из приложения](enable-in-app-product-purchases.md), из которой вы узнаете о лицензировании и о том, как правильно вносить продукты из приложения в список Магазина.
-   Когда вы создадите код для продаж внутренних продуктов приложения и будете проверять его в первый раз, используйте объект [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) вместо объекта [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp). В этом случае вы сможете проверить логику лицензирования путем имитации обращения к серверу лицензирования вместо вызова реального сервера. Чтобы сделать это, необходимо настроить файл с именем WindowsStoreProxy.xml в папку % userprofile %\\AppData\\локального\\пакетов\\&lt;имя пакета&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. Имитатор Microsoft Visual Studio создает этот файл при первом запуске приложения. Также можно загрузить собственный его вариант во время выполнения. Дополнительные сведения см. в разделе [Использование файла WindowsStoreProxy.xml с CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   В этом разделе также приведены ссылки на примеры кода из статьи [Пример для Магазина](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Этот пример дает отличную возможность поэкспериментировать с разными вариантами монетизации, доступными для приложений универсальной платформы Windows (UWP).

## <a name="step-1-making-the-purchase-request"></a>Шаг 1. Что делает запрос на покупку

Первоначальный запрос на покупку отправляется с помощью [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), как и для любой покупки из приложения, выполняемой через Магазин. Отличие покупки потребляемых внутренних продуктов приложения состоит в том, что после покупки клиент не сможет снова приобрести тот же товар, пока Магазин не получит уведомления от приложения об успешном исполнении предыдущей покупки. За исполнение покупки потребляемых товаров и уведомление о сделке Магазина отвечает ваше приложение.

В следующем примере показан запрос на покупку потребляемого товара из приложения. В комментариях к коду указано, когда ваше приложение должно осуществить свою часть сделки по приобретению потребляемого внутреннего продукта приложения в двух разных сценариях — при успешном запросе и в случае сбоя запроса, из-за того что предыдущая покупка того же товара не выполнена.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Шаг 2. Отслеживание локального выполнение машинные команды

Предоставляя клиентам доступ к потребляемым внутренним продуктам приложения, важно отслеживать, какой товар поставляется (*productId*) и с какой транзакцией связывается эта поставка (*transactionId*).

> [!IMPORTANT]
> Ваше приложение отвечает за то, чтобы предоставить Магазину точный отчет об исполнении сделки. Это необходимо, чтобы сделка была честной и надежной.

Пример ниже демонстрирует, как использовать свойства [PurchaseResults](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) вызова [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) из предыдущего этапа, чтобы указать, что покупка товара выполнена. Для сохранения сведений о товаре в месте, на которое впоследствии можно будет сослаться, чтобы подтвердить, что приложение выполнило свою часть сделки, используется коллекция.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

Следующий пример показывает, как использовать массив из предыдущего примера, чтобы получить доступ к паре кодов продукта и транзакции, которые в дальнейшем будут использоваться в отчетах о сделке для Магазина.

> [!IMPORTANT]
> Независимо от способа отслеживания и подтверждения сделки ваше приложение должно продемонстрировать добросовестность и доказать, что клиент получил тот товар, за который заплатил.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Шаг 3. Reporting реализации продукта к Store

После выполнения приложением своей части сделки оно должно отправить вызов [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync), содержащий данные о товаре *productId* и соответствующей сделке.

> [!IMPORTANT]
> Если информация о выполненной покупке потребляемого внутреннего продукта приложения не будет передана в Магазин, пользователь не сможет снова купить тот же товар до поступления отчета о выполнении предыдущей сделки.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Шаг 4. Определение невыполненной покупки

С помощью метода [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) приложение может в любой момент проверить невыполненные покупки потребляемых внутренних продуктов приложения. Этот метод следует вызывать регулярно, чтобы проверять наличие покупок потребляемых товаров, которые не были выполнены из-за непредвиденных событий приложения, таких как сетевые проблемы или завершение работы приложения.

Пример ниже показывает, как можно использовать [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) для перечисления потребляемых товаров и как приложение может последовательно проходить по этому списку, чтобы завершить свою часть сделки.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>См. также

* [Поддержка покупки внутренних продуктов приложений](enable-in-app-product-purchases.md)
* [Пример Store (демонстрирует пробные версии и покупки из приложений)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)
 

 
