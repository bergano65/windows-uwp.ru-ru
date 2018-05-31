---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: Введение в проектирование приложений универсальной платформы Windows (UWP) (приложений для Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817282"
---
#  <a name="introduction-to-uwp-app-design"></a>Введение в проектирование приложений UWP

Руководство по проектированию для универсальной платформы Windows (UWP) — это ресурс, который поможет проектировать и разрабатывать прекрасные, тщательно проработанные приложения.

Это не список нормативных правил — это постоянно обновляемый документ, изменения в который вносятся по мере развития нашей [системы проектирования Fluent Design](../fluent-design-system/index.md) и с учетом меняющихся потребностей нашего сообщества разработчиков приложений.

Это раздел основных сведений содержит обзор универсальных функций проектирования, которые включены в каждое приложение UWP, для создания пользовательских интерфейсов, которые превосходно масштабируются для отображения на различных устройствах.

## <a name="video-summary"></a>Видео с кратким описанием

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>Эффективные пиксели и масштабирование

Приложения UWP автоматически регулируют размер элементов управления, шрифтов и других элементов пользовательского интерфейса, чтобы они были разборчивыми и с ними было легко взаимодействовать [на всех устройствах, поддерживающих приложения UWP](../devices/index.md).

При запуске приложения на устройстве система использует алгоритм, позволяющий оптимизировать отображение элементов пользовательского интерфейса на экране. Этот алгоритм масштабирования учитывает расстояние, на котором осуществляется просмотр, и растровую плотность (количество пикселей на дюйм) и оптимизирует воспринимаемый (а не физический) размер элементов. Алгоритм масштабирования позволяет добиться того, что шрифт размером 24 пикселя на Surface Hub, который располагается на расстоянии трех метров от пользователя, будет таким же читаемым, как и шрифт аналогичного размера, отображаемый на телефоне с диагональю экрана 5 дюймов, который располагается на расстоянии нескольких сантиметров от пользователя.

![Дистанция просмотра для различных устройств](images/1910808-hig-uap-toolkit-03.png)

В силу особенностей работы системы масштабирования, создавая приложение UWP, вы ведете разработку в эффективных, а не физических пикселях. Эффективные пиксели (epx) — это виртуальные единицы измерения, которые используются для обозначения размеров макета и интервалов независимо от плотности экрана. (В наших рекомендациях обозначения epx, ep и px взаимозаменяемы.)

При разработке вы можете игнорировать плотность пикселей и фактическое разрешение экрана. Выполняйте разработку с учетом эффективного разрешения (разрешения в эффективных пикселях) для класса размеров (дополнительные сведения см. в статье [Размеры экрана и точки останова](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> При создании макетов в графических редакторах установите количество точек на дюйм равным 72 и соотнесите размеры изображения с эффективным разрешением целевого класса размеров. Список классов размеров и эффективные разрешения см. в статье [Размеры экрана и точки останова](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).


## <a name="layout"></a>Структура

### <a name="margins-spacing-and-positioning"></a>Поля, интервалы и размещение 
![сетка](images/4epx.png)

Система масштабирует пользовательский интерфейс приложения в величинах, кратных 4.

В результате размеры, поля и положения **элементов пользовательского интерфейса должны быть кратны 4 epx**. Это отражается на улучшении качества отрисовки за счет выравнивания с целыми пикселями. Кроме того, благодаря этому элементы пользовательского интерфейса имеют четкие, острые края. 

Обратите внимание, что это требование не относится к тексту; текст может быть любого размера и положения. Инструкции по выравниванию текста с другими элементами пользовательского интерфейса см. в разделе [Руководство по шрифтовому оформлению UWP](../style/typography.md).

![масштабирование на сетке](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>Шаблоны макетов

![Распространенный шаблон макета](images/page-components.png)

Пользовательский интерфейс состоит из трех типов элементов: 
1. **Элементы навигации** помогают пользователям выбрать то содержимое, которое они хотят отобразить. См. раздел [Основы навигации](navigation-basics.md).
2. **Командные элементы** служат для выполнения действий, таких как управление, сохранение или предоставление общего доступа к содержимому. См. раздел [Основные сведения о командных элементах](commanding-basics.md).
3. **Элементы содержимого** служат для отображения содержимого приложения. См. раздел [Содержимое: основные понятия](content-basics.md).

### <a name="adaptive-behavior"></a>Адаптивное поведение
![Адаптивное поведение для телефона или компьютера](../controls-and-patterns/images/patterns_masterdetail.png)

Несмотря на то что пользовательский интерфейс вашего приложения будет обладать хорошей читаемостью и удобством использования на всех устройствах под управлением Windows, вам может потребоваться настроить пользовательский интерфейс приложения для конкретных устройств и размеров экрана. Подробные инструкции см. в разделах [Размеры экрана и точки останова](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) и [Принципы разработки гибкого дизайна](../layout/responsive-design.md).

## <a name="type"></a>Тип

По умолчанию приложения UWP используют шрифт **Segoe UI**, а набор шрифтов UWP включает в себя семь классов оформления текста, что позволяет использовать доступные средства оформления пользовательского интерфейса наиболее эффективно для всех размеров экрана. 

![Набор шрифтов](images/type-ramp.png)

Дополнительные сведения о наборе шрифтов см. в разделе [Руководство по шрифтовому оформлению UWP](../style/typography.md). Чтобы узнать, как использовать различные уровни набора шрифтов UWP в приложении, см. раздел [Ресурсы темы](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp).

## <a name="color"></a>Цвет

Цвета — это интуитивно понятный способ передачи информации для пользователей. Его можно использовать для выделения интерактивных возможностей приложения, для обратной связи на действия пользователя, передачи сведений о состоянии, а также чтобы придать интерфейсу ощущение визуальной непрерывности. 

Windows 10 предоставляет общую универсальную цветовую палитру из 48 цветов, которая применяется в оболочке и приложениях UWP. 

![универсальная цветовая палитра windows](images/colors.png)

Система автоматически применяет цвет к приложению UWP, используя цвет элементов системы. Когда пользователь выбирает цвет элементов на цветовой палитре в разделе "Параметры", цвет отображается как часть темы системы. В зависимости от предпочтений пользователя цвет элементов системы также может отображаться на начальном экране (или меню "Пуск"), плитках начального экрана (или меню "Пуск"), панели задач и заголовках окон. 

![Выбор цвета элементов в разделе "Параметры"](images/selectcolor.png)

В приложении UWP приложения для гиперссылок и выбранных состояний в общих элементах управления будет использоваться цвет элементов системы.

![Цвет элементов системы для элементов управления](images/accentcolor.png)

В приложениях UWP можно переопределять цвет элементов системы так, чтобы он не использовался для элементов управления. Это можно сделать с помощью [облегченного определения стиля](../controls-and-patterns/xaml-styles.md#lightweight-styling) или путем создания [пользовательских элементов управления](../controls-and-patterns/control-templates.md).

Дополнительные рекомендации по использованию цвета в своем приложении UWP см. в статье [Цвет](../style/color.md).

### <a name="themes"></a>Темы

Пользователи могут выбрать между светлой, темной и темой с высокой контрастностью. Вы можете изменить внешний вид приложения на основе предпочтений темы пользователя или не делать этого.

Светлая тема лучше всего подходит для выполнения задач, когда требуется высокая производительность устройства, и для чтения.

Темная тема повышает контрастность в приложениях, связанных с мультимедиа, а также в тех случаях, когда пользователям приходится просматривать множество видео и изображений. В этих сценариях основная задача далеко не всегда связана с чтением, в то время как удобство просмотра фильмов при слабом освещении может быть крайне важным.

![Калькулятор в светлой и темной теме](images/light-dark.png)

В темах с высокой контрастностью используется небольшая палитра контрастных цветов, благодаря чему интерфейс хорошо видно.
![Калькулятор в светлой теме и в черной теме с высокой контрастностью.](../accessibility/images/high-contrast-calculators.png)


Дополнительные сведения об использовании тем и набора цветов UWP в вашем приложении см. в разделе [Ресурсы темы](../controls-and-patterns/xaml-theme-resources.md).

## <a name="icons"></a>Значки
![значки](images/icons.png)

Значки — это тип визуального языка, ставшего частью нашей повседневной жизни. Они позволяют выражать концепции и действия визуально привлекательным образом, экономить пространство на экране, а также служат в качестве средства перемещения по цифровой среде. 

У всех приложений UWP есть доступ к значкам в [шрифте Segoe MDL2](../style/segoe-ui-symbol-font.md). В основе этих значков используются стандартные формы, которые все знают, однако они также имеют современный вид, чтобы было похоже как-будто это штучная работа.

Если вы хотите создать собственные значки, см. раздел [Значки для приложений UWP](../style/icons.md).

## <a name="tiles"></a>Плитки
![Плитки в меню "Пуск"](images/tiles.png)

Плитки отображаются в меню "Пуск" и рассказывают содержимом приложения. Содержимое, скрытое за ними, и есть их функционал. Важно то, насколько искусно и умело они были созданы. 

Приложения UWP поддерживают плитки четырех размеров (маленький, средний, широкий и большой), которые можно настроить, используя значок приложения и его отличительные черты. Руководство по проектированию плиток для приложений UWP см. в разделе [Руководство по работе с ресурсами плиток и значков](../shell/tiles-and-notifications/app-assets.md).

## <a name="controls"></a>Элементы управления
![элемент управления ''Кнопка''](images/1910808-hig-uap-toolkit-01.png)

Платформа UWP предоставляет набор общих элементов управления, которые будут хорошо работать на всех устройствах под управлением Windows. Эти элементы управления включают в себя как простые элементы управления, такие как кнопки и текстовые элементы, так и сложные элементы управления, которые могут создавать списки на основе набора данных и шаблона.

К элементам управления UWP автоматически применяются тема системы и цвет элементов системы, они работают со всеми типами ввода данных и масштабируются на всех устройствах. Также они обладают широкими возможностями настройки— можно настроить основной цвет элемента управления или полностью изменить его внешний вид. 

Полный список элементов управления UWP и шаблонов, которые могут быть созданы на их основе, см. в разделе [Элементы управления и шаблоны](../controls-and-patterns/index.md).

## <a name="input"></a>Ввод
![Способы ввода](../input/images/input-interactions/icons-inputdevices03.png)

В основе приложений UWP лежит интеллектуальное взаимодействие с пользователем. Вы можете выполнять обработку нажатия и при этом вам будет все равно, осуществляется ли это нажатие с помощью щелчка мыши, пера или прикосновения пальца. Тем не менее вы также можете разработать приложения для [конкретных режимов ввода и устройств](../input/input-primer.md).

## <a name="accessibility"></a>Специальные возможности
![Пользователи инклюзивных разработок](images/inclusive.png)

Последнее, но не менее важное— это специальные возможности. Они позволяют сделать ваше приложение доступным для всех пользователей. Специальные возможности могут использовать все пользователи, а не только люди с ограниченными возможностями. Любой пользователь может воспользоваться преимуществами по-настоящему инклюзивных возможностей. См. раздел [Удобство использования приложений UWP](../usability/index.md) чтобы узнать, как сделать приложение удобным для всех пользователей. Вы также можете добавить в приложение [специальные возможности](../accessibility/accessibility-overview.md) для слабослышащих и слабовидящих пользователей, а также пользователей с трудностями в передвижении. 

Если специальные возможности встроены в проект вашего приложения с самого начала, [реализация полной доступности вашего приложения](../accessibility/accessibility-in-the-store.md) займет очень мало дополнительного времени и усилий.

## <a name="tools-and-design-toolkits"></a>Средства и наборы инструментов для проектирования
Теперь, когда вы знакомы с основными особенностями проектирования, давайте приступим к разработке собственного приложения UWP.

Мы предоставляем разнообразные инструменты для разработки:

* На [странице наборов инструментов для проектирования](../downloads/index.md) представлены XD, Illustrator, Photoshop, Framer, Sketch, а также другие инструменты и шрифты. 

* Чтобы подготовить компьютер к написанию кода для приложений UWP, изучите статью [Начало работы &gt;Начало работы](../../get-started/get-set-up.md). 

* Идеи по внедрению пользовательского интерфейса для UWP можно найти в разделе с готовыми [примерами приложений UWP](https://developer.microsoft.com/windows/samples).

## <a name="next-fluent-design-system"></a>Далее: система проектирования Fluent Design
Если вы хотите узнать о принципах проектирования с помощью Fluent Design (системы проектирования корпорации Майкрософт) и ознакомиться с дополнительными компонентами, которые можно встроить в приложение UWP, см. раздел [Система проектирования Fluent Design](../fluent-design-system/index.md).