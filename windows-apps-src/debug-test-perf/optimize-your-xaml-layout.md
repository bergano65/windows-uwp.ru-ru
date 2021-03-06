---
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: Оптимизация макета XAML
description: Макет может быть дорогостоящей частью приложения XAML &\#8212; оба в использовании и памяти нагрузку на ЦП. Ниже приведены несколько простых действий, которые можно выполнить для повышения производительности макета приложения XAML.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 92dca27a4cfb02f5d1bcb722683eca89ec16a6d6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362219"
---
# <a name="optimize-your-xaml-layout"></a>Оптимизация макета XAML


**Важные API**

-   [**Панель**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel)

Определение макета — это процесс определения визуальной структуры для пользовательского интерфейса. Базовым механизмом описания макета в XAML является определение с помощью панелей, представляющих собой объекты-контейнеры, в которых можно помещать и соответствующим образом располагать элементы пользовательского интерфейса. Макет может быть затратной частью приложения XAML как в плане загрузки ЦП, так и в плане использования памяти. Ниже приведены несколько простых действий, которые можно выполнить для повышения производительности макета приложения XAML.

## <a name="reduce-layout-structure"></a>Уменьшение структуры макета

Максимального повышения производительности макета можно добиться путем упрощения иерархической структуры дерева элементов пользовательского интерфейса. Панели располагаются в визуальном дереве, однако они являются структурными элементами, а не *элементами, увеличивающими количество пикселей*, как, например, [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) или [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Упрощение дерева путем уменьшения количества элементов, не увеличивающих количество пикселей, как правило, обеспечивает существенное повышение производительности.

Многие пользовательские интерфейсы реализованы путем вложения панелей, что приводит к созданию сложных деревьев панелей и элементов. Вкладывать панели удобно, но во многих случаях точно такой же пользовательский интерфейс можно создать и с помощью одной более сложной панели. Использование только одной панели позволяет повысить производительность.

### <a name="when-to-reduce-layout-structure"></a>Когда следует уменьшать структуру макета

Уменьшение структуры макета обычным способом, например путем удаления одной вложенной панели из страницы верхнего уровня, не даст ощутимого эффекта.

Наиболее значительного увеличения производительности можно добиться за счет уменьшения структуры макета, которая повторяется в пользовательском интерфейсе, например в [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) или [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). Эти элементы [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) используют шаблон [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate), определяющий поддерево элементов пользовательского интерфейса, из которого многократно создаются экземпляры. Если такое же поддерево много раз дублировать в приложении, то любые улучшения производительности окажут мультипликативный эффект на общую производительность приложения.

### <a name="examples"></a>Примеры

Рассмотрите приведенный ниже пользовательский интерфейс.

![Пример макета формы](images/layout-perf-ex1.png)

В этих примерах показаны 3 способа реализации одного пользовательского интерфейса. Каждый вариант реализации обеспечивает примерно одинаковое количество пикселей на экране, но существенно отличается в плане способа реализации.

Вариант 1. Вложенные [ **StackPanel** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) элементов

Хотя это наиболее простая модель, в ней используются элементы с 5 панелями, что приводит к значительным дополнительным затратам.

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

Вариант 2. Один [ **сетки**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)

[  **Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) добавляет определенную сложность, но в этой модели используется элемент с одной панелью.

```xml
<Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

Вариант 3. Один [ **RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel):

Эта модель с одной панелью также немного более сложная, чем модель с использованием вложенных панелей, но она легче для понимания и обслуживания, чем [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid).

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

Как показывают эти примеры, существует много способов создания одного и того же пользовательского интерфейса. При выборе необходимо внимательно рассматривать все факторы, включая производительность, удобочитаемость и возможность поддержки.

## <a name="use-single-cell-grids-for-overlapping-ui"></a>Для накладывающихся пользовательских интерфейсов используйте сетки с одной ячейкой

Общее требование к пользовательскому интерфейсу заключается в том, чтобы в макете элементы накладывались друг на друга. Как правило, для размещения элементов подобным образом используют заполнение, поля, выравнивание и преобразование. Элемент управления XAML [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) оптимизирован для повышения производительности макета с накладывающимися элементами.

**Важные**  для просмотра улучшение используйте одноячейковым [ **сетки**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid). Не задавайте [**RowDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) или [**ColumnDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.columndefinitions).

### <a name="examples"></a>Примеры

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![Текст, наложенный на круг](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![Два текстовых блока в сетке](images/layout-perf-ex3.png)

## <a name="use-a-panels-built-in-border-properties"></a>Используйте свойства встроенной границы панели

[**Сетка**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), [ **StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel), [ **RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), и [  **ContentPresenter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter) элементы управления имеют встроенные свойства, позволяющие рисования границы вокруг них без добавления дополнительного [ **границы** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) элемент для в XAML. Ниже приведены новые свойства, которые поддерживают встроенные границы. **BorderBrush**, **BorderThickness**, **CornerRadius**, и **Padding**. Каждое из этих свойств представляет собой [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty), в связи с чем их можно использовать с привязками и анимациями. Они предназначены для того, чтобы служить полной заменой отдельного элемента **Border**.

Если ваш пользовательский интерфейс включает элементы [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) вокруг этих панелей, используйте вместо них встроенную границу, которая позволит сэкономить место для дополнительного элемента в структуре макета вашего приложения. Как упоминалось ранее, это может обеспечить значительную экономию, особенно в случае повторяемых пользовательских интерфейсов.

### <a name="examples"></a>Примеры

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## <a name="use-sizechanged-events-to-respond-to-layout-changes"></a>Используйте события **SizeChanged** в случае изменения макета

[ **FrameworkElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) класс предоставляет два аналогичных событий за реагирование на изменения макета: [**LayoutUpdated** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) и [ **SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Вы можете использовать одно из этих событий для получения уведомлений об изменении размера элемента во время редактирования макета. Семантика этих двух событий отличается, и при выборе одного из них важно учитывать производительность.

Для обеспечения высокой производительности почти всегда лучше выбирать [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Событие **SizeChanged** имеет интуитивную семантику. Оно возникает во время редактирования макета в случае изменения размера [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement).

[**LayoutUpdated** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) также вызывается во время структурирования, но у него есть глобальный семантику — это событие возникает для каждого элемента при каждом обновлении любого элемента. Это событие, как правило, выбирают только для локальной обработки в обработчике событий; и в этом случае код запускается чаще, чем это необходимо. Используйте **LayoutUpdated** только в том случае, если вам необходимо получать уведомление о перемещении элемента без изменения размера (что происходит нечасто).

## <a name="choosing-between-panels"></a>Выбор панелей

Производительность, как правило, не учитывается при выборе панели. Этот выбор обычно делается на основании того, какая из панелей обеспечивает поведение макета, наиболее близкое к пользовательскому интерфейсу, который вы реализуете. Например, если выбирать между [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) и [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), следует отдать предпочтение той панели, которая обеспечивает наиболее точное сопоставление с задуманной вами моделью реализации.

Каждая панель XAML оптимизирована для обеспечения высокой производительности, и все панели обеспечивают одинаковую производительность для аналогичных пользовательских интерфейсов.

