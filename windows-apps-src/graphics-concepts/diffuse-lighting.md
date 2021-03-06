---
title: Рассеянное освещения
description: Рассеянное освещение зависит и от направления света, и от нормали поверхности объекта.
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- Рассеянное освещения
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1785b06aa2217e8ec15aeaa560bd98a65522df2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603379"
---
# <a name="diffuse-lighting"></a>Рассеянное освещения


*Рассеянное освещение* зависит и от направления света, и от нормали поверхности объекта. Рассеянное освещение различается на разных участках поверхности объекта в результате изменения направления света и изменения числового вектора поверхности. Для расчета рассеянного света требуется больше времени, поскольку он различается для каждой вершины объекта, однако преимущество его использования заключается в том, что он придает объектам оттенки и трехмерную глубину.

После настройки интенсивности света для всех эффектов затухания модуль освещения вычисляет количество оставшегося света, отражаемое от вершины, с учетом угла нормали вершины и направления падающего света. Модуль освещения переходит к этому этапу в случае с направленными источниками света, так как они не ослабевают с расстоянием. Система учитывает два типа отражения — рассеянное и зеркальное — и использует другую формулу для определения количества отраженного света в каждом случае.

После вычисления количества отраженного света Direct3D применяет новые полученные значения к свойствам рассеянного и зеркального отражения текущего материала. Полученные значения цвета являются компонентами рассеянного и зеркального отражения, которые средство отрисовки использует для создания затенения по методу Гуро и световых бликов.

Рассеянное освещение описывается следующим уравнением.

Рассеянное освещение = сумма\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup> L<sub>dir</sub>)\*Atten\*место\]

| Параметр       | Значение по умолчанию | Тип          | Описание                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| sum             | Н/Д           | Н/Д           | Совокупность рассеянных компонентов каждого источника света.                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Рассеянный цвет.                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Рассеянный цвет источника света.                                                                             |
| N               | Н/Д           | D3DVECTOR     | Нормаль вершины                                                                                    |
| L<sub>dir</sub> | Н/Д           | D3DVECTOR     | Вектор направления от вершины объекта к источнику света.                                                |
| Atten           | Н/Д           | FLOAT         | Затухание света. См. раздел [Коэффициент затухания и вспышки](attenuation-and-spotlight-factor.md). |
| Spot            | Н/Д           | FLOAT         | Коэффициент узкой направленности света. См. раздел [Коэффициент затухания и вспышки](attenuation-and-spotlight-factor.md).  |

 

Инструкции по вычислению характеристик затухания (Atten) или узкой направленности света (Spot) см. в разделе [Коэффициент затухания и узкой направленности света](attenuation-and-spotlight-factor.md).

Рассеянные компоненты прикрепляются к диапазону от 0 до 255, после того как все источники освещения отдельно обработаны и интерполированы. Полученное значение рассеянного света представляет собой сочетание значений внешнего, рассеянного и излучаемого освещения.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Пример


В этом примере цвет объекта определяется рассеянным цветом источника света и рассеянным цветом материала.

Согласно уравнению, получившийся цвет для вершин объекта — это сочетание цвета материала и цвета света.

На двух следующих рисунках показан цвет материала (серый) и цвет света (ярко-красный).

![иллюстрация серой сферы](images/amb1.jpg)![иллюстрация красной сферы](images/lightred.jpg)

Получившаяся сцена показана на следующем рисунке. Единственный объект в сцене — сфера. Для расчета рассеянного света рассеянный цвет материала и источника света изменяется с учетом угла между направлением света и нормалью вершины при помощи скалярного произведения. В результате, тыльная сторона сфера темнеет по мере ухода изгиба сферы от источника света.

![иллюстрация сферы с рассеянным освещением](images/lightd.jpg)

При совмещении рассеянного и внешнего освещения в предыдущем примере обеспечивает затенение всей поверхности объекта. Внешнее освещение затеняет всю поверхность, а рассеянное освещение помогает раскрыть трехмерную форму объекта, как показано на следующей иллюстрации.

![иллюстрация сферы с рассеянным и внешним освещением](images/lightad.jpg)

Рассеянное освещение сложнее в расчете, чем внешнее. Поскольку оно зависит от нормалей вершин и направления света, раскрывается геометрия объекта в трехмерном пространстве, поэтому освещение становится более реалистичным, чем при использовании внешнего освещения. Вы можете использовать световые блики для получения более реалистичного вида.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Математика освещения](mathematics-of-lighting.md)

 

 




