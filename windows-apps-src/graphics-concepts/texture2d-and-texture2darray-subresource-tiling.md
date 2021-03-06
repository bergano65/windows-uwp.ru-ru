---
title: Размещение на плитках вложенных ресурсов Texture2D и Texture2DArray
description: В таблицах ниже показано, как размещаются на плитках вложенные ресурсы Texture2D и Texture2DArray.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Размещение на плитках вложенных ресурсов Texture2D и Texture2DArray
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e193ab7bce31c1f13cb40f04902922c6ff21056
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370917"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Размещение на плитках вложенных ресурсов Texture2D и Texture2DArray


В таблицах ниже показано, как размещаются на плитках вложенные ресурсы [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) и [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray). Значения в этих таблицах не учитывают упаковку хвостовых MIP-карт.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Вспомогательных ресурсах с множественное число 1


В этой таблице показано, как размещаются на плитках вложенные ресурсы [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) и [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) с множественной дискретизацией и одной выборкой.

| Бит/пкс (1 семпл/пкс) | Размеры плиток (пиксели, ШxВ) |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128x64                        |
| 128                         | 64 x 64                         |
| BC1,4                       | 512x256                       |
| BC2,3,5,6,7                 | 256x256                       |

 

Количество бит формат не поддерживается с помощью потоковой передачи ресурсов форматы 96 bpp, форматы видео DXGI\_ФОРМАТ\_R1\_UNORM, DXGI\_ФОРМАТ\_R8G8\_B8G8\_UNORM, и DXGI\_ФОРМАТ\_R8R8\_G8B8\_UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Вспомогательных ресурсов с различным количеством мультисэмплинга


В этой таблице показано, как размещаются на плитках вложенные ресурсы [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) и [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) с множественной дискретизацией и различным числом выборок.

| Бит/пкс (1 семпл/пкс) | Размеры плиток (пиксели, ШxВ) |
|-----------------------------|-------------------------------|
| 1                           | 1x1                           |
| 2                           | 2x1                           |
| 4                           | 2x2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

Требуется и разрешена поддержка только 1 и 4 выборок для потоковых ресурсов. Потоковые ресурсы в настоящее время не поддерживают 2, 8 и 16 выборок, несмотря на то, что они отображаются.

При реализации можно выбрать поддержку режима сглаживания с множественной дискретизацией (MSAA) с 2, 8 и 16 выборками для ресурсов, не являющихся потоковыми, несмотря на то, что потоковые ресурсы это не поддерживают.

Потоковые ресурсы с количеством выборок больше одной не могут использовать форматы 128 бит/пкс.

Ограничения на количество поддерживаемых выборок и форматы связаны с несогласованностью оборудования и спецификации.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Как выполняется мозаичное заполнение области потоковой передачи ресурсов](how-a-streaming-resource-s-area-is-tiled.md)

 

 




