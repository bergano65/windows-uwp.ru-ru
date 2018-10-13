---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Привязка данных и MVVM
description: Привязка данных основой шаблон проектирования архитектуры пользовательского интерфейса Model-View-ViewModel (MVVM) и позволяет свободная связь между кода пользовательского интерфейса и без пользовательского интерфейса.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2018
ms.locfileid: "4574613"
---
# <a name="data-binding-and-mvvm"></a>Привязка данных и MVVM

Model-View-ViewModel (MVVM) — это шаблон проектирования архитектуры пользовательского интерфейса для разделения кода пользовательского интерфейса и без пользовательского интерфейса. С помощью MVVM декларативно определения пользовательского интерфейса в XAML и использовать разметку привязки данных для связать ее со другие уровни, содержащих данные и команды. Инфраструктура привязки данных обеспечивает слабую связь, который держит пользовательского интерфейса и связанных данных синхронизации и направляет входные данные пользователя в соответствующие команды. 

Так как она обеспечивает слабую связь, использование привязки данных уменьшает жесткие зависимости между различные виды кода. Это упрощает изменение отдельных модулей (методы, классы, элементы управления, и т.д.) не вызовет непредусмотренные побочные эффекты в других единицах. Это разделения является примером *Разделение проблем*, это важно в многие шаблоны проектирования. 

## <a name="benefits-of-mvvm"></a>Преимущества MVVM

Разделение кода имеет множество преимуществ, в том числе:

* Включение итеративный произвольного кода стиль. Изменение, которая изолирована менее рискованно и проще поэкспериментировать с.
* Упрощения модульное тестирование. Можно протестировать блоки кода, которые изолированы друг от друга, по отдельности, так и вне рабочих средах.
* Поддержка совместной работы. Отделенный код, соответствующий разработанное интерфейсы можно разработчик отдельных сотрудников или групп и интегрировать в более поздней версии.
* Повышение удобства поддержки. Устранение ошибок в коде отделенный меньшей вероятностью вызывают регрессий в другой код.

В противоположность MVVM приложения с более традиционных структурой «кода» обычно используется привязка данных только для отображения данных и отвечает на ввод данных пользователем с помощью непосредственно обработки событий, предоставляемые элементом управления. Обработчики событий, которые реализованы в файлы кода программной части (например, MainPage.xaml.cs) и часто от него тесно связаны для элементов управления, как правило, содержащий код, который управляет пользовательский Интерфейс напрямую. Это делает сложно или невозможно заменить элемент управления без необходимости обновить код обработки событий. С помощью этой архитектуры файлы кода программной части часто накапливаются при постоянном код, не относящиеся непосредственно к пользовательскому Интерфейсу, такие как код доступа к базе данных, который может оказаться не может быть дублирование и изменить для использования с другими страницами.

## <a name="app-layers"></a>Приложения

При использовании шаблон MVVM, приложение состоит из следующих уровням:

* Уровень **модели** определяет типы, которые представляют бизнес-данных. Это включает в себя все необходимые для модели домена основные приложения и представитель основной логики приложения. Этот уровень совершенно независим от представления и модели представления уровней и часто частично размещены в облаке. Конкретный уровень полностью реализованные модели, можно создать несколько различных клиентских приложений при необходимости можно выбрать, например UWP и веб-приложения, которые работают с теми же данными базовый.
* На уровне **представления** определяет пользовательский Интерфейс, с помощью разметки XAML. Разметка включает в себя выражения привязки данных (например, [x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)), определяющих соединение между компонентами пользовательского интерфейса и различные модели представления и модели члены. Файлы кода программной части иногда используются как часть уровня представления содержат дополнительный код, необходимый для настройки и управления пользовательского интерфейса, или для извлечения данных из аргументов события обработчик перед вызовом метода модели представления, который выполняет работу. 
* На уровне **модель представления** предоставляет цели привязки данных для представления. Во многих случаях модель представления предоставляет модель напрямую или предоставляет члены с переносом членов конкретной модели. Модель представления, можно также определить члены для отслеживания данные, относящиеся к пользовательскому Интерфейсу, но не к модели, таких как порядок отображения списка элементов. Модель представления, также выступает в качестве точки интеграции с другими службами, такими как кода доступа к базе данных. Для простых проектов не может понадобиться уровень отдельные модели, но только модели представления, инкапсулирующего все необходимые данные. 

## <a name="basic-and-advanced-mvvm"></a>Основные и дополнительные MVVM

Как и в случае с любым шаблоном разработки существует способ реализации MVVM и множество различных способов считаются частью MVVM. По этой причине существует несколько различных платформ MVVM третьих лиц, поддержка различных на основе XAML платформ, включая UWP. Тем не менее эти платформы обычно содержат несколько служб для реализации отделенный архитектуры, что точное определение MVVM немного неоднозначным. 

Несмотря на то, что сложных MVVM платформ могут оказаться очень полезными, особенно для проектов корпоративного уровня, обычно есть затраты, связанные с внедрение любой определенного шаблона или прием и преимущества не всегда очистить, в зависимости от масштаба и размер проект. К счастью можно использовать только методы, которые предоставляют понятными и ощутимые преимущества и игнорировать других пользователей, пока они требуются. 

В частности вы можете получить множество преимуществ, достаточно понимание и применение возможности данных для привязки и разделить логику приложения в слои, описанных выше. Этого можно достичь с помощью только возможности, предоставляемые Windows SDK и без использования все внешние платформы. В частности, [расширения разметки {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) упрощает привязку данных и более поздней версии, выполнение чем в предыдущих платформ XAML, что устраняет необходимость значительная часть кода шаблонных требуется более ранних версий.

Дополнительные рекомендации по использованию базовый, вне поля MVVM посмотрите [пример базы данных заказов клиентов](https://github.com/Microsoft/Windows-appsample-customers-orders-database) на GitHub. Во многих других [примеров приложений UWP](https://github.com/Microsoft?q=windows-appsample
) также использовать базовой архитектуре MVVM и [пример приложения трафика](https://github.com/Microsoft/Windows-appsample-trafficapp) включает кода и версиями MVVM, заметки, описывающий [MVVM преобразования](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>См. также

### <a name="topics"></a>Разделы

[Подробно о привязке данных](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Расширение разметки {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Примеры

[Пример базы данных заказов клиентов](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Пример VanArsdel инвентаризации](https://github.com/Microsoft/InventorySample)  
[Пример приложения трафика](https://github.com/Microsoft/Windows-appsample-trafficapp)  