---
title: Настройка сканера штрихкодов
description: Сведения о настройке сканер штрих-кодов для нужное приложение.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, точка обслуживания, POS
ms.localizationpriority: medium
ms.openlocfilehash: 8466c56ef73a1c38c67e28cf52de7f380e6c563a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321584"
---
# <a name="configure-a-barcode-scanner"></a>Настройка сканера штрихкодов

Настраивать сканеры штрихкодов можно в нескольких режимах.  Очень важно, чтобы сканер штрихкодов был настроен в соответствии с его предполагаемым применением.

Многие сканеры штрихкодов могут быть настроены в режиме **разрыва клавиатуры**, в результате чего Windows воспринимает сканер штрихкодов как клавиатуру.  Это позволяет сканировать штрихкоды в приложения, которые не предназначены непосредственно для работы со сканером штрихкодов, например в Блокнот.  При сканировании штрихкода в этом режиме декодированные данные со сканера штрихкодов вставляются в точку вставки, как будто бы вы ввели эти данные с клавиатуры.  Если вы хотите в большей степени контролировать сканер штрихкодов из своего приложения UWP, вам понадобится настроить его в другом режиме.

## <a name="usb-barcode-scanner"></a>Сканер штрихкодов, подключаемый по USB
Сканер штрихкодов, подключаемый по USB, должен быть настроен в режиме **Сканер POS HID**, чтобы он мог работать с драйвером сканера, входящим в состав Windows. Этот драйвер представляет собой реализацию **HID точки продажи использования таблиц** спецификации публикуются в [USB HID](https://www.usb.org/hid).  Чтобы узнать, как включить сканер штрихкодов в режиме **Сканер POS HID**, обратитесь к документации сканера или свяжитесь с производителем сканера.  Будучи настроенным как **Сканер POS HID**, ваш сканер штрихкодов появится в диспетчере устройств в узле **Сканер штрихкодов пункта обслуживания** и будет называться **Сканер штрихкодов POS HID**.

У производителя сканера штрихкодов также может быть собственный драйвер, который поддерживает API-интерфейсы для сканеров штрихкодов в UWP и использует другой режим (не **Сканер POS HID**).  Если вы уже установили от изготовителя драйвер, совместимый с API-интерфейсы сканера штрихкодов универсальной платформы Windows, может появиться поставщика устройства, перечисленные в разделе **сканер штрихкодов POS** в диспетчере устройств.

## <a name="bluetooth-barcode-scanner"></a>Сканер штрихкодов, подключаемый по Bluetooth
Сканер штрихкодов, подключаемый по Bluetooth, для работы с API-интерфейсами для сканеров штрихкодов в UWP должен быть настроен в режиме **Протокол последовательного порта - простой последовательный интерфейс (SPP-SSI)** .  Чтобы узнать, как включить сканер штрихкодов в режиме **SPP-SSI**, обратитесь к документации сканера или свяжитесь с производителем сканера.

Перед тем как использовать сканер штрихкодов Bluetooth необходимо связать его с помощью **параметры > устройства > Bluetooth & другие устройства > Добавить Bluetooth или другое устройство**.

Можно инициировать и контролировать связывания процесс, используя [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) пространства имен.  См. в разделе [устройств пары](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) Дополнительные сведения.

[!INCLUDE [feedback](./includes/pos-feedback.md)]