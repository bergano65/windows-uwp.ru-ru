---
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: Использование инклинометра
description: Узнайте, как использовать инклинометр для определения поворотов относительно поперечной, продольной и вертикальной осей.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 15ea49ea0e8e334158000248caf26f662ee5bd35
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369647"
---
# <a name="use-the-inclinometer"></a>Использование инклинометра


**Важные API**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**инклинометра**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer)

**Пример**

-   Более полные сведения о внедрении см. в [примере с уклономером](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer).

Узнайте, как использовать инклинометр для определения поворотов относительно поперечной, продольной и вертикальной осей.

В некоторых трехмерных играх инклинометр используется в качестве устройства ввода. Простой пример — авиационный тренажер, в котором три оси инклинометра (X, Y и Z) сопоставлены командам управления рулем высоты, элероном и рулем направления самолета.

 ## <a name="prerequisites"></a>предварительные требования

Вы должны быть знакомы с расширяемого приложения разметки языка (XAML), Microsoft Visual C#и события.

Используемые вами устройство или эмулятор должны поддерживать инклинометр.

 ## <a name="create-a-simple-inclinometer-app"></a>Создание простого приложения инклинометра

Данный раздел состоит из двух подразделов. В первом подразделе описаны необходимые этапы создания простого приложения инклинометра с нуля. В следующем подразделе описано приложение, которое вы только что создали.

###  <a name="instructions"></a>Инструкция

-   Для создания нового проекта выберите **Пустое приложение (универсальное приложение Windows)** из шаблонов проектов **Visual C#** .

-   Откройте файл проекта MainPage.xaml.cs и замените существующий код следующим.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core;
    using Windows.Devices.Sensors;


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Inclinometer _inclinometer;

            // This event handler writes the current inclinometer reading to
            // the three text blocks on the app' s main page.

            private async void ReadingChanged(object sender, InclinometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    InclinometerReading reading = e.Reading;
                    txtPitch.Text = String.Format("{0,5:0.00}", reading.PitchDegrees);
                    txtRoll.Text = String.Format("{0,5:0.00}", reading.RollDegrees);
                    txtYaw.Text = String.Format("{0,5:0.00}", reading.YawDegrees);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _inclinometer = Inclinometer.GetDefault();


                if (_inclinometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _inclinometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _inclinometer.ReportInterval = reportInterval;

                    // Establish the event handler
                    _inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, InclinometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Имя пространства имен в предыдущем фрагменте нужно заменить именем вашего проекта. Например, если вы создали проект под названием **InclinometerCS**, необходимо заменить `namespace App1` на `namespace InclinometerCS`.

-   Откройте файл MainPage.xaml и замените исходное содержимое следующим XML-кодом.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

Первую часть имени класса из предыдущего фрагмента нужно заменить на пространство имен вашего приложения. Например, если вы создали проект под названием **InclinometerCS**, необходимо заменить `x:Class="App1.MainPage"` на `x:Class="InclinometerCS.MainPage"`. Кроме того, необходимо заменить `xmlns:local="using:App1"` на `xmlns:local="using:InclinometerCS"`.

-   Нажмите клавишу F5 или выберите **Отладка** > **Начать отладку**, чтобы выполнить сборку, развернуть и запустить приложение.

После запуска приложения можно изменить значения инклинометра, перемещая устройство или используя средства эмулятора.

-   Остановите приложение, вернувшись в Visual Studio и нажав клавиши Shift+F5 или выбрав **Отладка** > **Остановить отладку**, чтобы остановить приложение.

###  <a name="explanation"></a>Объяснение

Из предыдущего примера видно, какой небольшой объем кода требуется написать, чтобы включить в ваше приложение обработку входных данных инклинометра.

Приложение устанавливает связь с инклинометром по умолчанию в методе **MainPage**.

```csharp
_inclinometer = Inclinometer.GetDefault();
```

Приложение устанавливает интервал передачи данных в методе **MainPage**. Этот код позволяет получить значение минимально допустимого для данного устройства интервала и сравнить его с требуемым интервалом в 16 миллисекунд (что приблизительно соответствует частоте обновления 60 Гц). Если минимально допустимый интервал больше требуемого, то код задает значение интервала, равное минимальному. В противном случае задается значение интервала, равное необходимому.

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

Новые данные от инклинометра принимаются в методе **ReadingChanged**. Каждый раз, когда драйвер датчика получает от датчика новые данные, он передает их вашему приложению с помощью этого обработчика событий. Приложение регистрирует этот обработчик событий в следующей строке.

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer,
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

Новые значения записываются в элементы TextBlock в XAML-коде проекта.

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```

