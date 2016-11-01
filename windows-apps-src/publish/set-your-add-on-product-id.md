---
author: jnHs
Description: "При создании новой надстройки на информационной панели Центра разработки для Windows вам потребуется указать тип продукта и назначить ему код продукта."
title: "Установка типа и кода продукта для надстройки"
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
translationtype: Human Translation
ms.sourcegitcommit: e59324aca65cf8baacb085da22a20d952fdb8c9a
ms.openlocfilehash: 2a469506c8b440e1aa8555ac57b88f2026ae4d8e

---

# Установка типа и кода продукта для надстройки

Надстройка должна быть связана с приложением, которое вы уже создали на информационной панели (даже если еще не отправили его). Вы можете найти кнопку **Создать новую надстройку** на странице **Обзор** или на странице **Надстройки** вашего приложения.

После нажатия этой кнопки появится страница **Создать новую надстройку**. Здесь необходимо указать тип продукта и назначить ему код продукта.

## Тип продукта

Сначала необходимо указать, какой тип надстройки вы предлагаете. Это выбор относится к тому, как клиенты могут использовать вашу надстройку.

> **Примечание.** Невозможно изменить тип продукта после сохранения этой страницы для создания надстройки. Если вы выбрали неверный тип продукта, вы всегда сможете удалить текущую надстройку и начать заново, создав новую.

Если продукт можно приобрести, использовать (потребить), а затем приобрести еще раз, необходимо будет выбрать один из **потребляемых** типов продукта. Потребляемые надстройки часто используются для таких вещей, как игровая валюта (золото, монеты и т. д.), которая может приобретаться определенными суммами, а затем использоваться пользователем. Дополнительные сведения о включении потребляемых надстроек в ваше приложение см. в разделе [Поддержка приобретения потребляемых надстроек](../monetize/enable-consumable-add-on-purchases.md).

Существует два типа потребляемых надстроек, которые можно выбрать:

- **Управляемые разработчиком потребляемые надстройки**: поддерживаются во всех версиях ОС. Управление балансом и выполнением осуществляется в приложении. 
- **Управляемые разработчиком потребляемые надстройки**: баланс отслеживается Microsoft для всех устройств клиента, работающих под управлением Windows 10 версии 1607 или выше; не поддерживается в более ранних версиях ОС. Чтобы использовать этот вариант, родительский продукт должен быть скомпилирован с помощью Windows 10 SDK версии 14393 или выше. Обратите внимание, что невозможно отправить управляемую Магазином потребляемую надстройку в Магазин до тех пор, пока родительский продукт не опубликован (хотя можно создать отправку на информационной панели и в любой момент начать работать с ней). Потребуется ввести количество для управляемой Магазином потребляемой надстройки на странице **Свойства**.

Необходимо выбрать значение **Длительного пользования**, если продукт можно приобрести однократно. Надстройки длительного пользования часто используются для разблокировки дополнительных функциональных возможностей приложения. Надстройки длительного пользования не потребляются, поэтому вы можете задать **Срок службы продукта**, чтобы он становился негодным через определенный промежуток времени (доступен выбор от 1 до 365 дней). Для надстройки длительного пользования **Срок службы продукта** по умолчанию имеет значение **Всегда**, то есть срок службы надстройки никогда не заканчивается. Вы можете выбрать другую длительность на этапе [Свойства надстройки](enter-add-on-properties.md) процесса отправки надстройки.

## Код продукта

Введите уникальный код продукта для надстройки. Этот тот же идентификатор, который понадобится для указания [в коде приложения с целью вызова надстройки](https://msdn.microsoft.com/library/windows/apps/mt219684).

Вот несколько вещей, которые следует помнить при выборе кода продукта:

-   Клиенты не будут видеть этот код продукта. (Позже можно ввести [заголовок и описание](create-add-on-descriptions.md) для отображения пользователям).
-   Вы не сможете изменять или удалять код продукта надстройки после публикации.
-   Длина кода продукта не может быть больше 100 символов.
-   Код продукта может включать любые из следующих символов: **&lt; &gt; \* % & : \\ ? + ,**
-   Чтобы предложить надстройку на всех устройствах, необходимо использовать только буквенно-цифровые символы, точки и знаки подчеркивания. Если вы используете любые другие типы символов, надстройка не будет доступна для покупки клиентам, использующим Windows Phone 8.1 или более ранние версии.
-   Код продукта не должен быть уникальным в Магазине Windows, однако он должен быть уникальным для вашей учетной записи разработчика.
 







<!--HONumber=Aug16_HO5-->

