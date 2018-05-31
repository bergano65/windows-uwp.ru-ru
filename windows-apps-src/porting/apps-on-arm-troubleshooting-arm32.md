---
title: Устранение неполадок в приложениях UWP ARM32
author: msatranjr
description: Распространенные проблемы с приложениями ARM32 при работе в ARM и способы их устранения.
ms.author: misatran
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, постоянно подключенный, приложения ARM32 в ARM, windows 10 в ARM, устранение неполадок
ms.localizationpriority: medium
ms.openlocfilehash: 71d92ec26311514e0eebdfa4a1dab39e86ce72fc
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595184"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>Устранение неполадок в приложениях UWP ARM32
Если приложение UWP ARM32 неправильно работает в ARM, вот некоторые советы, которые могут вам помочь. 

## <a name="common-issues"></a>Общие проблемы
Вот несколько распространенных проблем, о которых нужно помнить, устраняя неполадки в приложениях ARM32.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Использование API с поддержкой только Windows 10 Mobile в процессорах на базе ARM 
В работе приложений ARM32 могут возникать проблемы при использовании API только для мобильных устройств (например, **HardwareButtons**). Чтобы решить эту проблему, можно динамически определить, выполняется ли ваше приложение в Windows 10 Mobile, прежде чем вызывать эти API. Изучите инструкции в записи в блоге [Динамическое обнаружение функций с контрактами API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Включение зависимостей, не поддерживаемых приложениями UWP
Приложения универсальной платформы Windows (UWP), созданные в Visual Studio и SDK UWP с ошибками, могут зависеть от компонентов ОС, которые недоступны приложениям ARM32, выполняемым в системе ARM64. Примеры таких зависимостей:

- Ожидание доступности частей .NET Framework.
- Ссылка на сторонние компоненты .NET, которые несовместимы с UWP.

Эти проблемы можно решить следующим образом: удалив недоступные зависимости и перестроив приложение с помощью новейших версий Microsoft Visual Studio и SDK UWP; кроме того, в крайнем случае можно удалить приложение ARM32 из Microsoft Store, чтобы версия x86 этого приложения (если доступна) загрузилась на ПК пользователей. 

Дополнительные сведения об API .NET, доступных для приложений UWP, см. в разделе [.NET для приложений UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Компиляция приложения с более старой версией Visual Studio и SDK
Если возникнут проблемы, воспользуйтесь для компиляции приложения последними версиями Microsoft Visual Studio и Windows SDK. В приложениях, скомпилированных с помощью более ранней версии Visual Studio и SDK, могут возникать проблемы, устраненные в более поздних версиях.

## <a name="debugging"></a>Отладка
Для разработки приложений ARM32 для платформы ARM можно использовать существующие инструменты. Вот некоторые полезные ресурсы.

- Visual Studio 15.5 Preview 1 и выше поддерживает запуск приложений ARM32 в режиме универсальной аутентификации. При этом автоматически начинается начальная загрузка необходимых инструментов удаленной отладки.
- Дополнительные сведения об инструментах и стратегиях отладки в ARM см. в разделе [Отладка в ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64).