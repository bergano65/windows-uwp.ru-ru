---
Description: Проектирование и разработка приложения таким образом, чтобы оно правильно работало на системах с различными конфигурациями языка и региона.
Search.Refinement.TopicID: 180
title: Руководство по глобализации
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, глобализация, локализуемость, локализация
ms.localizationpriority: medium
ms.openlocfilehash: 18c68baf991b3fd939a6e6ee681700977a5a5eb9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258088"
---
# <a name="guidelines-for-globalization"></a>Руководство по глобализации

Проектирование и разработка приложения таким образом, чтобы оно правильно работало на системах с различными конфигурациями языка и региона. Используйте API-интерфейсы [**Globalization**](/uwp/api/Windows.Globalization?branch=live) для форматирования данных; избегайте предположений в коде в отношении языка, региона, классификации символов, системы письменности, форматирования данных/времени, чисел, валют, систем измерения веса и правил сортировки.

| Рекомендации | Описание |
| ------------- | ----------- |
| Учитывайте язык и региональные параметры при обработке и сравнении строк. | Например, не изменяйте регистр строк до их сравнения. См. раздел [Рекомендации по использованию строк](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| При сортировке строк и других данных не следует предполагать, что она всегда выполняется в алфавитном порядке. | Для языков, не использующих латиницу, сортировка основана на таких факторах, как произношение или число росчерков пера. Даже для языков, основанных на латинице, сортировка по алфавиту используется не всегда. Например, в некоторых культурах записи в телефонной книге не сортируются в алфавитном порядке. Windows может провести сортировку за вас, но если вы создаете собственный алгоритм сортировки, следует учесть методы сортировки, принятые на ваших целевых рынках. |
| Используйте правильный формат для чисел, дат, времени, адресов и телефонных номеров. | Эти форматы отличаются в зависимости от культуры, региона, языка и рынка. Если ваше приложение отображает эти данные, для получения формата, подходящего для определенной аудитории, используйте API[**Globalization**](/uwp/api/Windows.Globalization?branch=live). См. раздел [Глобализации форматов даты/времени/чисел](use-global-ready-formats.md). Порядок, в котором отображаются фамилии и имена, а также формат адреса, также могут различаться. Используйте стандартный формат отображения даты, времени и чисел. Используйте стандартные элементы управления выбором даты и времени. Используйте стандартные адресные сведения. |
| Поддержка международных единиц измерения и валют. | В разных странах используются разные единицы измерения и шкалы, самыми распространенными являются метрическая и английская системы. Если ваше приложение использует измерения, например длину, температуру или площадь, убедитесь, что они указаны в правильной системе измерения. Используйте свойство [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse), чтобы получить набор валют, которые используются в вашем регионе. |
| Используйте Юникод для кодировки. | По умолчанию в Microsoft Visual Studio используются Юникод для всех документов. Если вы используете другой редактор, убедитесь, что исходные файлы сохранены в соответствующей кодировке Юникода. Все API-интерфейсы UWP возвращают строки с кодировкой UTF-16. |
| Поддержка международных размеров бумаги. | Разные страны используют разные размеры бумаги, поэтому, если ваше приложение поддерживает возможности, зависящие от размера бумаги, например печать, оно должно поддерживать и распространенные международные размеры бумаги. |
| Записывайте язык клавиатуры или идентификатор IME. | Когда ваше приложение предлагает пользователям ввести текст, запишите тег языка для включенной в данный момент раскладки клавиатуры или редактора метода ввода (IME). Благодаря такой записи при последующем отображении введенной информации будет использовано правильное форматирование. Используйте свойство [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) для получения текущего языка ввода. |
| Не используйте язык пользователя для определения его региона и не используйте регион пользователя для определения его языка. | Язык и регион представляют собой разные понятия. Пользователь может говорить на региональном варианте языка, например, на британском английском (en-GB), а находиться в совершенно другой стране или другом регионе. Решите, нужны ли вашему приложению данные о языке пользователя (например, для текста пользовательского интерфейса) или его регионе (например, для лицензионного соглашения). Дополнительные сведения см. в разделе [Обзор языков профиля пользователя и языков манифеста приложения](manage-language-and-region.md). |
| Правила сравнения языковых тегов являются нестандартными. | [Языковые теги BCP-47](https://tools.ietf.org/html/bcp47) являются сложными. При сравнении языковых тегов возникает ряд проблем, например с информацией о соответствии сценариев, устаревшими тегами и несколькими региональными вариантами. Система управления ресурсами в Windows решает за вас проблему соответствия. Вы можете указать набор ресурсов на любом языке, система сама выберет подходящий для пользователя и приложения язык. См. разделы [Ресурсы приложения и Система управления ресурсами](../../app-resources/index.md) и [Как Система управления ресурсами сопоставляет языковые теги](../../app-resources/how-rms-matches-lang-tags.md). |
| Проектируйте пользовательский интерфейс с учетом текста разной длины и размеров шрифтов для меток и элементов управления вводом текста. | Длина строк, переведенных на разные языки, может сильно различаться, поэтому вам понадобятся элементы управления пользовательского интерфейса для динамического изменения размера их содержимого. Распространенные буквы в других языках могут иметь значки над буквой или под ней (например, Å или Ņ). Используйте стандартные размеры шрифтов и высоту строк для обеспечения достаточного пространства по вертикали. Имейте в виду, что для шрифтов других языков может потребоваться использовать более крупные минимальные размеры шрифта, чтобы они оставались отчетливыми. Ознакомьтесь с классами в пространстве имен [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live). |
| Обеспечьте поддержку зеркального отображения направления чтения. | Выравнивание текста и чтение могут выполняться слева направо, как в английском языке, или справа налево, как в арабском или иврите. Если вы локализуете ваш продукт для языков с отличным от вашего направлением чтения, убедитесь, что структура элементов вашего пользовательского интерфейса поддерживает зеркальное отображение. Зеркального отображения могут требовать даже такие элементы, как кнопки "Назад", эффекты перехода пользовательского интерфейса и изображения. Дополнительные сведения см. в разделе [Настройка макета и шрифтов, реализация поддержки написания справа налево](adjust-layout-and-fonts--and-support-rtl.md). |
| Отображайте текст и шрифты правильно. | Общепринятый шрифт, его размер и направление текста отличаются в зависимости от рынка. Дополнительные сведения см. в разделах [**Настройка макета и шрифтов, реализация поддержки написания справа налево**](adjust-layout-and-fonts--and-support-rtl.md) и [Международные шрифты](loc-international-fonts.md). |

## <a name="important-apis"></a>Важные API
 
* [Globalization](/uwp/api/Windows.Globalization?branch=live)
* [Жеографикрегион. КурренЦиесинусе](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language. Куррентинпутмесодлангуажетаг](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows. Globalization. шрифты](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>См. также

* [Рекомендации по использованию строк](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Глобализация форматов даты, времени и чисел](use-global-ready-formats.md)
* [Общие сведения о языках профилей пользователей и языках манифестов приложений](manage-language-and-region.md)
* [Теги языка BCP-47](https://tools.ietf.org/html/bcp47)
* [Ресурсы приложений и система управления ресурсами](../../app-resources/index.md)
* [Как система управления ресурсами сопоставляет языковые теги](../../app-resources/how-rms-matches-lang-tags.md)
* [Настройка макета и шрифтов, добавление поддержки написания справа налево](adjust-layout-and-fonts--and-support-rtl.md)
* [Международные шрифты](loc-international-fonts.md)
* [Сделайте приложение локализованным](prepare-your-app-for-localization.md)

## <a name="samples"></a>Примеры

* [Пример настроек глобализации](https://code.msdn.microsoft.com/windowsapps/Globalization-preferences-6654eb36)
