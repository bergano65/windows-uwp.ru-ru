---
title: Радиус угла
description: Узнайте о принципах скругления углов, подходах к проектированию и параметрах настройки.
ms.date: 10/08/2019
ms.topic: article
keywords: Windows 10, uwp, радиус угла, скругленный
ms.openlocfilehash: 84cd27bf8c65ed65a6ee2b0f044e0ffb3ef86bf0
ms.sourcegitcommit: 49af415e4eefea125c023b7071adaa5dc482e223
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74799928"
---
# <a name="corner-radius"></a>Радиус угла

Начиная с версии 2.2 [библиотеки пользовательского интерфейса Windows](/uwp/toolkits/winui/) (WinUI), стиль по умолчанию для многих элементов управления обновлен для использования скругленных углов. Эти новые стили призваны вызывать энтузиазм и доверие, а также облегчать визуальную обработку интерфейса пользователями.

Здесь представлены два элемента управления "Кнопка": первая без скругленных углов, а вторая — в новом стиле со скругленными углами.

![Кнопки со скругленными углами и без них](images/rounded-corner/my-button.png)

При установке пакета NuGet для WinUI 2.2 или более поздней версии новые стили по умолчанию устанавливаются для элементов управления WinUI и платформ. Эти стили автоматически применяются при использовании WinUI 2.2 в приложении; никаких дополнительных действий для использования новых стилей не требуется. Однако далее в этой статье мы покажем, как настроить скругленные углы, если это необходимо.

> [!IMPORTANT]
> Некоторые элементы управления доступны как на платформе ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)), так и в WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2)); например **TreeView** или **ColorPicker**. При использовании WinUI в приложении следует использовать версию элемента управления WinUI. При использовании с WinUI в версии платформы скругление углов может применяться непоследовательно.

> **Важные API**: [Свойство Control.CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>Макеты элементов управления по умолчанию

Существуют три области элементов управления, в которых используются стили скругленных углов: прямоугольные элементы, всплывающие элементы и элементы панели.

### <a name="corners-of-rectangle-ui-elements"></a>Углы прямоугольных элементов пользовательского интерфейса

- Эти элементы пользовательского интерфейса включают в себя основные элементы управления, такие как кнопки, которые пользователи постоянно видят на экране.
- Значение радиуса по умолчанию, используемое для этих элементов пользовательского интерфейса, — **2 пкс**.

![Кнопка с выделенными скругленными углами](images/rounded-corner/button.png)

**Элементы управления**

- Элемент управления AutoSuggestBox
- Button
  - Кнопки ContentDialog
- CalendarDatePicker
- Флажок
  - Флажки множественного выбора TreeView
- ComboBox
- DatePicker
- DropDownButton
- FlipView.
- PasswordBox
- Элемент управления RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>Углы элементов пользовательского интерфейса всплывающего меню и наложения

- Это могут быть элементы пользовательского интерфейса, которые отображаются на экране временно, например MenuFlyout, или элементы, накладываемые на другой элемент пользовательского интерфейса, такие как вкладки TabView.
- Значение радиуса по умолчанию, используемое для этих элементов пользовательского интерфейса, — **4 пкс**.

![Пример всплывающего элемента](images/rounded-corner/flyout.png)

**Элементы управления**

- CommandBarFlyout
- ContentDialog
- Всплывающий элемент
- MenuFlyout
- вкладки TabView
- TeachingTip
- ToolTip
- Часть всплывающего элемента (при открытии)
  - Элемент управления AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>Элементы панели

- Эти элементы пользовательского интерфейса выглядят как строки или линии; например ProgressBar.
- Используемые по умолчанию значения радиуса — **2 пкс**.

![Пример индикатора выполнения](images/rounded-corner/bars.png)

**Элементы управления**

- Индикатор выбора NavigationView
- Индикатор выбора сводки
- ProgressBar
- ScrollBar (при `IndicatorMode=TouchIndicator`)
- Ползунок
  - Ползунок цвета ColorPicker
  - Ползунок полосы поиска MediaTransportControls

## <a name="customization-options"></a>Параметры настройки

Значения радиусов углов по умолчанию, которые мы предоставляем, не окончательны. Вы можете изменить количество закруглений по углам, используя один из нескольких простых способов. Используйте два глобальных ресурса или свойство [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) непосредственно в элементе управления в зависимости от необходимой степени детализации пользовательской настройки.

### <a name="when-not-to-round"></a>Когда не следует закруглять

Существуют экземпляры, в которых угол элемента управления не должен закругляться, и мы не будем закруглять их по умолчанию.

- Когда несколько элементов пользовательского интерфейса, размещенные внутри контейнера, соприкасаются друг с другом, например две части SplitButton. При взаимодействии не должно быть пробелов.

![SplitButton](images/rounded-corner/split-button-2.png)

- Когда элемент управления находится в другом контейнере, например, в ScrollBar и кнопках, которые являются частью контейнера ScrollBar, также являющегося частью ScrollViewer.

![ScrollBar](images/rounded-corner/scrollbar.png)

- Если всплывающий элемент пользовательского интерфейса подключен к пользовательскому интерфейсу, который вызывает всплывающее окно на одной стороне.

![Автозаполнение](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>Прямоугольник фокуса клавиатуры и тень

Наша структура по умолчанию не выполняет никаких специальных действий по скруглению углов прямоугольника фокуса ввода с клавиатуры или тени элементов управления. Использование большего значения радиуса угла не нарушит его с функциональной точки зрения, однако об этом следует помнить, чтобы избежать нежелательных сбоев в визуализации, которые могут возникнуть при большем значении.

Ниже приведен пример того, как радиус более крупного угла может привести к нежелательному внешнему виду тени.

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>Скругленные углы и производительность

Отрисовка скругленных углов естественным образом использует больше возможностей рисования, чем отрисовка квадратных углов. При выборе значений радиуса угла по умолчанию мы не только учитывали принципы проектирования, но и старались обеспечить хорошую работу элементов управления по умолчанию при их использовании в ваших приложениях.

Размышляя о производительности приложения в этом контексте, вы должны в первую очередь учитывать время загрузки страницы и время запуска приложения. Учтите, что скругленные углы на большой поверхности пользовательского интерфейса оказывают большее влияние на производительность. Избегайте скругления углов в полноэкранном пользовательском интерфейсе приложения. Это менее проблематично, если пользовательский интерфейс отображается на короткое время после загрузки страницы, например в ContentDialog.

### <a name="page-or-app-wide-cornerradius-changes"></a>Изменения CornerRadius на уровне страницы или приложения

Существует 2 ресурса приложения, которые управляют радиусом углов всех элементов управления:

- `ControlCornerRadius`Значение по умолчанию — 2 пкс.
- `OverlayCornerRadius`Значение по умолчанию — 4 пкс.

Переопределение значения этих ресурсов в какой бы то ни было области соответствующим образом повлияет на все элементы управления в этой области.

Это означает, что если вы хотите изменить округлость всех элементов управления, к которым ее можно применить, вы можете определить оба ресурса на уровне приложений с помощью таких новых значений CornerRadius:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">4</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
            </ResourceDictionary>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Кроме того, если вы хотите изменить закругление всех элементов управления в определенной области, например на уровне страницы или контейнера, можно следовать подобному шаблону:

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> Чтобы изменения вступили в силу, ресурс `OverlayCornerRadius` следует определить на уровне приложения.
>
>Это связано с тем, что всплывающие окна и всплывающие меню являются динамическими и создаются в корневом элементе визуального дерева, поэтому в этом месте следует определить также все используемые ими ресурсы. В противном случае они выходят за пределы области.

### <a name="per-control-cornerradius-changes"></a>Изменения для каждого элемента управления CornerRadius

Чтобы изменить скругление только выбранного числа элементов управления, можно изменить свойство [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) непосредственно на элементах управления.

|По умолчанию | Измененное свойство |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

Не все углы элементов управления будут реагировать на изменение их свойства `CornerRadius`. Чтобы убедиться, что элемент управления, углы которого вы хотите закруглить, будет отвечать свойству `CornerRadius`, сначала проверьте, влияют ли на рассматриваемый элемент управления глобальные ресурсы `ControlCornerRadius` или `OverlayCornerRadius`. Если нет, убедитесь, что у элемента управления, который вы хотите закруглить, есть углы. Многие элементы управления не отображают фактические края и поэтому не могут правильно использовать свойство `CornerRadius`.
