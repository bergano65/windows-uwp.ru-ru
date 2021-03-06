---
title: Отключение режима мыши
description: Инструкции по отключению режима мыши по умолчанию.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 1e4b8868f416494daf978d65d4a4ccde02d6ccf5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656629"
---
# <a name="how-to-disable-mouse-mode"></a>Отключение режима мыши
Режим мыши включен по умолчанию для всех приложений; это означает, что все приложения, которые явно не отключили указатель мыши, получают его (аналогично указателю в браузере Microsoft Edge на консоли). Настоятельно рекомендуется отключать эту функцию и выполнять оптимизацию для навигации с помощью направляемого контроллера.   
   
## <a name="html"></a>HTML   
Для включения навигации с помощью направляемого контроллера в приложении для универсальной платформы Windows (UWP) на языке JavaScript используйте библиотеку JavaScript [TVHelpers DirectionalNavigation](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Включите JavaScript-файл для работы с направленной навигацией в пакет приложения и добавьте ссылку на этот файл во все HTML-страницы, на которых требуется использовать навигацию с помощью направляемого контроллера.

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Подробнее см. в [вики-статье, посвященной направленной навигации](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Если вместо этого вы хотите отключить режим мыши и использовать модель DOM или API-интерфейсы WinRT для игрового контроллера напрямую, выполните следующие действия для всех страниц, на которых требуется это сделать. 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   По умолчанию это свойство имеет значение `mouse`, которое включает режим мыши. Если для свойства установить значение `keyboard`, то режим мыши будет отключен и вместо него при получении входных сигналов с игрового контроллера будут генерироваться события клавиатуры модели DOM. Если для свойства установить значение `gamepad`, то режим мыши будет отключен, события клавиатуры модели DOM генерироваться не будут, и вы сможете просто использовать модель DOM или API-интерфейсы WinRT для игрового контроллера.

## <a name="xaml"></a>XAML    
Чтобы отключить режим мыши, добавьте следующий код в конструктор для вашего приложения.   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
Если вы пишете приложение на C++/DirectX, то ничего делать не нужно. Режим мыши применяется только к приложениям на HTML и XAML.

## <a name="see-also"></a>См. также
- [Советы и рекомендации для Xbox](tailoring-for-xbox.md)
- [Приложения UWP для Xbox One](index.md)

