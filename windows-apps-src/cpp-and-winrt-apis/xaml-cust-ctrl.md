---
author: stevewhims
description: В этом разделе описана этапы создания простого пользовательского элемента управления с помощью C + +/ WinRT. Вы можете создать данные для создания собственных функциональных возможностей и настраиваемых элементов управления пользовательского интерфейса.
title: (XAML шаблонных) элементов управления с помощью C + +/ WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, стандартная, c ++, cpp, winrt, проекция, XAML, пользовательский, шаблон, элемент управления
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "2917313"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>(XAML шаблонных) элементов управления с помощью [C + +/ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Одним из наиболее важных возможностей универсальной платформы Windows (UWP) является гибкость, что в стеке пользовательского интерфейса (UI) для создания пользовательских элементов управления, в зависимости от типа XAML [элемента управления](/uwp/api/windows.ui.xaml.controls.control) . Платформа пользовательского интерфейса XAML предоставляет функции, такие как [пользовательские свойства зависимостей](/windows/uwp/xaml-platform/custom-dependency-properties) и присоединенных свойств и [шаблоны элементов управления](/windows/uwp/design/controls-and-patterns/control-templates), которые упрощают создание многофункциональных и настраиваемых элементов управления. В этом разделе описана процедура инструкции по созданию пользовательского элемента управления (шаблона) с помощью C + +/ WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Создайте пустое приложение (BgLabelControlApp)
Начните с создания нового проекта в Microsoft Visual Studio. Создание **пустое приложение Visual C++ (C + +/ WinRT)** проект и назовите его *BgLabelControlApp*.

Мы создадим новый класс, представляющий пользовательского элемента управления (шаблона). Класс создается и используется в рамках одной и той же единицы компиляции. Но мы хотим иметь возможность создавать экземпляры этот класс из разметки XAML, поэтому, это будет представлять собой класс среды выполнения. Кроме того, мы применим C++/WinRT для его создания и использования.

Первый шаг при создании нового класса среды выполнения — добавление в проект нового элемента **файла Midl (.idl)**. Назовите его `BgLabelControl.idl`. Удалите содержимое `BgLabelControl.idl` по умолчанию вставьте в это объявление класса среды выполнения.

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

Выше показан шаблон, которому необходимо следовать при объявлении свойства зависимостей (DP). Существует два значения для каждого DP. Во-первых необходимо объявить статическое свойство только для чтения типа [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty). Он имеет имя вашего DP, а также *Свойства*. Это статическое свойство будем использовать в вашей реализации. Во-вторых вы объявляете свойство экземпляра чтения и записи с помощью типа и имя вашего DP.

> [!NOTE]
> Если вы хотите DP с типом с плавающей запятой, сделать ее `double` (`Double` в [MIDL 3.0](/uwp/midl-3/)). Объявления и реализации DP типа `float` (`Single` в MIDL), и затем задать значение для этого DP в разметке XAML, приводит к ошибке *не удалось создать «Windows.Foundation.Single» из текста "<NUMBER>"*.

Сохраните файл и выполните сборку проекта. Во время сборки запускается инструмент `midl.exe` для создания файла метаданных среды выполнения Windows (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`), описывающего класс среды выполнения. Затем запускается средство `cppwinrt.exe` для создания файлов исходного кода для поддержки создания и использования вашего класса среды выполнения. Эти файлы включают заглушки для начала реализации класса среды выполнения **BgLabelControl** , объявленного в вашем IDL. Это заглушки `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` и `BgLabelControl.cpp`.

Скопируйте файлы заглушек `BgLabelControl.h` и `BgLabelControl.cpp` из `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` в папку проекта `\BgLabelControlApp\BgLabelControlApp\`. Убедитесь, что в **Обозревателе решений** включена функция **Показать все файлы**. Щелкните правой кнопкой мыши скопированные файлы заглушек и выберите **Включить в проект**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Реализуйте класс пользовательского элемента управления **BgLabelControl**
Теперь давайте откроем `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` и `BgLabelControl.cpp` и реализуем класс среды выполнения. В `BgLabelControl.h`, измените конструктор, чтобы задать ключ стиля по умолчанию, реализовать **метку** и **LabelProperty**, добавить обработчик статических событий с именем **OnLabelChanged** для обработки изменений значения свойства зависимостей и добавьте частный член для хранения резервное поле для **LabelProperty**.

В этом пошаговом руководстве мы не используем **OnLabelChanged**. Но есть таким образом, можно узнать, как зарегистрировать свойство зависимостей с помощью обратного вызова при изменении свойства.

После их добавления ваш `BgLabelControl.h` выглядит следующим образом.

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

В `BgLabelControl.cpp`, определите статические члены следующим образом.

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>Проектирование стиль по умолчанию **BgLabelControl**

В его конструктор **BgLabelControl** устанавливает ключ стиля по умолчанию для себя. Но какие *— это* стиль по умолчанию? (Шаблонного) пользовательского элемента управления необходимо иметь стиль по умолчанию&mdash;с шаблоном элемента управления по умолчанию&mdash;которой его можно использовать для отрисовки с на случай, если потребитель элемента управления не задать стиль и шаблон. В этом разделе мы добавим файла разметки в проект, содержащий наших стиль по умолчанию.

В узле проекта создайте новую папку и назовите его «Темы». В разделе `Themes`, добавить новый элемент типа **Visual C++** > **XAML** > **Представление XAML**и назовите его «Generic.xaml». Имена папок и файлов должны быть следующим образом в порядке для платформы XAML найти стиль по умолчанию для пользовательского элемента управления. Удалите содержимое по умолчанию `Generic.xaml`и вставьте в разметке ниже.

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

В этом случае единственным свойством, которое задает стиль по умолчанию — это шаблон элемента управления. Шаблон состоит из квадрат (которого фона привязано к свойству **фона** , которой все экземпляры типа XAML [элемента управления](/uwp/api/windows.ui.xaml.controls.control) ) и текстовый элемент (текст которого привязано к свойству зависимостей **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Добавьте экземпляр **BgLabelControl** на главную страницу пользовательского интерфейса

Откройте файл `MainPage.xaml`, который содержит разметку XAML для главной страницы пользовательского интерфейса. Сразу после элемента **Button** (внутри **StackPanel**) добавьте следующую разметку.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Кроме того, добавьте следующий директиву для `MainPage.h` таким образом, учитывать тип пользовательского элемента управления **BgLabelControl** типа **MainPage** (сочетание компиляции разметки XAML и императивном коде).

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Выполните сборку и запуск проекта. Вы увидите, что шаблон элемента управления по умолчанию — это привязка кисть фона и метку, экземпляра **BgLabelControl** в разметке.

В этом пошаговом руководстве показали простой пример пользовательского элемента управления (шаблона) в C + +/ WinRT. Вы можете сделать собственные пользовательские элементы управления произвольно полнофункциональной и полнофункциональных. Например пользовательский элемент управления может занять форме что-то же сложная, как редактируемых данных сетки, проигрыватель видео или визуализатор трехмерной геометрии.

## <a name="important-apis"></a>Важные API
* [Элемент управления](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>Еще по теме
* [Шаблоны элементов управления](/windows/uwp/design/controls-and-patterns/control-templates)
* [Пользовательские свойства зависимостей](/windows/uwp/xaml-platform/custom-dependency-properties)