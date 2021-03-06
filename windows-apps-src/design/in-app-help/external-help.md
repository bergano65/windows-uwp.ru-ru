---
Description: Проектируйте внешние страницы справки так, чтобы они содержали подробные инструкции и советы по вашему приложению.
title: Руководство по разработке внешних страниц справки
label: External help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 56afd553-c520-4a28-b63d-2e1b3c1d3606
ms.localizationpriority: medium
ms.openlocfilehash: eaca2af3a497de75beaffe5d3af4a261b24d8ba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617179"
---
# <a name="external-help-pages"></a>Страницы внешней справки



Если в приложении требуется подробная справка для выполнения сложных задач, возможно, эти инструкции следует разместить на веб-странице.

## <a name="when-to-use-external-help-pages"></a>Когда следует использовать внешние страницы справки

Внешние страницы справки менее удобны для общего использования или быстрого получения информации. Они подходят для слишком большого по объему содержимого справки, которое сложно вставить в само приложение, а также для обучающих материалов и инструкций по расширенным функциям приложения, которые не будут использованы общей аудиторией.

Если содержимое справки короткое или достаточно специфичное для отображения в приложении, необходимо пойти по этому пути. Не направляйте пользователей за пределы приложения для получения справки, если только это не является необходимым.

## <a name="navigating-external-help-pages"></a>Навигация по внешним страницам справки

Если пользователь направлен на внешнюю страницу справки, придерживайтесь одного из следующих двух сценариев.
-   Пользователь переходит непосредственно на страницу, которая соответствует описываемой проблеме. Это контекстная справка, которая должна использоваться по возможности.
-   Они привязаны к общей странице справки с четким отображением выбираемых категорий и подкатегорий.

Предоставление пользователям возможностей поиска по справке может оказаться полезным, однако не делайте этот поиск единственным способом навигации по справке. Иногда пользователям может быть трудно описать свою проблему, что сделает поиск достаточно сложной задачей. Пользователи должны быстро находить страницы, связанные с их проблемами, без поиска.

## <a name="tutorials-and-detailed-walkthroughs"></a>Учебные материалы и подробные пошаговые инструкции

Внешние страницы справки являются идеальным местом для предоставления пользователям обучающих материалов и пошаговых инструкций в виде текста или видео.
-   В учебных материалах внимание должно уделяться сложным вопросам и расширенным функциям. Пользователи не должны испытывать необходимость в использовании учебных материалов для работы с приложением.
-   Убедитесь, что эти учебные материалы отображаются отдельно от стандартных справочных инструкций. Пользователи, которым необходимы расширенные инструкции, будут искать их с большей готовностью, чем пользователи, которым необходим простой способ устранения проблем.
-   Рассмотрите возможность предоставления ссылок на учебные материалы как из каталога, так и с отдельных страниц справки, соответствующих тому или иному руководству.

## <a name="related-articles"></a>Связанные статьи

* [Рекомендации для получения справки приложения](guidelines-for-app-help.md)
