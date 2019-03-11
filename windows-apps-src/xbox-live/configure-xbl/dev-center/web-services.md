---
title: Веб-службы
description: Узнайте, как создать веб-службу для приложения
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10, xbox, один, веб-служб
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600019"
---
# <a name="set-up-web-services-in-udc"></a>Настройка веб-служб в UDC

> [!WARNING]
> Приведенная ниже статья содержит для ID@Xbox и управляемым партнерским разработчики только из-за ограничений, налагаемых на конфигурацию веб-службы. Настройка веб-служб доступен только для разработчиков с проверяющим сторонам учетной записи уровня разрешения. Если у вас нет элемента управления из вашей учетной записи есть разрешения на уровне диспетчера учетной записи разработки (DAM) обратитесь за помощью.

Издатели могут создать веб-службы, если необходимо настроить их приложений и заголовки взаимодействия со службами Xbox Live. Веб-служб являются конфигураций издателя и могут быть вызваны любой заголовок в "песочнице" принадлежит издателю путем настройки единого входа.

Причины для определения веб-служб:

1. Предоставление единого входа для пользователей Xbox Live — в порядке веб-службы для предоставления единого входа пользователям, Xbox Live, ему необходимо настроить в качестве проверяющей стороны Xbox Live. Если настроена таким образом, пользователи, авторизованные для Xbox Live будет автоматически пройти проверку подлинности в службе без необходимости повторного ввода другой набор учетных данных.
2. Вызовы служб из службы к службам Xbox Live - Если ваш продукт будет использовать один из веб-служб для звонков в службу Xbox Live, либо непосредственно, либо от имени отдельных пользователей, вам потребуется сертификат партнера бизнеса.

1. ## <a name="create-a-web-service"></a>Создание веб-службы

1. Перейдите к [мониторинга центра партнеров](https://partner.microsoft.com/dashboard/windows/overview)  
2. Щелкните значок шестеренки в форме в правом верхнем углу страницы, чтобы получить доступ к **параметры** раскрывающегося списка.
3. В раскрывающемся списке выберите **параметров разработчика**.
4. На панели навигации слева разверните параметр **Xbox Live** и выберите **веб-служб**.

![Web services gif ](../../images/dev-center/web-services/web-services.gif)

5. На странице веб-служб щелкните **новой веб-службы**.
6. Введите имя веб-службы и выберите тип доступа, при необходимости.  
    * Доступ к телеметрии позволяет службе для получения данных телеметрии игры для любого из игры.
    * Доступ приложения канал позволяет поставщик мультимедиа, владеющий службы полномочия на публикация каналы приложения для использования на консоли через трюк OneGuide программным образом.
7. Нажмите кнопку **Сохранить**.

На этом этапе вы определили службы и Xbox Live знать о существовании службы. В зависимости от причины для создания веб-службы необходимо будет настроить Parties(Single Sing-On) проверяющей стороны или Certificates(Service-to-service calls) деловых партнеров.  

## <a name="configure-relying-party"></a>Настройка проверяющей стороны

Веб-служба должна быть настроена как Доверяющая сторона Xbox live для работы единого входа для пользователей Xbox Live — пользователи, авторизованные для Xbox Live будет быть автоматически проходит проверку подлинности в веб-службу без необходимости повторного ввода другой набор учетных данных. Для этого необходимо установить доверие между службами Xbox и веб-службы. Набор утверждений (например: тег игрока, тип устройства, идентификатор title) используются как часть конфигурации доверяющей стороной для принудительного применения этого отношения доверия. Это сведения, передаваемые между Xbox Live и веб-службы, помогающие автоматически проверять подлинность пользователей.

### <a name="create-a-relying-party"></a>Создание проверяющей стороны

1. Перейдите к панели мониторинга центра партнеров  
2. Щелкните значок шестеренки в форме в правом верхнем углу страницы, чтобы получить доступ к **параметры** раскрывающегося списка.
3. В раскрывающемся списке выберите **параметров разработчика**.
4. На панели навигации слева разверните параметр **Xbox Live** и выберите **проверяющим сторонам**.
5. На странице проверяющим сторонам, щелкните **новый проверяющая сторона**.
6. Введите URI для проверяющей стороны в следующем формате: *example.com*.
7. Выберите тип шифрования, используемый для обеспечения безопасности службы доверяющей стороной.
8. Если вы выбрали симметричного шифрования с использованием общих ключей на предыдущем шаге, щелкните **создать новый ключ** получить новый общий ключ. Следуйте инструкциям на экране для безопасного сохранения ключа.
9. Введите **время жизни маркера** в часах.
10. В разделе **утверждений**, раскрывающийся список содержит список утверждений, которые службе проверяющая сторона может использовать для проверки подлинности. Выберите все утверждения, которые вы хотите использовать. Выбранные утверждения отображается под раскрывающегося списка. Некоторые стандартные утверждения будут заполнены в этой области по умолчанию.
11. Нажмите кнопку **Сохранить** после завершения.  

## <a name="configure-a-business-partner-certificate"></a>Настроить сертификат деловых партнеров

Если ваш продукт будет использовать один из веб-служб для вызовов к службе Xbox Live, либо непосредственно, либо от имени отдельных пользователей, потребуется сертификат партнера бизнеса.

### <a name="generate-a-business-partner-certificate"></a>Создать сертификат деловых партнеров

После успешного создания веб-службы выполните действия ниже.  

1. На странице веб-служб найдите веб-службы, необходимо связать сертификат деловых партнеров.
2. Выберите **создать сертификат** связи с выбранным веб-службы.
3. Щелкните **Показать параметры** рядом с создания нового сертификата. Отображает команды, которые должны выполняться с помощью PowerShell с правами администратора.
4. Выполнения всех команд после другой успешно следует предоставить Base64-кодировке BLOB-объектов. Это открытый ключ. Скопируйте открытый ключ с помощью PowerShell и вставьте его в заполнителе CSP большого двоичного объекта.
5. Нажмите кнопку **загрузить** и следуйте инструкциям на странице для привязки сертификатов.
    1. Используйте компьютере, который использовался для создания открытого ключа.
    2. Выполните следующую команду в PowerShell: *mmc.exe*
    3. Выберите файл и выберите Добавить/удалить оснастку.
    4. Выберите сертификаты и выберите Add. Обязательно выберите учетную запись компьютера для сертификата, а затем щелкните "Готово" и нажмите кнопку ОК.
    5. Откройте хранилище Personal\Certificate.
    6. Щелкните правой кнопкой мыши на сертификаты, выберите все задачи и выберите Import.
    7. Выберите сертификат, загруженный из UDC.
    8. Щелкните правой кнопкой мыши сертификат в пользовательском Интерфейсе после был импортирован, выберите все задачи и выберите Экспорт.
    9. Следуйте указаниям мастера экспорта и обязательно выберите, чтобы экспортировать закрытый ключ с сертификатом.
    10. Завершите работу мастера экспорта.