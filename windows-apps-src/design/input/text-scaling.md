---
author: Karl-Bridge-Microsoft
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: Масштабирование текста
label: Text scaling
template: detail.hbs
keywords: Отображение UWP, текст, масштабирование, специальные возможности, «специальные возможности», «Больше текста», взаимодействие с пользователем, ввод
ms.author: kbridge
ms.date: 08/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 885ccc89fcbd4315eeed40c3546ef485c515294e
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563709"
---
# <a name="text-scaling"></a>Масштабирование текста

![Пример текста, масштабирования 100% для 225%](images/coretext/text-scaling-news-hero-small.png)  
*Пример текста, масштабирование в Windows 10 (100 225%)*

## <a name="overview"></a>Обзор

Чтение текста на экране компьютера (с мобильного устройства на ноутбуке рабочего стола мониторе огромная экрана Surface Hub) может оказаться сложной задачей для многих пользователей. И наоборот некоторые пользователи находят размеры шрифтов, используемые в приложениях и веб-сайты быть больше, чем необходимо.

Чтобы убедиться, что текст же читаемым, как для широкого круга пользователей, Windows предоставляет возможность для пользователей изменить относительный размер шрифта операционной системы и отдельных приложений. Вместо в приложении Экранная лупа (который обычно просто увеличивает все в пределах области экрана и представляет свой собственный проблем), изменения разрешения экрана или полагаться на масштабирование (который изменяет все зависимости от экрана и обычных просмотра расстояние), пользователь может быстро получить доступ к параметр, чтобы изменить размер только текст, начиная от 100% (размер по умолчанию) до 225%.

## <a name="support"></a>Поддержка

Универсальных приложений для Windows (как стандартные и PWA), поддержка текста, масштабирования по умолчанию.

Если ваше приложение UWP содержит пользовательские элементы управления, поверхности пользовательский текст, жестко управления высоты, более ранние структуры или сторонние платформы, скорее всего необходимо внести некоторые обновления, чтобы обеспечить единообразие и полезные для пользователей.  

DirectWrite, GDI и XAML SwapChainPanels не поддерживают изначально масштабирование текста, если поддержка Win32 ограничено меню, значков и панели инструментов.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Взаимодействие с пользователем

Пользователи могут настроить масштабирования текста с текста "->" больше ползунок на параметры специальных возможностей, "->" концепции и экрана.

![Пример текста, масштабирования 100% для 225%](images/coretext/text-scaling-settings-100-small.png)  
*Параметр из параметров масштабирования текста "->" специальные возможности "->" концепции и экрана*

## <a name="ux-guidance"></a>Руководство по проектированию взаимодействия с пользователем

При изменении размера текста, элементы управления и контейнеров необходимо также изменить размер и адаптировать для размещения текста и его новый макет. Как упоминалось ранее, в зависимости от приложения, инфраструктуры и платформ, большую часть работы выполняется за вас. В следующем руководстве взаимодействия с Пользователем рассматриваются случаи, где не.

### <a name="use-the-platform-controls"></a>Используйте элементы управления платформы

Мы сказали это уже? Стоит Повторение: по возможности всегда используйте встроенных элементов управления с различными структуры приложения Windows для получения возможности наиболее полное взаимодействие с пользователем для наименьшее количество усилий.

Например все элементы управления текстом UWP поддерживают полный текст, масштабирование взаимодействия без настройки или шаблонов.

Ниже приведен фрагмент кода из базового приложения UWP, которое включает в себя несколько стандартных текстовых элементов управления.

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Анимация текста, масштабирования 100% для 225%](images/coretext/text-scaling.gif)  
*Масштабирование анимированного текста*

### <a name="use-auto-sizing"></a>Используйте функция автоматического выбора размера

Не задавайте абсолютные размеры элементов управления. Когда это возможно, позвольте платформе изменить размер элементов управления автоматически в зависимости от параметров пользователей и устройств.  

В этом фрагменте кода из предыдущего примера мы используем `Auto` и `*` значений ширины для набора столбцов сетки и позволить платформе настроить макет приложения, в зависимости от размера элементы, содержащиеся в сетке.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Используйте обтекание текстом

Чтобы убедиться, что макет приложения как гибкость и адаптивность максимально, включите обтекание текстом в любом элементе управления, который содержит текст (многие элементы управления не поддерживают обтекание текстом по умолчанию).

Если вы не укажете обтекание текстом, платформа использует другие методы макет, включая кадрирования (см. предыдущий пример).

Здесь мы используем `AcceptsReturn` и `TextWrapping` свойств TextBox, чтобы наш макет как гибкой.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Анимация текста, масштабирования 100% для 225% с переносом текста](images/coretext/text-scaling-textwrap.gif)  
*Анимация текста, масштабирование с переносом текста*

### <a name="specify-text-trimming-behavior"></a>Укажите поведение обрезки текста

Если обтекание текстом не является предпочтительным поведение, большинство текстовые элементы управления позволяют обрезки текста или указать многоточия для поведения усечения. Обрезка предпочтительнее использовать многоточия, как многоточия занимают место самостоятельно.

> [!NOTE]
> Если вам необходимо сжать текст, обрезки в конце строки, а не в начале.

В этом примере показано, как обрезка текста в элементе TextBlock с помощью свойства [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Текст в масштабе 100% до 225% с обрезка текста](images/coretext/text-scaling-clipping-small.png)  
*Текст, масштабирование с обрезка текста*

### <a name="use-a-tooltip"></a>Используйте всплывающую подсказку

Если обрезка текста, используйте всплывающую подсказку для предоставления пользователям полный текст.

Теперь добавим подсказку к TextBlock, который не поддерживает обтекание текстом:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Не масштабируются значков на основе шрифтов и символов

При использовании значков на основе шрифтов для выделения или оформления, отключение масштабирования на этих символов.

Присвойте свойству [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) `false` для большинства XAML элементов управления.

### <a name="support-text-scaling-natively"></a>Поддержка текста, масштабирования по умолчанию

Обработка события системы UISettings [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) в пользовательской структуры и элементов управления. Это событие вызывается каждый раз, когда пользователь устанавливает коэффициент масштабирования текста в своей системе.

## <a name="summary"></a>Краткий обзор

В этом разделе представлен обзор текст, масштабирования в Windows и включает руководство по взаимодействию с Пользователем и разработчиков о том, как настраивать взаимодействие с пользователем.

## <a name="related-articles"></a>Связанные разделы

### <a name="api-reference"></a>Справочник по API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)