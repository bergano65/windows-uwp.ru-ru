---
Description: Используется для выбора или отмены выбора элементов действий. Можно использовать для одного элемента списка или для нескольких элементов списка.
title: Флажки
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c69c5e64d75e03ce5bb2ef0d82c3b816fe78a770
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339420"
---
# <a name="check-boxes"></a>Флажки

 

Флажок используется для выбора или отмены выбора элементов действий. Его можно использовать для одного элемента или списка из нескольких элементов с возможностью выбора. Элемент управления предусматривает три состояния выделения: "не выбрано", "выбрано" и "не определено". Состояние "не определено" используется, когда в подсписке вариантов есть одновременно состояния "не выбрано" и "выбрано".

> **Важные API**: [CheckBox class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox), [Checked event](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) и [IsChecked property](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)

![Пример состояний флажка](images/templates-checkbox-states-default.png)


## <a name="is-this-the-right-control"></a>Выбор правильного элемента управления

Используйте **один флажок** для выбора из двух вариантов "да"/"нет", например в сценарии входа с запросом "Запомнить меня" или при подтверждении условий соглашения на обслуживание.

![Один флажок используется для отдельного выбора](images/checkbox1.png)

Основное различие между **флажком** и [переключателем](toggles.md) при двоичном выборе состоит в том, что флажок предназначен для обозначения состояния, а переключатель — для обозначения действия. Действие по флажку можно отложить (например, как часть отправки формы), тогда как действие по переключателю требуется выполнить сразу. Кроме того, множественный выбор можно сделать только с помощью флажков.

Используйте **несколько флажков** в сценариях с множественным выбором, когда пользователь может выбрать один или несколько элементов группы, которые не исключают друг друга.

Создавайте группу флажков, когда пользователи могут выбирать любое сочетание вариантов.

![Выбор нескольких вариантов с помощью флажков](images/checkbox2.png)

Если варианты можно сгруппировать, вы можете использовать флажок с неопределенным состоянием для представления всей группы. Неопределенное состояние флажка используется, когда пользователь выбирает лишь некоторые (не все) подэлементы в группе.

![Флажки, используемые для обозначения смешанного выбора](images/checkbox3.png)

С помощью элементов управления **флажок** и **переключатель** пользователь может выбирать варианты из списка. Флажки позволяют пользователю выбирать сочетание вариантов. В отличие от них, при помощи переключателей пользователь может выбрать один из взаимоисключающих вариантов. Если есть несколько вариантов, но выбрать можно только один из них, используйте переключатель.

## <a name="examples"></a>Примеры

<table>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Если у вас установлено приложение <strong style="font-weight: semi-bold">галереи элементов управления XAML</strong>, щелкните здесь, чтобы <a href="xamlcontrolsgallery:/item/CheckBox">открыть приложение и увидеть CheckBox в действии</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Получить приложение XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Получить исходный код (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>Создание флажка

Чтобы присвоить флажку метку, установите свойство [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content). Метка отображается рядом с флажком.

Этот код XAML позволяет создать один флажок, который используется для принятия условий соглашения перед отправкой формы. 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

Вот код для создания того же флажка.

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### <a name="bind-to-ischecked"></a>Привязка к свойству IsChecked

Используйте свойство [IsChecked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked), чтобы определять, установлен ли флажок или снят. Вы можете привязать значение свойства IsChecked к другому двоичному значению. Однако из-за того, что свойство IsChecked имеет логическое значение, [допускающее значение null](https://docs.microsoft.com/dotnet/api/system.nullable-1), для его привязки к логическому значению вам потребуется преобразователь величин.

В этом примере свойство **IsChecked** флажка для принятия условий соглашения привязано к свойству [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) кнопки "Отправить". Кнопка "Отправить" включена, только если пользователь принял условия соглашения.

> Примечание&nbsp;&nbsp;Здесь приводится только соответствующий код. Дополнительные сведения о привязке данных и преобразователях величин можно найти в разделе [Общие сведения о привязке данных](../../data-binding/data-binding-quickstart.md).

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```

```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### <a name="handle-click-and-checked-events"></a>Обработка событий Click и Checked

Чтобы выполнять действие при изменении состояния флажка, вы можете обрабатывать событие [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) или события [Checked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) и [Unchecked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked). 

Событие **Click** происходит при изменении состояния Checked При обработке события Click используйте свойство **IsChecked**, чтобы определить состояние флажка.

События **Checked** и **Unchecked** происходят независимо друг от друга. Следует обрабатывать оба этих события, чтобы реагировать на изменения состояния флажка.

В следующих примерах мы продемонстрируем обработку события Click, а также событий Checked и Unchecked.

Несколько флажков могут совместно использовать один и тот же обработчик событий. В этом примере будет создано 4 флажка для выбора соуса для пиццы. Эти четыре флажка используют один и тот же обработчик событий **Click**, чтобы обновлять список выбранных соусов.

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

Вот обработчик события Click. При каждом нажатии на флажок обработчик проверяет, какие флажки установлены, и обновляет список выбранных соусов.

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### <a name="use-the-indeterminate-state"></a>Использование неопределенного состояния

Элемент управления CheckBox наследует от [ToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) и может иметь три состояния: 

Состояние | Свойство | Значение
------|----------|------
установлен | IsChecked | **true** 
снят | IsChecked | **false** 
не определен | IsChecked | **null** 

Чтобы флажок сообщал о неопределенном состоянии, установите для свойства [IsThreeState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.isthreestate) значение **true**. 

Если варианты можно сгруппировать, вы можете использовать флажок с неопределенным состоянием для представления всей группы. Неопределенное состояние флажка используется, когда пользователь выбирает лишь некоторые (не все) подэлементы в группе.

В следующем примере свойству IsThreeState флажка "Выбрать все" задано значение **true**. Флажок "Выбрать все" устанавливается, если установлены все дочерние элементы, снимается, если сняты все дочерние элементы, и не определен в остальных случаях.

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## <a name="dos-and-donts"></a>Возможности и ограничения

-   Убедитесь, что цель применения флажка и его текущее состояние понятны.
-   Длина текстового содержимого, связанного с флажком, не должна превышать двух строк
-   Формулируйте подпись к флажку как инструкцию, для которой установка флажка соответствует значению "истина", а снятие флажка обозначает "ложь".
-   Используйте шрифт по умолчанию, если указания для торговой марки не требуют использования другого шрифта.
-   Если применяется динамическое текстовое содержимое, подумайте, как будет изменяться размер элемента управления и что будет происходить с окружающими его визуальными элементами.
-   Если выбирать нужно из двух или более взаимоисключающих вариантов, обдумайте использование [переключателей](radio-button.md).
-   Не следует размещать две группы флажков рядом друг с другом. Используйте метки групп, чтобы разделить группы.
-   Для элемента управления типа "включено-выключено" используйте переключатель, а не флажок.
-   Не используйте флажок для отображения других элементов управления, например диалоговых окон.
-   Используйте неопределенное состояние, чтобы показать, что выбранный вариант задан не для всех дочерних элементов.
-   При использовании неопределенного состояния применяйте подчиненные флажки, чтобы показать, какие варианты выбраны, а какие нет. Разрабатывайте пользовательский интерфейс так, чтобы пользователь мог видеть доступные для выбора дочерние элементы.
-   Не используйте неопределенное состояние для представления третьего состояния. Неопределенное состояние используется, чтобы показать, что выбраны некоторые, но не все дочерние объекты. Поэтому не разрешайте пользователям напрямую устанавливать неопределенное состояние. Пример того, как не надо делать: в этом флажке неопределенное состояние используется для того, чтобы обозначить среднее количество специй:

    ![Флажок с неопределенным состоянием](images/checkbox4_spicy.png)

    Вместо этого используйте переключатель с тремя вариантами: "Без специй", "Со специями" и "Много специй".

    ![Переключатель с тремя вариантами: "Без специй", "Со специями" и "Много специй".](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>Получить пример кода

- [Пример из коллекции элементов управления XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) — ознакомьтесь со всеми элементами управления XAML в интерактивном режиме.

## <a name="related-articles"></a>Связанные статьи

- [Класс CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 
- [Переключатели](radio-button.md)
- [Тумблер](toggles.md)
