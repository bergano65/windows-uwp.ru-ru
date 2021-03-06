---
title: Конвейерный доступ к потоковым ресурсам
description: Потоковые ресурсы можно использовать в представлениях ресурсов шейдера (SRV), представлениях целевого объекта отрисовки (RTV), представлениях трафарета глубины (DSV) и представлениях неупорядоченного доступа (UAV), а также как точки привязки, где представления не используются, например как привязки буфера вершин.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Конвейерный доступ к потоковым ресурсам
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b10ba23db301a675bf102fd8fb6e278dbba11da8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371024"
---
# <a name="pipeline-access-to-streaming-resources"></a>Конвейерный доступ к потоковым ресурсам


Потоковые ресурсы можно использовать в представлениях ресурсов шейдера (SRV), представлениях целевого объекта отрисовки (RTV), представлениях трафарета глубины (DSV) и представлениях неупорядоченного доступа (UAV), а также как точки привязки, где представления не используются, например как привязки буфера вершин. Список поддерживаемых привязок см. в разделе [Параметры создания потоковых ресурсов](streaming-resource-creation-parameters.md). Различные операции копирования D3D также работают с потоковыми ресурсами.

Если несколько координат плиток в одном или нескольких представлениях привязаны к одной области памяти, операции чтения и записи для различных путей в одной области памяти являются недетерминистическими и неповторяемыми.

Если все плитки, связанные с доступом к памяти шейдера, сопоставляются с уникальными плитками, поведение для всех реализаций будет одинаковым, т. е. на поверхности будет одинаковое содержимое памяти без плиток.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Поведение SRV с несопоставленные плитки</a></p></td>
<td align="left"><p>Поведение операций чтения представления ресурса шейдера (SRV), включающих несопоставленные плитки, зависит от уровня поддержки оборудования.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Поведение UAV с несопоставленные плитки</a></p></td>
<td align="left"><p>Поведение операций чтения и записи представлений неупорядоченного доступа (UAV) зависит от уровня поддержки оборудования.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Поведение растеризации с несопоставленные плитки</a></p></td>
<td align="left"><p>В этом разделе описывается поведение растеризации несопоставленных плиток.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Ограничения доступа плитки с повторяющиеся сопоставления</a></p></td>
<td align="left"><p>Существуют ограничения на доступ к плиткам с повторяющимися сопоставлениями, например при копировании потоковых ресурсов с перекрывающимися источником и назначением или при отрисовке общих плиток для области отрисовки.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Потоковой передачи ресурсов текстуры функции выборки</a></p></td>
<td align="left"><p>Дискретизация текстур потоковых ресурсов включает в себя обратную связь от шейдера о состоянии сопоставленных областей. При этом проверяется, были ли все данные, к которым запрашивается доступ, сопоставлены в ресурсе, выполняется сжатие, чтобы помочь шейдерам избежать областей в потоковых ресурсах, которые не были сопоставлены, и определяется минимальный уровень детализации, полностью сопоставленной для всего фильтра текстуры.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Ресурсы раскрытия HLSL для потоковой передачи</a></p></td>
<td align="left"><p>Для поддержки потоковых ресурсов в <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5">модели шейдера 5</a> требуется определенный синтаксис HLSL.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Потоковые ресурсы](streaming-resources.md)

 

 




