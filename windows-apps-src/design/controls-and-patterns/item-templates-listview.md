---
Description: Шаблоны элементов для представления списка
title: Шаблоны элементов для представления списка
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 9328c3f156acd13fd8947e01e924bf0d6849c0a6
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684407"
---
# <a name="item-templates-for-list-view"></a>Шаблоны элементов для представления списка

В этом разделе вы найдете шаблоны элементов, которые можно использовать вместе с элементом управления [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView). Используйте эти шаблоны для создания интерфейса приложений стандартных типов. 

Для демонстрации связывания данных эти шаблоны связывают тип **ListViewItems** с примером класса записи из раздела о [привязке данных](../../data-binding/data-binding-quickstart.md).

> [!NOTE] 
> Если тип **DataTemplate** содержит несколько элементов управления (например, больше, чем один элемент **TextBlock**), специальное имя по умолчанию для средств чтения с экрана можно получить с использованием .ToString() элемента. Для удобства можно задать имя [**AutomationProperties.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties) в корневом элементе шаблона **DataTemplate**. См. подробнее о [специальных возможностях](../accessibility/accessibility-overview.md).

## <a name="single-line-list-item"></a>Однострочный элемент списка
Этот шаблон используется для отображения списка элементов, состоящих из изображения и одной текстовой строки.

![Пример однострочного элемента списка](images/listitems/singlelineexample.png)
![Однострочный элемент списка](images/listitems/singlelineicon.png)
```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="SingleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="44" Padding="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="16" Width="16" VerticalAlignment="Center"/>
                <TextBlock Text="{x:Bind CompositionName}" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" Margin="12,0,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="double-line-list-item"></a>Двухстрочный элемент списка 
Этот шаблон используется для отображения списка элементов, состоящих из изображения и двух текстовых строк.

![Пример двухстрочного элемента списка со значком](images/listitems/doublelineexample.png) 
![Двухстрочный элемент списка со значком](images/listitems/doublelineicon.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="triple-line-list-item"></a>Трехстрочный элемент списка
Этот шаблон используется для отображения списка элементов, состоящих из изображения и трех текстовых строк.

![Пример трехстрочного элемента списка](images/listitems/triplelineexample.png)
![Трехстрочный элемент списка](images/listitems/tripleline.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TripleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="84" Padding="20" AutomationProperties.Name="{x:Bind CompositionName}">
                <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".8" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ReleaseDateTime}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".6" Margin="0,4,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="table-list-item"></a>Табличный элемент списка
Этот шаблон используется для отображения списка элементов, состоящих из распределенного между определенными столбцами текста.

![Пример табличного элемента списка](images/listitems/tablelist.png)
```xaml
<ListView  ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.HeaderTemplate>
        <DataTemplate>
            <Grid Padding="12" Background="{ThemeResource SystemBaseLowColor}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="408"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Composition" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="1" Text="Artist" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="2" Text="Release Date" Style="{ThemeResource CaptionTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.HeaderTemplate>
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TableDataTemplate" x:DataType="local:Recording">
            <Grid Height="48" AutomationProperties.Name="{x:Bind CompositionName}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="48"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <Ellipse Height="32" Width="32" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <TextBlock Grid.Column="1" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Text="{x:Bind CompositionName}" />
                <TextBlock Grid.Column="2" VerticalAlignment="Center" Text="{x:Bind ArtistName}"/>
                <TextBlock Grid.Column="3" VerticalAlignment="Center" Text="{x:Bind ReleaseDateTime}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="related-articles"></a>Похожие статьи
- [Класс ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [Общие сведения о привязке данных](../../data-binding/data-binding-quickstart.md)
- [Общие сведения о специальных возможностях](../accessibility/accessibility-overview.md)
- [Пример элементов ListView и GridView (Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Эскизы](../../files/thumbnails.md)
