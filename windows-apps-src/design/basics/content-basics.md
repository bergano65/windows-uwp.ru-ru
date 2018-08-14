---
author: serenaz
Description: An overview of common page patterns and UI elements for displaying content in your UWP app.
title: Основы проектирования содержимого для приложений универсальной платформы Windows (UWP)
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 12/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6479d8ed5a0a0f98556f49bf12a30408de052f8a
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654333"
---
# <a name="content-design-basics-for-uwp-apps"></a>Основы проектирования содержимого для приложений UWP

Основная цель любого приложения — обеспечить доступ к содержимому. Так как разные приложения используются для разных целей, содержимое может иметь разные формы: в приложении для редактирования фотографий содержимым является фотография, в приложении для путешествий — карты и сведения о достопримечательностях и т. д. 

В этой статье приведено описание того, как можно представлять содержимое в приложении. Мы добавили распространенные шаблоны страниц и элементов пользовательского интерфейса, которые можно использовать для отображения содержимого независимо от его формы.

## <a name="common-page-patterns"></a>Распространенные шаблоны страниц

Многие приложения используют некоторые или все эти распространенные шаблоны страниц для отображения различных типов содержимого. Аналогичным образом вы можете комбинировать и сопоставлять эти шаблонов для оптимизации содержимого вашего приложения.

### <a name="landing"></a>Целевая страница

![Целевая страница](images/content-basics/hero-screen.png)

Целевые страницы, также называемые главными экранами, часто отображаются на верхнем уровне взаимодействия с приложением. Область с большой поверхностью выступает в качестве уровня приложений, на котором можно выделить содержимое, которое пользователи могут захотеть просматривать или потреблять.

### <a name="collections"></a>Коллекции

![галерея](images/content-basics/gridview.png)

Коллекции позволяют пользователям просматривать группы содержимого или данных. [Представление в виде сетки](../controls-and-patterns/item-templates-gridview.md) является хорошим способом размещения фотографий или мультимедиа-содержимого, а [представление списка](../controls-and-patterns/item-templates-listview.md) является хорошим способом размещения содержимого с большим количеством текста или данных.

### <a name="hub"></a>Центр

![центр](images/content-basics/hub.png)

[Центры](../controls-and-patterns/hub.md) предназначены для просмотра содержимого с его последующим выбором. Пользователи получают возможность хорошо рассмотреть предлагаемое содержимое. Основной задачей центров является компактная демонстрация всего разнообразия содержимого. Например, раздел 1 центра может содержать главный экран, раздел 2 центра может содержать коллекцию, раздел 3 центра может содержать другую коллекцию и так далее.

### <a name="masterdetail"></a>Основные и подробные данные

![Основные и подробные данные](images/content-basics/master-detail.png)

Модель [основных и подробных данных](../controls-and-patterns/master-details.md) состоит из представления списка (основные данные) и представления содержимого (подробные данные). Обе области закреплены и поддерживают вертикальную прокрутку. Между элементом списка и представлением содержимого существует четкая связь: при выборе элемента в основном представлении соответствующим образом обновляется подробное представление. Помимо возможности перемещения по подробному представлению элементы в основном представлении можно добавлять и удалять.

### <a name="details"></a>Подробные сведения

![Несколько представлений](images/multi-view.png)

Рассмотрите возможность создания специальных страниц просмотра содержимого, чтобы пользователи находили нужное содержимое, которое можно просмотреть на специальной странице без каких-либо посторонних элементов. Если это возможно, [создайте полноэкранный вариант просмотра](../layout/show-multiple-views.md), который развернет содержимое на весь экран, скрыв при этом все остальные элементы пользовательского интерфейса. 

Чтобы обеспечить подстройку содержимого к размеру экрана, также рекомендуется создать [гибкий дизайн](design-and-ui-intro.md), который будет скрывать/показывать элементы пользовательского интерфейса соответствующим образом.

### <a name="forms"></a>Формы
![форма](images/content-basics/forms.png)

[Форма](../controls-and-patterns/forms.md) — это группа элементов управления, которые собирают данные пользователей и отправляют их. В большинстве приложений, если не во всех из них, используется форма определенного рода для страниц параметров, процедуры входа на порталы, центров отзывов, создания учетных записей и других целей. 

## <a name="common-content-elements"></a>Распространенные элементы содержимого

Для создания таких шаблонов страниц необходимо использовать сочетание отдельных элементов содержимого. Ниже приведены некоторые элементы пользовательского интерфейса, которые часто используются для отображения содержимого. (Полный список элементов пользовательского интерфейса см. в разделе [Элементы управления и шаблоны](../controls-and-patterns/index.md).)

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Категория</th>
<th align="left">Элементы</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Звук и видео<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">Элементы управления транспортом и воспроизведением мультимедиа</a></td>
<td align="left">Воспроизведение звука и видео.</td>
</tr>
<tr class="even">
<td align="left">Средства просмотра изображения<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">Представление пролистывания</a>, <a href="../controls-and-patterns/images-imagebrushes.md">изображение</a></td>
<td align="left">Отображение изображений. Элемент пролистывания предназначен для просмотра изображений в коллекции, например фотографий в альбоме или элементов на странице описания продукта. Каждый раз отображается одно изображение.</td>
</tr>
<tr class="odd">
<td align="left">Коллекции <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Представления списка и сетки</a></td>
<td align="left">Представляет элементы в интерактивном списке или сетке. Используйте эти элементы, чтобы позволить пользователям выбирать фильм из списка новинок или управлять складом.</td>
</tr>
<tr class="even">
<td align="left">Текст и текстовый ввод <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">Блок текста</a>, <a href="../controls-and-patterns/text-box.md">блок текста</a>, <a href="../controls-and-patterns/rich-edit-box.md">поле с форматом</a></p>
</td>
<td align="left">Отображение текста. Некоторые элементы позволяют пользователю редактировать текст. Дополнительные сведения см. в разделе <a href="../controls-and-patterns/text-controls.md">Элементы управления текстом</a>.
<p>Инструкции по отображению текста см. в разделе [Оформление текста](../style/typography.md).</p>
</td>
</tr>
<tr class="odd">
<td align="left">Карты<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">Отображает символьную или фотореалистичную карту Земли.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">Преобразует HTML-содержимое для его просмотра.</td>
</tr>
</tbody>
</table>
</div>