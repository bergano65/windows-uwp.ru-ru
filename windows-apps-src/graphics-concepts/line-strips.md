---
title: Ломаные
description: Ломаная — это примитив, состоящий из соединенных отрезков. Приложение может использовать ломаные линии для создания незамкнутых многоугольников. Замкнутый многоугольник — это многоугольник, последняя вершина которого соединяется отрезком с его первой вершиной.
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords:
- Ломаные
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7eec406faf1f695a473154dd23322bcdf9189049
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043163"
---
# <a name="line-strips"></a>Ломаные


Ломаная — это примитив, состоящий из соединенных отрезков. Приложение может использовать ломаные линии для создания незамкнутых многоугольников. Замкнутый многоугольник — это многоугольник, последняя вершина которого соединяется отрезком с его первой вершиной. Если приложение создает многоугольники на основе ломаных, вершины могут находиться в разных плоскостях.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Пример


На следующем рисунке показана отрисованная ломаная линия.

![иллюстрация ломаной линии](images/linstrip.gif)

Следующий код показывает, как создать вершины для такой ломаной линии.

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

В примере кода ниже показано, как отрисовать ломаную линию в Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Статьи по теме


[Примитивы](primitives.md)

 

 



