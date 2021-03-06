---
title: Форматы сжатых текстур
description: Этот раздел содержит сведения о внутренней организации форматов сжатых текстур.
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- Форматы сжатых текстур
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3171eba376911157a6ad2687fe3879df751615ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662199"
---
# <a name="compressed-texture-formats"></a>Форматы сжатых текстур


Этот раздел содержит сведения о внутренней организации форматов сжатых текстур. Эти сведения не нужны для того, чтобы использовать сжатые текстуры, потому что для преобразования форматов можно использовать функции Direct3D. Однако эта информация окажется полезной, если потребуется выполнять операции со сжатыми поверхностными данными напрямую.

В Direct3D используется формат сжатия, который делит карты текстур на текстовые блоки 4x4. Если текстура не имеет прозрачности (то есть является непрозрачной) или прозрачность задана 1-разрядным альфа-значением, 8-байтный блок представляет блок карты текстуры. Если карта текстуры не содержит прозрачных текселей, использующих альфа-канал, его представляет 16-байтный блок.

Любая текстура должна указывать, что ее данные хранятся по 64 или 128 битов на группу из 16 текселей. Если 64-разрядные блоки (то есть формат BC1) используются для текстуры, можно смешать непрозрачные и 1-разрядные альфа-форматы в блоках одной текстуры. Другими словами, сравнение абсолютное значение целого числа без знака цвет\_0 и цвет\_1 выполняется уникально для каждого блока 16 текселя.

Если используются 128-разрядные блоки, необходимо задать альфа-канал в явном (формат BC2) или интерполированном режиме (формат BC3) для всей текстуры. Как и с цветом, при использовании интерполированного режима на уровне блоков можно использовать режим восьми или шести интерполированных альфа-каналов. Еще раз сравнения абсолютное значение альфа-канал\_0 и альфа-канал\_1 однозначно выполняется на основе блок за блоком.

Шаг шрифта для форматов BCn измеряется в байтах (не блоках). Например, если у вас есть ширину, равную 16, будет создана шаг четыре блока (4\*8 для BC1, 4\*16 для BC2 или BC3).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Сжатые текстурными ресурсами](compressed-texture-resources.md)

 

 




