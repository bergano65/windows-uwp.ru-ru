---
title: Типы освещения
description: Свойство типа освещения определяет, какой тип источника света вы используете. В Direct3D существует три типа источников света — точечные, прожекторные и направленные.
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- Типы освещения
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f3dd8397f92137bbd934b2f5835de703f05c2000
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044683"
---
# <a name="light-types"></a>Типы освещения


Свойство типа освещения определяет, какой тип источника света вы используете. В Direct3D существует три типа источников света — точечные, прожекторные и направленные. Каждый тип по-разному освещает объекты в сцене и вносит различные объемы вычислительной нагрузки.

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>Точечный источник света


Точечные источники света имеют цвет и расположение в сцене, но не имеют определенного направления. Они излучают свет равномерно во всех направлениях, как показано на следующем рисунке.

![иллюстрация точечного источника света](images/ptlight.png)

Хорошим примером точечного источника света является лампа накаливания. Точечные источники света обладают свойствами затухания и дальности действия и освещают сетку по принципу вершина за вершиной. Во время освещения Direct3D использует расположение точечного источника света в пространстве и координаты освещаемых вершин, чтобы рассчитать вектор направленности освещения и расстояние, которое свет преодолевает. Оба этих параметра используются вместе с нормалью вершины, чтобы вычислить вклад этого источника света в освещенность определенной поверхности.

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>Направленный источник света


Направленные источники света имеют только цвет и направление и не имеют расположения. Они излучают параллельный свет. Это означает, что весь излучаемый направленными источниками свет проходит через сцену в одном направлении. Представить себе направленный источник света можно как очень сильно удаленный источник света, такой как солнце. Направленные источники света не имеют свойств затухания и дальности действия, поэтому при расчете цветов вершин в качестве коэффициентов Direct3D использует только заданные вами свойства направления и цвета. Из-за небольшого количества коэффициентов освещения эти источники света являются наименее сложными в вычислениях.

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>Прожекторные источники света


Прожекторные источники света имеют свойства цвета, расположения и направления, в котором они светят. Свет, излучаемый прожекторным источником, состоит из яркого внутреннего конуса и более крупного внешнего конуса, а интенсивность света уменьшается от одного к другому, как показано на следующем рисунке.

![иллюстрация прожектора с внутренним конусом и внешним конусом](images/spotlt.png)

Прожекторы имеют свойства ослабления, затухания и дальности действия. Эти факторы, а также расстояние, которое свет преодолевает до каждой вершины, учитываются при расчете эффектов освещения для объектов в сцене. Вычисление этих эффектов для каждой вершины делает прожекторы наиболее сложными из всех источников света в Direct3D.

Значения ослабления, тета и фи используются только прожекторными источниками. Эти значения определяют размер внутренних и внешних конусов света на объекте, а также то, как свет ослабляется при переходе от одного к другому.

Тета — это угол для внутреннего конуса освещения в радианах, а фи — это угол для внешнего конуса освещения прожектора. Ослабление определяет, как снижается интенсивность освещения при переходе от внешнего края внутреннего конуса к внутреннему краю внешнего конуса. В большинстве приложений задается значение ослабления 1,0 для создания равномерного ослабления на переходе между двумя конусами, но при необходимости можно задать другие значения.

На следующем рисунке показана связь между этими значения и тем, как они влияют на внутренние и внешние конусы прожектора.

![иллюстрация связи значений фи и тета с конусами прожектора](images/spotlt2.png)

Прожекторы излучают свет, состоящий из двух частей: яркий внутренний конус и внешний конус. Самый яркий свет попадает во внутренний конус, а за пределами внешнего конуса свет отсутствует; между этими двумя областями интенсивность света ослабляется. Этот тип снижения интенсивности обычно называется ослаблением.

Количество света, который получает вершина, основано на расположении вершины во внутреннем или внешнем конусе. Direct3D вычисляет скалярное произведение вектора направления прожектора (L) и вектора от источника света до вершины (D). Это значение равно косинусу угла между двумя векторами и служит индикатором расположения вершины, его можно сравнить с углами конуса света, чтобы определить, находится ли вершина во внутреннем или внешнем конусе. На иллюстрации ниже представлено графическое изображение отношения между этими двумя векторами.

![изображение вектора направления прожектора и вектора от вершины до прожектора](images/spotalg1.png)

Система сравнивает это значение с косинусом углов внутреннего и внешнего конусов прожектора. Значения тета и фи источника света представляют суммарные значения углов конуса для внутреннего и внешнего конусов. Поскольку ослабление происходит по мере удаления вершины от центра освещенности (а не по всему углу конуса), среда выполнения делит эти углы конуса пополам перед расчетом их косинусов.

Если скалярное произведение векторов L и D меньше либо равно косинусу угла внешнего конуса, вершина находится за пределами внешнего конуса и не получает света. Если скалярное произведение векторов L и D больше косинуса угла внутреннего конуса, значит, вершина находится в пределах внутреннего конуса и получает максимальное количество света с учетом затухания по мере удаления. Если вершина находится где-то между двумя этими областями, ослабление рассчитывается по следующему уравнению.

![формула интенсивности света на вершине после ослабления](images/falloff.png)

Где:

-   I<sub>f</sub> — интенсивность света после ослабления
-   Альфа — это угол между векторами L и D
-   Тета — это угол внутреннего конуса
-   Фи — это угол внешнего конуса
-   p — значение ослабления

Эта формула выдает значение от 0,0 до 1,0, которое определяет интенсивность света на вершине с учетом ослабления. Также применяется значение затухания, которое зависит от расстояния между вершиной и источником света. На следующем графике показано, как различные значения ослабления могут влиять на кривую ослабления.

![график интенсивности света в зависимости от расстояние между вершиной и источником света](images/fallgraf.png)

Влияние различных значений ослабления на фактическое освещение довольно умеренное, и небольшое снижение производительности вызывается при расчете кривой для значений ослабления, отличных от 1,0. По этим причинам это значение обычно устанавливается равным 1,0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Статьи по теме


[Источники света и материалы](lights-and-materials.md)

 

 



