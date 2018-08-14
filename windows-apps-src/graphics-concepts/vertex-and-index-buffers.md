---
title: Буферы вершин и индексов
description: Буферы вершин — это буферы памяти, которые содержат данные вершин; вершины в буфере вершин обрабатываются в целях выполнения преобразований, освещения и кадрирования.
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- Буферы вершин и индексов
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 120e15762a0c9c8fb0313ec756dc64e469c9b37b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652913"
---
# <a name="vertex-and-index-buffers"></a>Буферы вершин и индексов


*Буферы вершин* — это буферы памяти, которые содержат данные вершин; вершины в буфере вершин обрабатываются в целях выполнения преобразований, освещения и кадрирования. *Буферы индексов* — это буферы памяти, содержащие данные индексов, представляющие собой целочисленные указатели для буферов вершин, которые используются для отрисовки примитивов.

Буферы вершин могут содержать вершины любого типа — преобразованные или непреобразованные, освещенные или неосвещенные — которые могут быть отрисованы. Вершины в буфере вершин можно обрабатывать для выполнения таких операций, как преобразование, освещение или создание флагов кадрирования. Преобразование выполняется во всех случаях.

Гибкость буферов вершин делает их идеальным механизмом промежуточного хранения для многократного использования преобразованной геометрии. Можно создать один буфер вершин, преобразовать, осветить и кадрировать вершины в нем, а затем отрисовать модель в сцене столько раз, сколько нужно, — без повторного ее преобразования, даже если отрисовка перемежается с изменениями состояния отрисовки. Это удобно делать при отрисовки моделей, в которых используется множество текстур: геометрия преобразовывается только один раз, после чего ее части можно отрисовывать по необходимости, перемежая отрисовку необходимыми изменениями текстур. Изменения состояния отрисовки, происходящие после обработки вершин, вступают в силу при следующей обработке вершин.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>В этом разделе


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Раздел</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="introduction-to-buffers.md">Вводные сведения о буферах</a></p></td>
<td align="left"><p>Буферный ресурс — это коллекция полностью распределенных по типам данных, сгруппированных в элементы. Буферы хранят данные, например координаты текстур в <em>буфере вершин</em>, индексы в <em>буфере индексов</em>, данные констант шейдера в <em>буфере констант</em>, векторы положений, векторы нормалей и состояния устройства.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">Буферы индексов</a></p></td>
<td align="left"><p><em>Буферы индексов</em>— это буферы памяти, содержащие данные индексов, представляющие собой целочисленные указатели для буферов вершин, которые используются для отрисовки примитивов.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Статьи по теме


[Обучающее руководство по графике Direct3D](index.md)

 

 



