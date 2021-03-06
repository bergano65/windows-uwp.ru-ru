---
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: Трехмерные эффекты для пользовательского интерфейса XAML
description: C помощью преобразований перспективы можно применять объемные эффекты к содержимому ваших приложений среды выполнения Windows. Например, можно создать иллюзию, что объект повернут к вам или отвернут от вас, как показано здесь.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8db56882833b9d3bd8a6d2932d04e07a72b205e2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365252"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>Трехмерные эффекты для пользовательского интерфейса XAML


C помощью преобразований перспективы можно применять объемные эффекты к содержимому ваших приложений среды выполнения Windows. Например, можно создать иллюзию, что объект повернут к вам или отвернут от вас, как показано здесь.

![Изображение с преобразованием перспективы](images/3dsimple.png)

Другой распространенный сценарий использования преобразований перспективы — это размещение объектов относительно друг друга для создания объемного эффекта, как здесь.

![Наложение объектов для создания объемного эффекта](images/3dstacking.png)

Помимо создания статических объемных эффектов, вы можете анимировать преобразование перспективы, чтобы создать движущиеся объемные эффекты.

Вы только что видели преобразование перспективы, примененное к изображениям; но вы также можете добавить эти эффекты к любому объекту [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), включая элементы управления. Например, можно применить объемный эффект ко всему контейнеру элементов управления, как показано ниже:

![Объемный эффект, примененный к контейнеру элементов](images/skewedstackpanel.png)

Вот код XAML, использованный для создания этого примера:

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

Здесь мы обращаем внимание на свойства [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection), которые используются для поворота и перемещения объектов в объемном пространстве. Следующий пример позволяет поэкспериментировать с этими свойствами и увидеть их влияние на объект.

## <a name="planeprojection-class"></a>Класс PlaneProjection

Можно применить трехмерные эффекты к любому элементу [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), задав значение свойства [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection) элемента UIElement с помощью [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection). **PlaneProjection** определяет, каким образом преобразование отображается в пространстве. В следующем примере рассмотрен простой случай.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

Этот рисунок показывает, как меняется изображение. Оси X, Y и Z показаны красными линиями. Изображение повернуто на 35 назад градусов вокруг оси X с помощью свойства [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx).

![RotateX минус 35 градусов](images/3drotatexminus35.png)

Свойство [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) выполняет поворот вокруг оси Y, проходящей через центр вращения.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY минус 35 градусов](images/3drotateyminus35.png)

Свойство [**RotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationz) выполняет поворот вокруг оси Z, проходящей через центр вращения (линии, перпендикулярной плоскости объекта).

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ минус 45 градусов](images/3drotatezminus35.png)

Свойствам вращения можно задать положительное или отрицательное значение, чтобы выполнить поворот в любом направлении. Абсолютное значение может превышать 360. В таком случае вращение объекта превысит один полный оборот.

С помощью свойств [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx), [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) и [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz) можно переместить центр вращения. По умолчанию оси вращения проходят прямо через центр объекта, благодаря чему он вращается вокруг своего центра. Но если вы переместите центр вращения к внешнему краю объекта, он будет вращаться вокруг этого края. По умолчанию свойства **CenterOfRotationX** и **CenterOfRotationY** имеют значение 0,5, а значение по умолчанию для свойства **CenterOfRotationZ** — 0. Для свойств **CenterOfRotationX** и **CenterOfRotationY** значения от 0 до 1 устанавливают точку вращения в каком-либо месте в пределах объекта. Значение 0 обозначает один край объекта, а значение 1 — противоположный край. Значения вне данного диапазона разрешены и перемещают центр вращения соответственно. Так как ось Z, проходящая через центр вращения, проходит через плоскость объекта, можно переместить центр вращения за объект, используя отрицательное число, и разместить его перед объектом (по направлению к себе), используя положительное число.

[**CenterOfRotationX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) перемещает центр вращения по параллельным оси x для объекта при [ **CenterOfRotationY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) перемещает center или поворота по оси y из объект. На следующих иллюстрациях показано использование различных значений для **CenterOfRotationY**.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.5" (default)**

![CenterOfRotationY равно 0,5](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.1"**

![CenterOfRotationY равно 0,1](images/3dcenterofrotationy0point1.png)

Обратите внимание, как изображение вращается вокруг центра, когда для свойства [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) установлено значение по умолчанию 0,5, и вращается у верхнего края, когда установлено значение 0,1. Можно наблюдать подобное поведение, изменяя свойство [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) для перемещения в точку, где свойство [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) вращает объект.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.5" (default)**

![CenterOfRotationX равно 0,5](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.9" (right-hand edge)**

![CenterOfRotationX равно 0,9](images/3dcenterofrotationx0point9.png)

Используйте [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz), чтобы поместить центр вращения выше или ниже плоскости объекта. Так вы можете вращать объект вокруг определенной точки, как планета вращается вокруг звезды.

## <a name="positioning-an-object"></a>Размещение объекта

До этого момента вы учились вращать объект в пространстве. Используя следующие свойства, можно разместить повернутые объекты в пространстве относительно друг друга:

-   [**LocalOffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx) перемещает объект вдоль оси x плоскости вращаемого объекта.
-   [**LocalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) перемещает объект вдоль оси y плоскости вращаемого объекта.
-   [**LocalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) перемещает объект вдоль оси z плоскости вращаемого объекта.
-   [**GlobalOffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) перемещает объект вдоль оси x по краю экрана.
-   [**GlobalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) перемещает объект вдоль оси y по правому краю экрана.
-   [**GlobalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) перемещает объект вдоль оси z на краю экрана.

**Локальное смещение**

Свойства [**LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx), [**LocalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) и [**LocalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz)перемещают объект вдоль соответствующей оси плоскости объекта после того, как он был повернут. Поэтому вращение объекта определяет направление, в котором он перемещается. Чтобы это продемонстрировать, в следующем примере выполняется анимация **LocalOffsetX** в диапазоне от 0 до 400 градусов и анимация [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) — от 0 до 65 градусов.

Обратите внимание, что в предыдущем примере объект перемещается вдоль своей собственной оси X. В самом начале анимации, когда значение [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) близко к нулю (параллельно экрану), объект перемещается вдоль экрана в направлении X, но по мере того как объект поворачивается в вашу сторону, он начинает перемещаться вдоль оси X плоскости объекта по направлению к вам. С другой стороны, если вы задали анимацию свойства **RotationY** до –65 градусов, объект будет удаляться от вас по кривой.

[**LocalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) действует аналогично [ **LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx), за исключением того, что он перемещается по вертикальной оси, поэтому изменение [ **RotationX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) влияет на направление **LocalOffsetY** перемещает объект. В следующем примере выполняется анимация **LocalOffsetY** в диапазоне от 0 до 400 градусов и анимация **RotationX** — от 0 до 65 градусов.

[**LocalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) преобразует объект перпендикулярно плоскости объекта так, будто вектор рисования непосредственно в центре из-за пределов объекта out к себе. Чтобы продемонстрировать, как работает свойство **LocalOffsetZ**, в следующем примере выполняется анимация **LocalOffsetZ** в диапазоне от 0 до 400 градусов и анимация [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) — от 0 до 65 градусов.

В начале анимации, когда значение [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) близко к нулю (параллельно экрану), объект перемещается прямо наружу по направлению к вам, но по мере вращения лицевой части объекта вниз он начинает двигаться вниз.

**Глобальное смещение**

Свойства [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx), [**GlobalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) и [**GlobalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) перемещают объект вдоль осей относительно экрана. То есть, в отличие от свойств локального смещения, ось, вдоль которой движется объект, не зависит от какого-либо вращения, примененного к объекту. Эти свойства полезны, если нужно просто переместить объект вдоль оси X, Y или Z экрана, не беспокоясь о вращении, примененном к объекту.

В следующем примере выполняется анимация [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) в диапазоне от 0 до 400 градусов и анимация [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) — от 0 до 65 градусов.

Обратите внимание, что в этом примере объект не меняет курс во время вращения. Это происходит из-за того, что объект перемещается вдоль оси X экрана без учета вращения.

## <a name="positioning-an-object"></a>Размещение объекта

Можно воспользоваться типами [**Matrix3DProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix3DProjection) и [**Matrix3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D) для более сложных сценариев полуобъемных эффектов, чем те, которые реализуются с помощью [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection). **Matrix3DProjection** предоставляет вам матрицу полного объемного преобразования, применимую к любому объекту [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), так что можно применять матрицы произвольного преобразования модели и матрицы перспективы к элементам. Не забывайте, что эти API минимальны, поэтому при использовании их необходимо написать код, который правильно создает матрицы объемного преобразования. По этой причине в простых сценариях объемных эффектов легче использовать **PlaneProjection**.
