---
author: TerryWarwick
title: Сканер штрихкодов на базе камеры
description: В этой статье перечисляются функции сканера штрихкодов на базе камеры, доступные для приложений UWP, и приводятся ссылки на статьи с инструкциями по их использованию.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, точка обслуживания, POS
ms.localizationpriority: medium
ms.openlocfilehash: dff05e8909c2afd2bac055eead87f4b66892109b
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833317"
---
# <a name="camera-barcode-scanner"></a>Сканер штрихкодов на базе камеры
Сканер штрихкодов на базе камеры создается динамически, когда Windows связывает камеру или камеры, подключенные к компьютеру, с программным декодером.  Каждая пара "камера-декодер" представляет собой полнофункциональный сканер штрихкодов.   

## <a name="in-this-section"></a>В этом разделе
|Раздел |Описание |
|------|------------|
| [Системные требования](pos-camerabarcode-system-requirements.md)  | Список выпусков Windows, которые поддерживают сканеры штрихкодов на базе камеры, а также требования к камере для успешного считывания штрихкодов. |
| [Начало работы](pos-camerabarcode-get-started.md)              | Пошаговое руководство по началу использования камеры в качестве сканера штрихкодов. |
| [Размещение предварительного изображения](pos-camerabarcode-hosting-preview.md)          | Узнайте, как разместить в своем приложении предварительное изображение со сканера штрихкодов на базе камеры. |
| [Включение и отключение](pos-camerabarcode-enable-disable.md)         | Узнайте, как включить или отключить программный декодер, который входит в состав Windows 10. |
| [Поддерживаемые символики](pos-camerabarcode-symbologies.md) | В этой статье приведены примеры штрихкодов для каждой из символик, поддерживаемых программным декодером штрихкодов в Windows 10, в том числе UPC/EAN, Code 39, Code 128, Interleaved 2 of 5, Databar Omnidirectional, Databar Stacked, QR Code и GS1DWCode. |
| 

> [!NOTE]
> Программный декодер, встроенный в Windows 10, любезно предоставлен [*Digimarc Corporation*](https://www.digimarc.com/).