---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: Направление и сила притяжения — анимация в приложениях UWP
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: a5216e81bc556a2e761e88b071e988bf6e4f457e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843918"
---
# <a name="directionality-and-gravity"></a>Направление и сила притяжения

> [!IMPORTANT]
> В этой статье описана еще не выпущенная функция, которая может быть существенно изменена до коммерческого выпуска. Майкрософт не дает никаких гарантий, явных или подразумеваемых, в отношении предоставленной здесь информации.

Направленные сигналы помогают закрепить ментальную модель пути, который пользователь проделывает в интерфейсе. Очень важно, чтобы направление любого движения поддерживало и непрерывность пространства, и целостность объектов в пространстве.

Направленное движение подвержено воздействию таких сил, как сила притяжения. Приложение силы к движению усиливает естественное восприятие движения.

## <a name="direction-of-movement"></a>Направление движения

:::row::: :::column::: Направление движения соответствует физическому перемещению. Так же, как в природе, объекты могут перемещаться по любой оси — X, Y, Z. Так мы воспринимаем движение объектов на экране.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Направление навигации

Для направления навигации между сценами в приложении характерна концептуальность. Пользователи переходят вперед и назад. Сцены появляются в поле зрения и уходят из него. Эти концепции в сочетании с физическим перемещением помогают пользователю.

Когда навигация вызывает перемещение объекта из предыдущей сцены в новую, объект просто перемещается из точки A в точку B. Чтобы повысить реалистичность перемещения объекта, добавляется стандартная анимация, а также ощущение силы притяжения.

При переходе назад перемещение отображается в обратном порядке (из точки B в точку A). Когда пользователи переходят назад, они рассчитывают как можно быстрее вернуться к предыдущему состоянию. Движение происходит быстрее, более прямолинейно и с замедлением в анимации.

Здесь эти принципы применяются, когда выбранный элемент остается на экране во время перехода вперед и назад.

![Пример непрерывного движения в пользовательском интерфейсе](images/continuous3.gif)

Когда навигация вызывает вымещение расположенных на экране элементов, важно показать, куда уходит старая сцена и откуда появляется новая.

Это дает ряд преимуществ:

- Закрепление ментальной модели пространства у пользователя.
- Показ исчезающей сцены предоставляет больше времени для подготовки содержимого для анимации в появляющейся сцене.
- Зрительное повышение производительности приложения.

Существует 4 отдельных направления навигации, которые необходимо учитывать.

:::row::: :::column::: **Вперед и внутрь**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end::: :::row::: :::column::: **Вперед и наружу**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end::: :::row::: :::column::: **Назад и внутрь**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end::: :::row::: :::column::: **Назад и наружу**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Сила тяготения

Сила тяготения делает процессы более естественными. Объекты, перемещающиеся по оси Z и не привязанные к сцене каким-либо экранным элементом, могут быть подвержены воздействию силы тяготения. Когда объект отрывается от сцены и до достижения им скорости ухода с экрана, сила тяготения тянет объект вниз, что обеспечивает более естественную траекторию перемещения объекта.

Сила тяготения обычно проявляется, когда объекту требуется перейти из одной сцены в другую. Поэтому в приведенной здесь анимации используется концепция силы тяготения.

Здесь на элемент в верхней строке сетки воздействует гравитация, что вызывает его небольшое падение по мере отрыва от своего места и перемещения на передний план.

![направление назад и внутрь](images/continuity-photos.gif)

## <a name="related-articles"></a>Смежные разделы

- [Обзор движения](index.md)
- [Синхронность и реалистичная анимация](timing-and-easing.md)