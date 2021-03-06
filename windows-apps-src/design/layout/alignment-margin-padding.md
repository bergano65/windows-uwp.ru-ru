---
Description: Используйте свойства выравнивания, поля и заполнения, чтобы расположить макет элементов на странице.
title: Выравнивание, поля и заполнение в макетах
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d7f702d145740703b9fbc4ca2e7fd8eba8957cc
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684465"
---
# <a name="alignment-margin-padding"></a>Выравнивание, поле и заполнение

В приложениях UWP большинство элементов пользовательского интерфейса наследуют от класса [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement). Каждый FrameworkElement имеет свойства размера, выравнивания, полей и заполнения, которые влияют на поведение макета. В следующем руководстве представлен обзор способов использования этих свойств макета для того, чтобы пользовательский интерфейс вашего приложения был читаемым и простым в использовании в любом контексте.

## <a name="dimensions-height-width"></a>Измерения (высота, ширина)
Правильная установка размера гарантирует, что все содержимое четко и хорошо читается. У пользователей не должно быть необходимости в прокрутке или масштабировании для расшифровки основного содержимого.

![Схема, показывающая размеры](images/dimensions.svg)

- [**Высота**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) и [**Ширина**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) укажите размер элемента. Значения по умолчанию с точки зрения математики являются NaN (не числами). Можно использовать статические значения, измеренные в [эффективных пикселях](../basics/design-and-ui-intro.md#effective-pixels-and-scaling), функцию **автоматического** выбора размера либо [пропорциональный размер](layout-panels.md#grid) для динамичного поведения.

- [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) и [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) — это свойства только для чтения, которые предоставляют размер элемента во время выполнения. Если гибкие макеты увеличиваются или уменьшаются в размерах, меняются значения в событии [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Обратите внимание, что [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) не меняет значения ActualHeight и ActualWidth.

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) и [**MinHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) указывают значения, которые ограничивают размер элемента, допуская изменение размера.

- Размеры [**шрифта**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) и других свойств текста имеют размер макета элемента управления для текстовых элементов. Хотя текстовые элементы не имеют явно объявленных размеров, у них есть вычисляемые ActualWidth и ActualHeight. 

## <a name="alignment"></a>Соответствие
Выравнивание делает внешний вид пользовательского интерфейса аккуратным и сбалансированным, а также может использоваться для создания визуальной иерархию и связей между элементами.

![схема, показывающая выравнивание](images/alignment.svg)

- [**HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) и [**VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) указывают, как элемент должен располагаться в родительском контейнере.
    - Значения для **HorizontalAlignment**: **Left**, **Center**, **Right** и **Stretch**.
    - Значения для **VerticalAlignment**: **Top**, **Center**, **Bottom** и **Stretch**.

- Значение **Stretch** используется по умолчанию для обоих свойств, при этом элементы занимают все место в соответствии с родительским контейнером. Реальное числовое значение Height и Width отменяет выравнивание Stretch, которое будет действовать как значение Center. Некоторые элементы управления, например Button, перезаписывают значение Stretch в своем стиле по умолчанию.

- [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) и [**VerticalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) указывают, как домещаются дочерние элементы в контейнере.

- Выравнивание может влиять на обрезку в рамках панели макета. Например, при выравнивании `HorizontalAlignment="Left"` правая часть элемента обрезается, если содержимое больше по размеру, чем ActualWidth.

- Текстовые элементы используют свойство [**TextAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment). Как правило, рекомендуется использовать выравнивание по левому краю, которое является значением по умолчанию. Дополнительные сведения об оформлении текста см. в разделе [Оформление текста](../style/typography.md).

## <a name="margin-and-padding"></a>Поле и заполнение
Свойства полей и заполнения помогают избежать перегруженности или излишней разреженности пользовательского интерфейса, а также упростить использование некоторых типов ввода, таких как перо и сенсорный ввод. На рисунке ниже показаны поля и заполнение для контейнера и его содержимого.

![схема полей и заполнения XAML](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>Margin
[**Поле**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) управляет размером пустого пространства вокруг элемента. Margin не добавляет пиксели к ActualHeight и ActualWidth и не считается частью элемента для проведения проверок и событий ввода источников.

- Значения свойства Margin могут быть универсальными или определенными. При значении `Margin="20"` к элементу будет применено универсальное поле в 20 пикселей слева, сверху, справа и снизу. При значении `Margin="0,10,5,25"` значения применяются слева, сверху, справа и снизу (в указанном порядке). 

- Поля можно добавить. Если каждый из двух элементов определяет ширину поля в 10 пикселей, и они расположены по соседству в любой ориентации, то расстояние ними составит 20 пикселей.

- Допустимы отрицательные поля. Однако использование отрицательного поля может вызвать обрезку и наползание соседних элементов друг на друга, поэтому отрицательные поля обычно не используют.

- Значения полей ограничиваются последними, поэтому будьте осторожны с полями, так как контейнеры могут обрезать элементы или ограничивать их размер. Значение Margin может стать причиной того, что элемент не будет поддаваться отрисовке, поскольку одно из его измерений было ограничено до 0 после применения поля.

### <a name="padding"></a>Заполнение
[**Заполнение**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) управляет объемом пространства между внутренней границей элемента и его дочерним содержимым или элементами. Положительное значение Padding уменьшает область содержимого элемента. 

В отличие от Margin Padding не является свойством FrameworkElement. Существует несколько классов, определяющих собственное свойство Padding:

-   [**Control. Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding): наследуется на все производные от [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls) классы. Не во всех элементах управления имеется содержимое, поэтому для таких элементов задание свойства ни на что не влияет. Если элемент управления имеет границу, заполнение применяется внутри этой границы.
-   [**Border. Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding): определяет пробел между линией прямоугольника, созданной с помощью [**BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) и [**дочернего**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child) элемента.
-   [**ItemsPresenter. Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding): участвует в отображении элементов элементов управления "элемент", помещая заданное заполнение вокруг каждого элемента.
-   [**TextBlock. Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) и [**RichTextBlock. Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding): разверните ограничивающий прямоугольник вокруг текста текстового элемента. Эти текстовые элементы не имеют параметра **Background**, поэтому его визуально трудно различить. По этой причине лучше использовать параметры [**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) для контейнеров [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block).

В каждом из этих случаев элементы также имеют свойство Margin. Если одновременно используются свойства Margin и Padding, они дополняют друг друга: очевидное расстояние между внешним контейнером и внутренним содержимым складывается из ширины поля и заполнения.

### <a name="example"></a>Пример
Рассмотрим влияние параметров Margin и Padding на реальные элементы управления. Вот параметр TextBox внутри Grid со значениями параметров Margin и Padding по умолчанию, равными 0.

![TextBox с полем и заполнением 0](images/xaml-layout-textbox-no-margins-padding.svg)

Вот те же самые параметры TextBox и Grid со значениями параметров Margin и Padding в TextBox, как показано в этом коде XAML.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="16,24"/>
</Grid>
```

![Параметр TextBox с положительными значениями поля и заполнения](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>Ресурсы стиля
Вам не придется задавать значение свойства по отдельности для элемента управления. Обычно такой способ более эффективен для группировки значений свойств в ресурс [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) и применения Style к элементу управления. Это особенно актуально, когда необходимо применить одинаковые значения свойства к многочисленным элементам управления. Подробнее об использовании стилей см. в разделе [Стили XAML](../controls-and-patterns/xaml-styles.md).

## <a name="general-recommendations"></a>Общие рекомендации
- Применяйте значения размеров только к определенным ключевым элементам, а для других элементов используйте поведение гибкого макета. Это обеспечивает [быстрый отклик интерфейса](responsive-design.md) при изменении ширины окна.

- При использовании значений размеров **все размеры, поля и заполнения следует устанавливать шагами по 4 эффективных пикселя**. Если UWP использует [эффективные пиксели и масштабирование](../basics/design-and-ui-intro.md#effective-pixels-and-scaling), чтобы сделать интерфейс вашего приложения разборчивым на всех устройствах и размера экрана, он масштабирует элементы пользовательского интерфейса в величинах, кратных 4. Использование значений с шагом 4 приводит к улучшению качества отрисовки за счет выравнивания с целыми пикселями.

- При небольшой ширине окна (менее 640 пикселей) рекомендуется использовать внутренние поля в 12 эффективных пикселей, а при большой — 24 эффективных пикселя.

![рекомендуемые внутренние поля](images/12-gutter.svg)

## <a name="related-topics"></a>Связанные темы
* [**FrameworkElement. Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement. Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement. HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement. VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement. Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Элемент управления. Заполнение**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
