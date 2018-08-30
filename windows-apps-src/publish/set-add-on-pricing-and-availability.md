---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: Настройки цен и доступности надстройки
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, надстройки, iap, цена
ms.localizationpriority: medium
ms.openlocfilehash: b5b7a6424fea3d62849e992f56b0b40ab72a55f5
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "2909524"
---
# <a name="set-add-on-pricing-and-availability"></a>Настройки цен и доступности надстройки


При отправке надстройки параметры на странице **Цена и доступность** определяют цену на вашу надстройку, а также то, как она должна предоставляться клиентам.

## <a name="markets"></a>Рынки

По умолчанию ваша надстройка будет отображаться по базовой цене на всех возможных рынках, включая любые последующие рынки, которые могут быть добавлены позже.

Однако как и в случае с приложением у вас есть возможность выбрать рынки, на которых следует предоставлять вашу надстройку. В большинстве случаев рекомендуется выбрать тот же набор рынков, что и для приложения, однако у вас есть возможность вносить изменения при необходимости. 

Дополнительные сведения и полный список доступных стран см. в разделе [Выбор определенных стран](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Видимость

Можно указать, следует ли предлагать вашу надстройку пользователям для покупки. 

Значением по умолчанию является— **Может отображаться в описании в Магазине родительского продукта**. Оставьте этот параметр включенным для надстроек, которые будут доступны для любого пользователя. 

Для надстроек, которые вы не хотите делать доступными всем пользователям, выберите **Скрыто в Магазине**, а также один из следующих параметров.

-   **Доступна для покупки только из родительского продукта** выбор этого параметра позволяет любому пользователю приобретать надстройку в самом приложении, однако она не будет отображаться в описании приложения в Магазине. Используйте этот параметр только в том случае, если предложение не является широко доступным, например на начальных этапах внутреннего тестирования.
-   **Остановить приобретение: любой клиент при наличии прямой ссылки увидит описание продукта в Магазине, но скачать его сможет, только если продукт был у него ранее или у него есть рекламный код и он использует устройство под управлением ОС Windows10. Эта надстройка не отображается в описании родительского продукта**: выбор этого параметра означает, что надстройка не будет отображаться вместе с приложением, а новые пользователи не смогут покупать надстройку. Однако **этот параметр не поддерживается пользователями устройств Windows8.1 и более ранних версий**. Если ваше приложение доступно на устройствах Windows8.1 и более ранних версий, надстройка будет доступна этим пользователям для покупки. Чтобы перестать предоставлять эту надстройку пользователям устройств Windows8.1 или более ранних версий, необходимо обновить ваше приложение, чтобы удалить код, который предоставляет надстройку, а затем опубликовать новую отправку приложения. Рекомендуется это сделать, даже если ваше приложение не предназначено для Windows8.1 или более ранних версий; лучше не предоставлять надстройку пользователям, чем сначала предоставить, а потом сделать недоступными.
    
 > [!NOTE] 
 > Выбор параметра **Остановить приобретение** и (или) отправка обновления приложения, которое удаляет надстройку из кода приложения, не влияет на пользователей, которые уже приобрели надстройку, независимо от используемой ими операционной системы.


## <a name="schedule"></a>Расписание

По умолчанию ваша надстройка будет доступна клиентам, как только пройдет сертификацию и процедуру публикации (если только вы не выбрали один из параметров **Скрыто в Магазине** в разделе **Видимость**). Чтобы развернуть этот раздел и указать другие даты, выберите вариант **Показать параметры**. 

Дополнительные сведения см. в разделе [Настройка точного расписания выпуска](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Цены

(Если вы выбрали вариант **Остановить приобретение** в разделе " **видимость** "), необходимо выбрать базовую цену для надстройки. Значением по умолчанию является **бесплатно**, поэтому если вы хотите взимать плату за надстройки, обязательно выберите один из доступных ценовых (начиная с.99 долл. США).

Вы также можете запланировать изменение цены, указав дату и время, когда цена вашей надстройки должна измениться. Кроме того, вы можете настроить эти изменения для определенных рынков. 

> [!TIP]
> Для надстроек с подпиской не может повысить цену после публикации надстройки, выбрав выше базовой цены при последующей отправке или путем планирования изменение цены, которая повышает цену. Вы можете выбрать более низкой цене, с помощью любого из этих методов, но после снижено цена вы не сможете позволяет повысить выше этого новые цены. По этой причине особенно важно гарантировать выберите соответствующий ценовую категорию надстройки с подпиской. 

Дополнительные сведения см. в разделе [Настройка и планирование цены приложения](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Цена продажи

Если вы хотите продавать надстройку по сниженной цене в течение ограниченного срока, вы можете создать и запланировать распродажу. Более подробную информацию см. в статье [Выставление на продажу приложений и надстроек](put-apps-and-add-ons-on-sale.md).


