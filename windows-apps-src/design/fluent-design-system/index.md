---
description: Изучите систему проектирования Fluent и способы ее интеграции в приложения.
title: Система проектирования Fluent для приложений UWP
author: mijacobs
keywords: структура приложения uwp, универсальная платформа windows, проектирование приложений, интерфейс, система проектирования fluent
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5b57dc2ddae4c6e260df663097db5649866b96aa
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638792"
---
# <a name="the-fluent-design-system-for-uwp-apps"></a>Система проектирования Fluent для приложений UWP

## <a name="introduction"></a>Введение

<img src="images/fluentdesign-app-header.jpg" alt=" " />

Пользовательские интерфейсы развиваются. В нем появляются новые измерения и способы взаимодействия — от двухмерных к трехмерным и далее, от клавиатуры и мыши до взгляда, пера и сенсорного ввода.  

Система проектирования Fluent Design — это набор инновационных функций UWP и рекомендаций по созданию приложений, которые прекрасно смотрятся на всех типах устройств под управлением Windows.

Это наша система для создания адаптивных, отзывчивых и красивых пользовательских интерфейсов. 

**Адаптивность: взаимодействия типа Fluent естественны на любом устройстве**

Взаимодействия типа Fluent адаптируются к среде. Взаимодействия типа Fluent удобны как на планшете, так и на ПК и Xbox и прекрасно подходят даже для гарнитуры смешанной реальности. А при добавлении нового оборудования, такого как дополнительный монитор для вашего компьютера взаимодействие Fluent использует и его. 

**Отзывчивость: взаимодействия типа Fluent интуитивно понятны и эффективны**

Взаимодействия типа Fluent адаптируются к поведению и намерениям пользователя &mdash; они понимают и заранее предугадывают то, что от них требуется. Они объединяют людей и идеи, которые находятся в противоположных уголках земного шара или сосем рядом друг с другом. 

Проявление отзывчивости означает выполнение нужных действий в нужное время. 

Взаимодействия типа Fluent используют элементы управления и шаблоны согласованно, чтобы они работали так, как ожидает пользователь. Взаимодействия типа Fluent доступным для людей с широким диапазоном физических возможности и включают функции глобализации, чтобы их могли использовать люди по всему миру. 

**Красота: взаимодействия типа Fluent привлекательны и иммерсивны** 

Включая элементы реального мира, взаимодействия типа Fluent устанавливают связь с фундаментальными аспектами. Они используют свет, тень, движение, глубину и текстуру для организации информации интуитивно и инстинктивно понятным образом. 

Fluent Design не использует яркие эффекты. Он использует физические эффекты, которые действительно улучшают взаимодействие с пользователем, так как эмулируют взаимодействие, которое наш мозг запрограммирован обрабатывать эффективно. 

## <a name="applying-fluent-design-to-your-app"></a>Применение Fluent Design для вашего приложения

Функции Fluent Design встроены в UWP. Некоторые из этих функций, такие как эффективные пиксели и универсальная система ввода, являются автоматическими. Для их использования не нужно создавать дополнительный код. Другие функции, например акриловые элементы, являются дополнительными; они добавляются в приложение путем создания соответствующего кода. 

> Чтобы узнать больше о базовых функциях, которые автоматически включаются в каждое приложение UWP, обратитесь к статье [Вводные сведения о проектировании приложения UWP](../basics/design-and-ui-intro.md). Если вы совсем не знакомы с разработкой приложений для UWP, рекомендуем сначала ознакомиться со страницей [Начало работы с UWP](https://developer.microsoft.com/windows/apps/getstarted). 

Чтобы узнать больше о новых функциях, которые позволяют интегрировать систему проектирования Fluent Design в ваше приложение, продолжайте читать этот раздел.

## <a name="find-a-natural-fit"></a>Найдите естественный подход

Как сделать приложение предельно удобным на различных устройствах? Необходимо создать ощущение, что оно было разработано с учетом каждого из них. Макет пользовательского интерфейса, адаптированный для различных размеров экрана таким образом, чтобы отсутствовало неиспользуемое пространство (а интерфейс при этом не был перегружен), делает взаимодействие естественным, как будто интерфейс был разработан для конкретного устройства. 

*  **Проектируйте для правильных точек прерывания**

    Вместо проектирования для каждого размера экрана в отдельности, сосредоточьтесь на нескольких ключевых значениях ширины (также называемых «точки прерывания»), чтобы значительно упростить код и дизайн и все равно обеспечить отличный вид приложения на экранах всех размеров.

    [Сведения о размерах экрана и точках прерывания](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)

*  **Создавайте динамические макеты**

    Чтобы интерфейс приложения выглядел естественно, он должен заполнять доступное место на экране, не создавая ощущения перегруженности. UWP предоставляет панели для упорядочивания содержимого в сетки, стеки и потоки, которые могут быть вложены друг в друга.

    [Сведения о панелях макета UWP](/windows/uwp/design/layout/layout-panels)

* **Проектируйте для спектра различных устройств**

    Приложения UWP могут работать на разнообразных устройствах под управлением Windows. Рекомендуется изучить список доступных устройств, для чего они предназначены, и как пользователи взаимодействуют с ними.

    [Подробнее об устройствах UWP](/windows/uwp/design/devices/)

* **Оптимизируйте для необходимого типа ввода**

    Приложения UWP автоматически поддерживают распространенные типы взаимодействия с помощью мыши, клавиатуры, пера и сенсорного ввода &mdash; никаких дополнительных действий не требуется. Однако можно улучшить приложение путем оптимизации поддержки определенных типов ввода, таких как перо и Surface Dial.

    [Сведения о типах ввода и взаимодействия](/windows/uwp/design/input/input-primer)


## <a name="make-it-intuitive-and-powerful"></a>Обеспечьте интуитивность и эффективность

Взаимодействие является интуитивным, когда оно соответствует тому, что ожидает пользователь. Используя заданные элементы управления и шаблоны, а также поддержку платформой специальных возможностей и глобализации, можно создавать не требующие усилий взаимодействия, которые помогают пользователям эффективнее выполнять задачи. 

* **Используйте правильные элементы управления для текущей задачи**

    Элементы управления — это основа пользовательского интерфейса; с помощью подходящих элементов управления можно создать пользовательский интерфейс, поведение которого соответствует ожиданиям пользователя.  UWP предоставляет более 45 элементов управления, начиная с простых кнопок, и заканчивая элементами управления данными с широкими возможностями. 

    [Сведения об элементах управления UWP](/windows/uwp/design/controls-and-patterns/)

* **Обеспечьте инклюзивность** 

    Хорошо спроектированные приложения обеспечивают доступность для людей с ограниченными возможностями. С помощью некоторого дополнительного кодирования можно сделать приложение доступным людям по всему миру.

    [Сведения об удобстве использования](/windows/uwp/design/usability/)


## <a name="be-engaging-and-immersive"></a>Обеспечьте привлекательность и иммерсивность 

Сделайте ваше приложение привлекательным, включив в него физические элементы, такие как свет и движение. 

## <a name="use-light"></a>Используйте свет

Свет всегда привлекает внимание. Он создает определенную атмосферу и ощущение места, это практический инструмент для подсветки важной информации.
        
Добавьте свет в свое приложение UWP:
        
* [Эффект отображения](../style/reveal.md) использует свет для выделения интерактивных элементов. Свет освещает элементы, с которыми может взаимодействовать пользователь, обнажая скрытые границы. Этот эффект автоматически включается для некоторых элементов управления, таких как представление списка и представление сетки. Его можно включить для другие элементов управления с помощью предопределенных стилей эффекта отображения. 

* [Фокус отображения](../style/reveal-focus.md) использует свет для привлечения внимания к элементу, на котором в данный момент находится фокус ввода.  

## <a name="create-a-sense-of-depth"></a>Создайте эффект глубины

Мы живем в трехмерном мире. Специально реализуя глубину в пользовательском интерфейсе, мы преобразуем плоский двухмерный интерфейс в нечто большее, в нечто, что эффективно представляет информацию и понятия, создавая визуальную иерархию. Такой интерфейс по-новому представляет связи между объектами в многослойной физической среде.

Добавьте глубину в свое приложение UWP:

* [Акрил](../style/acrylic.md)— это полупрозрачный материал, с помощью которого пользователь может просматривать слои содержимого, формируя иерархию элементов пользовательского интерфейса.

* [Параллакс](../motion/parallax.md) создает иллюзию глубины, благодаря чему кажется, что элементы на переднем плане движутся быстрее, чем на заднем плане.

## <a name="incorporate-motion"></a>Включите движение

Проектирование движения можно расценивать как съемку фильма. Плавные переходы позволяют сконцентрироваться на происходящем и оживляют изображение. Если реализовать это в пользовательском интерфейсе, пользователи будут логично и удобно, словно в кино, переходить от одной задачи к другой.

Добавьте движение в свое приложение UWP:

* [Подключенные анимации](../motion/connected-animation.md) помогают пользователю сохранять контекст, создавая плавные переходы между сценами. 

## <a name="build-it-with-the-right-material"></a>Используйте подходящие материалы

В реальном мире нас окружают доступные чувственному восприятию, полные жизни вещи. Они гнутся, тянутся, пружинят, разбиваются и скользят. Эти качества материалов можно перенести и в цифровую среду, чтобы пользователям захотелось прикоснуться к тому, что они видят на экранах.

Добавьте материалы в свое приложение UWP: 
        
* [Акрил](../style/acrylic.md)— это полупрозрачный материал, с помощью которого пользователь может просматривать слои содержимого, формируя иерархию элементов пользовательского интерфейса. 

## <a name="design-toolkits-and-code-samples"></a>Наборы инструментов для проектирования и примеры кода

Хотите приступить к созданию собственных приложений с помощью системы проектирования Fluent Design? Наши наборы инструментов для Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer и Sketchим помогут быстро приступить к работе над проектом, а образцы — быстрее перейти к кодированию.

* Посетите нашу [страницу с наборами инструментов для проектирования и образцами](/windows/uwp/design/downloads/)

<img src="images/fluentdesign_header.png" alt=" " />







