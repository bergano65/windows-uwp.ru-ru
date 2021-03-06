---
Description: Уведомления на отслеживаемых плитках позволяют узнать, что было изображено на живой плитке приложения, когда пользователь щелкнул ее.
title: Уведомления на отслеживаемых плитках
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: windows 10, uwp, отслеживаемые плитки, живые плитки, уведомления на отслеживаемых плитках
ms.localizationpriority: medium
ms.openlocfilehash: 6e27dec0e7256cfc035ecc3150bd976f69743fe3
ms.sourcegitcommit: f15cf141c299bde9cb19965d8be5198d7f85adf8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2019
ms.locfileid: "58358619"
---
# <a name="chaseable-tile-notifications"></a>Уведомления на отслеживаемых плитках

Уведомления на отслеживаемых плитках позволяют определить, какие уведомления на плитках отображались на живой плитке приложения, когда пользователь щелкнул плитку.  
Например, приложение новостей могло использовать эту функцию, чтобы определить, какая новость отображалась на живой плитке при запуске приложения; можно также настроить заметное отображение новости, чтобы пользователю было легко ее найти. 

> [!IMPORTANT]
> **Требует обновления Годовщина**: Для использования уведомлений chaseable плитки с помощью C#, C++ или VB-универсальной платформы Windows приложения, должен быть предназначен для пакета SDK для 14393 и выполняться сборка 14393 или более поздней версии. Для приложений UWP на основе JavaScript необходимо выбрать целевой пакет SDK 17134 и использовать сборку 17134 или более поздней версии. 


> **Важные API**: [LaunchActivatedEventArgs.TileActivatedInfo property](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [TileActivatedInfo class](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Принцип работы

Чтобы включить уведомления на отслеживаемых плитках, необходимо использовать свойство **Arguments** в полезных данных уведомления на плитке, аналогичное свойству launch в полезных данных всплывающего уведомления, позволяющее вставить сведения о содержимом в уведомление на плитке.

Если приложение запускается через живую плитку, система возвращает список аргументов из отображаемых в настоящее время или недавно уведомлений на плитках.


## <a name="when-to-use-chaseable-tile-notifications"></a>Когда использовать уведомления на отслеживаемых плитках

Уведомления на отслеживаемых плитках, как правило, используются, когда вы используете в живой плитке очередь уведомлений (то есть по очереди отображаете 5 разных уведомлений). Эта функция полезна и в случае, если содержимое на живой плитке может быть не синхронизировано с актуальным содержимым приложения. Так, приложение "Новости" обновляет свою живую плитку каждые 30 минут, но при запуске приложения загружаются актуальные новости (в которых может отсутствовать информация, отображаемая на плитке с последнего интервала опроса). В этом случае пользователь может быть разочарован невозможностью найти новость, которая отображалась на живой плитке. Для этого и предназначены уведомления на всплывающих плитках: они позволяют легко находить то, что пользователь видел на плитке раньше.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Что делать с уведомлениями на отслеживаемых плитках

Важно отметить, что в большинстве сценариев **НЕ следует переходить непосредственно к конкретному уведомлению**, отображавшемуся на плитке, когда ее щелкнул пользователь. Живая плитка используется как точка входа в приложение. Когда пользователь щелкает на живых иконок может быть два сценария: (1) нужно было запустить приложение, как правило, или (2) они хотели посмотреть дополнительные сведения об определенных уведомлений на живой плитки. Поскольку у пользователя нет способа явно заявить о том, какое из двух поведений ему необходимо, в идеале **нужно запускать приложение в обычном режиме и предоставлять пользователю возможность легко найти нужное уведомление**.

Например, при нажатии на живую плитку приложения MSN Новости приложение запускается в обычном режиме: отображается домашняя страница или статья, которую пользователь начал читать ранее. Однако на домашней странице приложения легко найти новости с живой плитки. Это обеспечивает поддержку обоих сценариев: сценария, в котором вам просто нужно запустить приложение или продолжить его работу; и сценария, где требуется просмотреть определенную новость.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Как включить свойство Arguments в полезные данные уведомления на плитке

В полезных данных уведомления свойство аргументов позволяет приложению предоставить данные, которые можно использовать для поиска уведомления в дальнейшем. Так, свойство arguments может включать идентификатор новости, чтобы при запуске можно было быстро найти и отобразить ее. Свойство принимает строку, которую можно сериализировать удобным вам способом (строка запроса, JSON и т. д.), однако, как правило, Рекомендуется использовать формат строки запроса, поскольку это облегченная сущность, которая хорошо поддерживает кодировку XML.

Это свойство можно задать для элементов **TileVisual** и **TileBinding** и распространить каскадом на элементы на нижних уровнях. Если требуется использовать одинаковое свойство arguments для плиток любого размера, просто задайте для этого свойства значение **TileVisual**. Если для плиток определенных размеров требуется определенное свойство arguments, можно задать свойство arguments для отдельных элементов **TileBinding**.

В этом примере создаются полезные данные уведомления, в которых используется свойство arguments, чтобы уведомление было легко найти в дальнейшем. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>Как проверить свойство arguments при запуске приложения

Большинство приложений содержат файл App.xaml.cs с переопределением метода [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_). Как становится понятно из названия, приложение вызывает этот метод при запуске. Оно принимает один аргумент, объект [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs).

Объект LaunchActivatedEventArgs имеет свойство, позволяющее использовать отслеживаемые уведомления: свойство [TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), предоставляющее доступ к объекту [TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo). Когда пользователь запускает приложение из плитки (а не из списка приложений, поиска или любой другой точки входа), приложение инициализирует это свойство.

Объект [TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) содержит свойство [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), которое в свою очередь содержит список уведомлений, отображавшихся на плитке за последние 15 минут. Первый элемент в списке представляет уведомление, которое отображается на плитке в настоящее время, а последующие — уведомления, которые пользователь видел раньше. Если плитка очищена, список пуст.

Каждый ShownTileNotification имеет свойство аргументы. Свойство Arguments инициализируются строкой аргументы из плитки полезные данные уведомления или значение null, если полезных данных не содержит строку аргументов.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


### <a name="accessing-onlaunched-from-desktop-applications"></a>Доступ к OnLaunched от классических приложений

Классические приложения (например Win32, WPF, и т.д.) с помощью [моста для классических](https://developer.microsoft.com/windows/bridges/desktop), можно также использовать chaseable плитки! Единственным различием является доступ к OnLaunched аргументы. Обратите внимание, что сначала необходимо [упаковки приложения с помощью Desktop Bridge](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root).

> [!IMPORTANT]
> **Требуется обновление октября 2018**: Чтобы использовать `AppInstance.GetActivatedEventArgs()` API, должны быть предназначены SDK 17763 и выполняться сборка 17763 или более поздней версии.

Для классических приложений для доступа к аргументам запуска, выполните следующие действия...

```csharp

static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // API only available on build 17763 or higher
    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:

            var launchArgs = args as LaunchActivatedEventArgs;

            // If clicked on from tile
            if (launchArgs.TileActivatedInfo != null)
            {
                // If tile notification(s) were present
                if (launchArgs.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
                {
                    // Get arguments from the notifications that were recently displayed
                    string[] allTileArgs = launchArgs.TileActivatedInfo.RecentlyShownNotifications
                    .Select(i => i.Arguments)
                    .ToArray();
     
                    // TODO: Highlight each story in the app
                }
            }
    
            break;
```


## <a name="raw-xml-example"></a>Пример: необработанный XML

Если вместо библиотеки уведомлений используется необработанный XML, см. этот пример XML-кода.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>Связанные статьи

- [LaunchActivatedEventArgs.TileActivatedInfo property](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [Класс TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)
