---
author: jnHs
Description: "Настройка пользовательских разрешений для пользователей учетных записей."
title: "Настройка собственных разрешений для пользователей учетных записей"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: d45ae4001dbb14a11e2beeecc3f98fb72bbc8a86
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2017
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Настройка ролей и пользовательских разрешений для пользователей учетных записей

При [добавлении пользователей в учетную запись Центра разработки](add-users-groups-and-azure-ad-applications.md) вам потребуется указать их тип доступа к учетной записи. Это можно сделать, назначив им [стандартные роли](#roles), которые применяются ко всей учетной записи, или вы можете [настроить их разрешения](#custom), чтобы предоставить соответствующий уровень доступа. Некоторые из пользовательских разрешений применяются ко всей учетной записи, а некоторые могут предоставляться всем продуктам или лишь некоторым из них.

> [!NOTE] 
> Те же роли и разрешения могут применяться независимо от того, добавляется ли пользователь, группа или приложение Azure AD.

При определении того, какую роль или разрешение требуется применить, имейте в виду следующее. 
-   Пользователи (в том числе группы и приложения Azure Active Directory) получат доступ ко всей учетной записи Центра разработки в соответствии с разрешениями, которые связаны с назначенной им ролью, если только вы [не настроите разрешения](#custom) и не назначите [разрешения уровня продукта](#product-level-permissions), чтобы они могли работать только с определенными приложениями и (или) надстройками.
-   Вы можете разрешить пользователю, группе или приложению Azure AD получать доступ к функциональности нескольких ролей, выбрав несколько ролей, или с помощью пользовательских разрешений для предоставления требуемых прав доступа.
-   Пользователь с определенной ролью (или набором пользовательских разрешений) также может быть членом группы с другой ролью (или набором разрешений). В этом случае пользователь будет иметь доступ ко всем возможностям, связанным и с отдельной учетной записью, и с группой.


<span id="roles" />
## <a name="assign-roles-to-account-users"></a>Назначение ролей пользователям учетной записи

По умолчанию вам доступен набор стандартных ролей, которые вы можете назначать при добавлении пользователя, группы или приложения Azure AD в вашу учетную запись Центра разработки. Каждая роль имеет определенный набор разрешений для выполнения определенных функций в учетной записи. 

Если вы не определите [пользовательские разрешения](#custom) с помощью пункта **Настроить разрешения**, всем пользователям, группам или приложениям Azure Active Directory, добавляемым в учетную запись, необходимо назначить по крайней мере одну из указанных ниже стандартных ролей. 

> [!NOTE]
> **Владельцем** учетной записи является пользователь, создавший ее с помощью учетной записи Майкрософт (а не пользователи, добавленные с помощью Azure Active Directory). Только владелец имеет полный доступ к учетной записи: он может удалять приложения, создавать и редактировать любых пользователей, а также изменять финансовую информацию и параметры учетной записи. 


| Роль                 | Описание              |
|----------------------|--------------------------|
| Менеджер              | Имеет полный доступ к учетной записи, за исключением права на изменение параметров налогов и выплат. Сюда относится и управление пользователями в Центре разработки, но обратите внимание, что право на создание и удаление пользователей зависит от разрешений в Azure Active Directory. То есть, если пользователю назначена роль менеджера, но в Azure Active Directory у него нет разрешений администратора, он не сможет создавать или удалять пользователей из каталога (но сможет изменять их роли в Центре разработки). |
| Разработчик            | Может отправлять пакеты, приложения и надстройки, а также просматривать сведения телеметрии в [отчете об использовании](usage-report.md). Не может просматривать финансовую информацию и параметры учетной записи.   |
| Бизнес-участник | Может просматривать отчеты по [Работоспособности](health-report.md) и [Использованию](usage-report.md). Не может создавать и отправлять продукты, изменять параметры учетной записи и просматривать финансовые сведения.                                         |
| Финансист  | Может просматривать [отчеты о выплатах](payout-summary.md), финансовую информацию и отчеты о приобретениях. Не может изменять приложения, надстройки и параметры учетной записи.                                                                                                                                   |
| Маркетолог             | Может [отвечать на отзывы клиентов](respond-to-customer-reviews.md) и просматривать нефинансовые [аналитические отчеты](analytics.md). Не может изменять приложения, надстройки и параметры учетной записи.      |

В следующей таблице показаны некоторые конкретные функции, доступные каждой из этих ролей (и владельцу учетной записи).

|                                 |    Владелец учетной записи                 |    Менеджер                       |    Разработчик                     |    Бизнес-участник    |    Финансист    |    Маркетолог                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Отчет "Приобретения"           |    Может просматривать                      |    Может просматривать                      |     Нет доступа                    |     Нет доступа              |    Может просматривать               |    Нет доступа                     |
|    Отчет по отзывам и ответы    |    Может просматривать и отправлять отзывы    |    Может просматривать и отправлять отзывы    |    Может просматривать и отправлять отзывы    |     Нет доступа              |     Нет доступа             |    Может просматривать и отправлять отзывы    |
|    Отчет о работоспособности                |    Может просматривать                      |    Может просматривать                      |    Может просматривать                      |    Может просматривать                |     Нет доступа             |    Нет доступа                     |
|    Отчет об использовании                 |    Может просматривать                      |    Может просматривать                      |    Может просматривать                      |    Может просматривать                |     Нет доступа             |    Нет доступа                     |
|    Счет для выплат               |    Может обновлять                    |    Нет доступа                     |    Нет доступа                     |    Нет доступа               |    Может просматривать               |    Нет доступа                     |
|    Профиль налогоплательщика                  |    Может обновлять                    |    Нет доступа                     |    Нет доступа                     |    Нет доступа               |    Может просматривать               |    Нет доступа                     |
|    Сводка по выплатам               |    Может просматривать                      |    Нет доступа                     |    Нет доступа                     |    Нет доступа               |    Может просматривать               |    Нет доступа                     |

Если ни одна из стандартных ролей не подходит или вы хотите ограничить доступ по определенным приложениям или надстройкам, можно предоставить пользователям собственные разрешения, щелкнув **Настроить разрешения**, как описано ниже.


<span id="custom" />
## <a name="assign-custom-permissions-to-account-users"></a>Назначение пользовательских разрешений пользователям учетных записей

Чтобы назначить пользовательские разрешения, а не стандартные роли, щелкните **Настройка разрешений** в разделе **Роли** при добавлении или изменении учетной записи пользователя. 

Чтобы включить разрешение для пользователя, задайте для соответствующего поля нужный параметр с помощью переключателя. 

![Руководство по настройкам доступа](images/permission_key.png)

- **Нет доступа**: у пользователя не будет указанных разрешений.
- **Только чтение**: пользователь сможет просматривать функции, связанные с указанной областью, но не сможет вносить изменения. 
- **Чтение и запись**: пользователь сможет вносить изменения в соответствующую область и просматривать ее.
- **Смешанный**: невозможно выбрать этот параметр напрямую, однако индикатор **Смешанный** покажет, разрешено ли сочетание разных типов доступа для этого разрешения. Например, если предоставить доступ **Только чтение** к разделу **Цена и доступность** для пункта **Все продукты**, а затем предоставить доступ **Чтение и запись** к разделу **Цена и доступность** для конкретного продукта, индикатор **Цена и доступность** для раздела **Все продукты** будет иметь значение "Смешанные". То же самое применимо, если у некоторых продуктов имеется разрешение **Нет доступа**, а у других— **Чтение и запись** или **Только чтение**.

Для некоторых разрешений, например связанных с просмотром данных аналитики, можно предоставить доступ **Только чтение**. Обратите внимание, что в текущей реализации определенные разрешения не имеют различия между параметрами **Только чтение** и **Чтение и запись**. Просмотрите сведения для каждого разрешения, чтобы понять, какие возможности предоставляют параметры доступа **Только чтение** и (или) **Чтение и запись**.

Подробное описание каждого разрешения приводится в таблицах ниже.

## <a name="account-level-permissions"></a>Разрешения уровня доступа

Разрешения в этом разделе невозможно ограничить конкретными продуктами. Предоставление доступа к этим разрешениям предоставляет пользователю разрешение для всей учетной записи.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Имя разрешения</th>
    <th align="left">Только чтение</th>
    <th align="left">Чтение и запись</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    **Параметры учетной записи**                    </td><td align="left">  Можно просматривать все страницы в разделе **Параметры учетной записи**, в том числе [Контактные данные](managing-your-profile.md).       </td><td align="left">  Можно просматривать все страницы в разделе **Параметры учетной записи**. Можно вносить изменения на страницу [Контактные данные](managing-your-profile.md) и другие страницы, но невозможно менять счет выплат или налоговый профиль (за исключением случаев, когда такое разрешение предоставляется отдельно).            </td></tr>
<tr><td align="left">    **Пользователи учетных записей**                       </td><td align="left">  Можно просматривать пользователей, которые были добавлены к учетной записи в разделе **Управление пользователями**.          </td><td align="left">  Можно добавлять пользователей к учетной записи и вносить изменения в существующих пользователей в разделе **Управление пользователями**.             </td></tr>
<tr><td align="left">    **Отчет об эффективности рекламы на уровне учетной записи** </td><td align="left">  Можно просматривать [отчет об эффективности рекламы](advertising-performance-report.md) на уровне учетной записи.      </td><td align="left">  Недоступно   </td></tr>
<tr><td align="left">    **Рекламные кампании**                        </td><td align="left">  Можно просматривать [рекламные кампании](create-an-ad-campaign-for-your-app.md), созданные в учетной записи.      </td><td align="left">  Можно создавать, просматривать [рекламные кампании](create-an-ad-campaign-for-your-app.md), созданные в этой учетной записи, и управлять ими.          </td></tr>
<tr><td align="left">    **Рекламный посредник**                        </td><td align="left">  Можно просматривать [конфигурации рекламного посредника](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) для всех продуктов в учетной записи.    </td><td align="left">  Можно просматривать и менять [конфигурации рекламного посредника](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) для всех продуктов в учетной записи.        </td></tr>
<tr><td align="left">    **Отчеты о рекламном посреднике**                </td><td align="left">  Можно просматривать [отчет о рекламном посреднике](ad-mediation-report.md) для всех продуктов в учетной записи.    </td><td align="left">  Н/Д    </td></tr>
<tr><td align="left">    **Отчеты об эффективности рекламы**              </td><td align="left">  Можно просматривать [отчеты об эффективности рекламы](advertising-performance-report.md) для всех продуктов в учетной записи.       </td><td align="left">  Недоступно         </td></tr>
<tr><td align="left">    **Группы объявлений**                            </td><td align="left">  Можно просматривать [рекламные блоки](monetize-with-ads.md), созданные для учетной записи.    </td><td align="left">  Можно создавать, просматривать [рекламные блоки](monetize-with-ads.md) для этой учетной записи и управлять ими.             </td></tr>
<tr><td align="left">    **Реклама от аффилированных лиц Майкрософт**                       </td><td align="left">  Можно просматривать использование [рекламы от аффилированных лиц Майкрософт](about-affiliate-ads.md) во всех продуктах учетной записи.    </td><td align="left">  Можно просматривать использование [рекламы от аффилированных лиц Майкрософт](about-affiliate-ads.md) для всех продуктов учетной записи.                </td></tr>
<tr><td align="left">    **Отчеты об эффективности рекламы от аффилированных лиц**      </td><td align="left">  Можно просматривать [Отчет об эффективности рекламы от аффилированных лиц](affiliates-performance-report.md) для всех продуктов учетной записи.   </td><td align="left">  Н/Д   </td></tr>
<tr><td align="left">    **Отчеты о рекламных объявлениях, которые привели к установке приложений**             </td><td align="left">  Можно просмотреть [Отчет о рекламной кампании](promote-your-app-report.md).           </td><td align="left">  Н/Д   </td></tr>
<tr><td align="left">    **Реклама от сообщества**                       </td><td align="left">  Можно просматривать бесплатное использование [рекламы от сообщества](about-community-ads.md) для всех продуктов в учетной записи.          </td><td align="left">  Можно создавать, просматривать бесплатное использование [рекламы от сообщества](about-community-ads.md) для всех продуктов в учетной записи, а также управлять этим использованием.               </td></tr>
<tr><td align="left">    **Контактные данные**                        </td><td align="left">  Можно просматривать [контактные данные](managing-your-profile.md) в разделе "Параметры учетной записи".        </td><td align="left">  Можно изменять и просматривать [контактные данные](managing-your-profile.md) в разделе "Параметры учетной записи".            </td></tr>
<tr><td align="left">    **Соответствие требованиям COPPA**                    </td><td align="left">  Можно просматривать выбранные параметры [соответствия требованиям COPPA](monetize-with-ads.md#coppa-compliance) (обозначающие, предназначены ли продукты для детей младше 13 лет) для всех продуктов в учетной записи.                                            </td><td align="left">  Можно изменять и просматривать выбранные параметры [соответствия требованиям COPPA](monetize-with-ads.md#coppa-compliance) (обозначающие, предназначены ли продукты для детей младше 13 лет) для всех продуктов в учетной записи.         </td></tr>
<tr><td align="left">    **Группы пользователей**                     </td><td align="left">  Можно просматривать [группы пользователей](create-customer-groups.md) (сегменты и тестовые группы) в разделе **Клиенты**.      </td><td align="left">  Можно создавать, изменять и просматривать [группы клиентов](create-customer-groups.md) (сегменты и тестовые группы) в разделе **Клиенты**.       </td></tr>
<tr><td align="left">    **Новые приложения**                            </td><td align="left">  Можно просмотреть страницу создания новых приложений, но невозможно создать никакие новые приложения в этой учетной записи.    </td><td align="left">  Можно [создавать новые приложения](create-your-app-by-reserving-a-name.md) в учетной записи, резервируя имена новых приложений, а также создавать отправки и отправлять приложения в Магазин.     </td></tr>
<tr><td align="left">    **Новые наборы**&nbsp;*                       </td><td align="left">  Можно просмотреть страницу создания новых наборов, но невозможно создать никакие новые наборы в этой учетной записи.     </td><td align="left">  Можно создать новые наборы продуктов.          </td></tr>
<tr><td align="left">    **Услуги партнеров**&nbsp;*                  </td><td align="left">  Можно просмотреть сертификаты для установки в службах с целью извлечения токенов XTokens.     </td><td align="left">  Можно просматривать сертификаты для установки в службах с целью извлечения токенов XTokens и управления ими.       </td></tr>
<tr><td align="left">    **Счет для выплат**                      </td><td align="left">  Можно просматривать [сведения о счете для выплат](setting-up-your-payout-account-and-tax-forms.md#payout-account) в разделе **Параметры учетной записи**.     </td><td align="left">  Можно редактировать и просматривать [сведения о счете для выплат](setting-up-your-payout-account-and-tax-forms.md#payout-account) в разделе **Параметры учетной записи**.       </td></tr>
<tr><td align="left">    **Сводка по выплатам**                      </td><td align="left">  Можно просматривать раздел [Сводка о выплатах](payout-summary.md), чтобы получить доступ к отчетным сведениям о выплатах и скачать их.       </td><td align="left">  Можно просматривать раздел [Сводка о выплатах](payout-summary.md), чтобы получить доступ к отчетным сведениям о выплатах и скачать их.   </td></tr>
<tr><td align="left">    **Проверяющие стороны**&nbsp;*                   </td><td align="left">  Можно просматривать проверяющие стороны для извлечения токенов XTokens.    </td><td align="left">  Можно просматривать проверяющие стороны для извлечения токенов XTokens и управлять этими сторонами.     </td></tr>
<tr><td align="left">    **Запрос дисков**&nbsp;*                   </td><td align="left">  Можно просматривать запросы на диски с играми.    </td><td align="left">  Можно создавать и просматривать запросы на диски с играми.     </td></tr>
<tr><td align="left">    **Песочницы**&nbsp;*                         </td><td align="left">  Можно получить доступ к странице **Песочницы** и просматривать песочницы в учетной записи и любые применимые конфигурации для этих песочниц. Невозможно просматривать продукты и отправки для каждой песочницы, если не предоставлены соответствующие разрешения уровня продукта. </td><td align="left">  Можно получить доступ к странице **Песочницы**, просматривать песочницы в учетной записи и управлять ими, включая создание и удаление песочниц и управление их конфигурациями. Невозможно просматривать продукты и отправки для каждой песочницы, если не предоставлены соответствующие разрешения уровня продукта.    </td></tr>
<tr><td align="left">    **Профиль налогоплательщика**                         </td><td align="left">  Можно просматривать [сведения о профиле налогоплательщика и формы](setting-up-your-payout-account-and-tax-forms.md#tax-forms) в разделе **Параметры учетной записи**.     </td><td align="left">  Можно заполнять налоговые формы и обновлять [сведения о налоговом профиле](setting-up-your-payout-account-and-tax-forms.md#tax-forms) в разделе **Параметры учетной записи**.     </td></tr>
<tr><td align="left">    **Тестовые учетные записи**&nbsp;*                     </td><td align="left">  Можно просматривать учетные записи для проверки конфигурации Xbox Live.      </td><td align="left">  Можно создавать, просматривать учетные записи для тестирования конфигурации Xbox Live и управлять ими.      </td></tr>
<tr><td align="left">    **Устройства Xbox**                        </td><td align="left">  Можно просматривать консоли разработки Xbox, включенные для этой учетной записи, в разделе **Параметры учетной записи**.       </td><td align="left">  Можно добавлять, удалять и просматривать консолей разработки Xbox, включенные для учетной записи в разделе **Параметры учетной записи**.     </td></tr>
    </tbody>
    </table>

\ * Разрешения, помеченные звездочкой (*), предоставляют доступ к функциям, которые доступны не для всех учетных записей. Если эти функции не включены для вашей учетной записи, выбор вами тех или иных разрешений не будет иметь никакого эффекта.   


## <a name="product-level-permissions"></a>Разрешения уровня продукта

Разрешения в этом разделе могут предоставляться для всех продуктов в учетной записи или настраиваться так, чтобы предоставить разрешения только для одного или нескольких конкретных продуктов. 

Разрешения уровня приложения сгруппированы в четыре категории: **Аналитика**, **Монетизация**, **Публикации** и **Xbox Live**. Можно развернуть каждую из этих категорий, чтобы просмотреть отдельные разрешения в каждой категории. Вы также можете включить параметр **Все разрешения** для одного или нескольких конкретных продуктов.

Чтобы предоставить разрешение для всех продуктов в этой учетной записи, выберите соответствующие параметры для этого разрешения (установите переключатель в положение **Только чтение** или **Чтение и запись**) в строке, помеченной как **Все продукты**. 
 
> [!TIP]
> Выбранные в поле **Все продукты** параметры будут применяться для каждого продукта, который в настоящее время используется в учетной записи, а также для всех будущих продуктов, созданных в этой учетной записи. Чтобы разрешения не применялись к будущим продуктам, выберите все продукты по отдельности, а не пункт **Все продукты**.

Под строкой **Все продукты** каждый продукт учетной записи отображается в отдельной строке. Чтобы предоставить разрешение только для конкретного продукта, выберите соответствующие параметры для этого разрешения в строке для этого продукта.

Каждая надстройка указана в отдельной строке под родительским продуктом вместе со строкой **Все надстройки**. Выбранные параметры для поля **Все надстройки**будут применяться ко всем действующим надстройкам для этого продукта, а также ко всем будущим надстройкам, созданным для этого продукта.

Обратите внимание, что некоторые разрешения невозможно настроить для надстроек. Это может быть вызвано тем, что они не применяются к надстройкам (например, разрешение **Отзывы пользователей**), или разрешение, предоставленное на уровне родительского продукта, применяется ко всем надстройкам этого продукта (например, **Рекламные коды**). Обратите внимание, что все разрешения, доступные для надстроек, должны устанавливаться отдельно; надстройки не наследуют, выбранные для родительского продукта параметры. Например, если требуется разрешить пользователю выбирать параметры цен и доступности для надстройки, необходимо включить разрешение **Цена и доступность** для надстройки (или в поле **Все надстройки**) независимо от того, было ли предоставлено разрешение **Цена и доступность** для родительского продукта. 


### <a name="analytics"></a>Аналитика

<table>
    <thead>
    <tr class="header">
    <th align="left">Имя&nbsp;разрешения</th>
    <th align="left">Только&nbsp;чтение</th>
    <th align="left">Чтение и запись</th>
    <th align="left">Только&nbsp;чтение&nbsp;(Надстройка) </th>
    <th align="left">Чтение и запись&nbsp;(надстройка)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Приобретения**     </td><td>    Можно просматривать отчеты [Приобретения](acquisitions-report.md) и [Приобретения надстроек](add-on-acquisitions-report.md) для продукта.        </td><td>    Н/Д    </td><td>    Н/Д (параметры родительского продукта включают отчеты о приобретении надстроек)        </td><td>    Н/Д                         </td></tr>
    <tr><td align="left">    **Использование** </td><td>    Можно просматривать [Отчет об использовании](usage-report.md) для продукта.     </td><td>    Н/Д       </td><td>    Н/Д     </td><td>    Н/Д         </td></tr>
    <tr><td align="left">    **Health** </td><td>    Можно просматривать [Отчет о работоспособности](health-report.md) для продукта.    </td><td>    Н/Д     </td><td>    Н/Д     </td><td>    Н/Д         </td></tr>
    <tr><td align="left">    **Отзывы пользователей**    </td><td>    Можно просматривать отчеты [Рецензии](reviews-report.md) и [Отзывы](feedback-report.md) для продукта.       </td><td>    Н/Д (для ответа на отзывы или рецензии должно быть предоставлено разрешение **Связаться с клиентом**)   </td><td>    Н/Д     </td><td>    Н/Д         </td></tr>
    <tr><td align="left">    **Аналитика Xbox** </td><td>    Можно просматривать аналитический отчет Xbox для конкретного продукта. (Примечание: этот отчет еще недоступен.)    </td><td>    Н/Д   </td><td>    Н/Д       </td><td>    Н/Д          </td></tr>
    <tr><td align="left">    **В режиме реального времени**   </td><td>    Можно просматривать отчет по продукту в режиме реального времени. (Примечание. В настоящий момент этот отчет доступен только через [Программу предварительной оценки для Центра разработки](dev-center-insider-program.md).)      </td><td>    Н/Д   </td><td>    Н/Д     </td><td>    Н/Д                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Получение дохода

<table>
    <thead>
    <tr class="header">
    <th align="left">Имя&nbsp;разрешения</th>
    <th align="left">Только&nbsp;чтение</th>
    <th align="left">Чтение и запись</th>
    <th align="left">Только&nbsp;чтение&nbsp;(Надстройка) </th>
    <th align="left">Чтение и запись&nbsp;(надстройка)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Промокоды**     </td><td>    Можно просматривать сведения о заказах и использовании [рекламных кодов](generate-promotional-codes.md) для продукта и его надстроек, а также просматривать сведения об использовании.         </td><td>    Можно просматривать, создавать сведения о заказах [рекламных кодов](generate-promotional-codes.md) для продукта и его надстроек, а также просматривать сведения об использовании и управлять этими данными.          </td><td>    Н/Д (параметры для родительского продукта влияют на все надстройки)     </td><td>    Н/Д (параметры для родительского продукта влияют на все надстройки)     </td></tr>
    <tr><td align="left">    **Целевые предложения**     </td><td>    Можно просматривать [целевые предложения](use-targeted-offers-to-maximize-engagement-and-conversions.md) для продукта.         </td><td>    Можно просматривать, администрировать и создавать [целевые предложения](use-targeted-offers-to-maximize-engagement-and-conversions.md) для продукта.          </td><td>    Недоступно     </td><td>    Недоступно      </td></tr>
    <tr><td align="left">    **Связаться с клиентом**  </td><td>    Можно просматривать [ответы на отзывы клиентов](respond-to-customer-feedback.md) и [ответы на рецензии клиентов](respond-to-customer-reviews.md), если также предоставлено разрешение **Отзывы клиентов**. Можно также просматривать [целевые уведомления](send-push-notifications-to-your-apps-customers.md), созданные для этого продукта.    </td><td>    Можно [отвечать на отзывы клиентов](respond-to-customer-feedback.md) и [отвечать на рецензии клиентов](respond-to-customer-reviews.md), если также предоставлено разрешение **Отзывы клиентов**. Можно также [создавать и отправлять целевые уведомления](send-push-notifications-to-your-apps-customers.md) для продукта.                   </td><td>    Н/Д         </td><td>    Н/Д                          </td></tr>
    <tr><td align="left">    **Эксперименты**</td><td>    Можно просматривать [эксперименты (A/B-тестирование)](../monetize/run-app-experiments-with-a-b-testing.md) и просматривать данные экспериментов для этого продукта.   </td><td>    Можно создавать, просматривать [эксперименты (A/B-тестирование)](../monetize/run-app-experiments-with-a-b-testing.md) и управлять ими для конкретного продукта, а также просматривать данные экспериментов.     </td><td>    Н/Д  </td><td>    Недоступно                 </td></tr>

    </tbody>
    </table>

### <a name="publishing"></a>Выполняется публикация 

<table>
    <thead>
    <tr class="header">
    <th align="left">Имя&nbsp;разрешения</th>
    <th align="left">Только&nbsp;чтение</th>
    <th align="left">Чтение и запись</th>
    <th align="left">Только&nbsp;чтение&nbsp;(Надстройка) </th>
    <th align="left">Чтение и запись&nbsp;(надстройка)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Цена и доступность**  </td><td>    Можно просматривать страницу [Цена и доступность](set-app-pricing-and-availability.md) отправок продуктов.     </td><td>    Можно просматривать и редактировать страницу [Цена и доступность](set-app-pricing-and-availability.md) отправок продуктов. </td><td>    Можно просматривать страницу [Цена и доступность](set-add-on-pricing-and-availability.md) отправок надстроек.   </td><td>    Можно просматривать и редактировать страницу [Цена и доступность](set-add-on-pricing-and-availability.md) отправок надстроек.          </td></tr>
    <tr><td align="left">    **Свойства**   </td><td>    Можно просматривать страницу [Свойства](enter-app-properties.md) отправок продуктов.      </td><td>    Можно просматривать и редактировать страницу [Свойства](enter-app-properties.md) отправок продуктов.       </td><td>    Можно просматривать страницу [Свойства](enter-add-on-properties.md) отправок надстроек.     </td><td>    Можно просматривать и редактировать страницу [Свойства](enter-add-on-properties.md) отправок надстроек.               </td></tr>
    <tr><td align="left">    **Возрастные категории**    </td><td>    Можно просматривать страницу [Возрастные категории](age-ratings.md) отправок продуктов.       </td><td>    Можно просматривать и редактировать страницу [Возрастные категории](age-ratings.md) отправок продуктов.    </td><td>    * Можно просматривать страницу "Возрастные категории" отправок надстроек.          </td><td>    * Можно просматривать и редактировать страницу "Возрастные категории" отправок надстроек.       </td></tr>
    <tr><td align="left">    **Пакеты**        </td><td>    Можно просматривать страницу [Пакеты](upload-app-packages.md) для отправок продуктов.  </td><td>    Можно просматривать и редактировать страницу [Пакеты](upload-app-packages.md) для отправок продукта, в том числе отправки пакетов.     </td><td>    * Можно просматривать ориентацию на семейство устройств и пакеты (если применимо) для отправок надстройки.   </td><td>    * Можно просматривать и редактировать ориентацию на семейство устройств для отправок надстроек, включая отправку пакетов, если применимо.             </td></tr>
    <tr><td align="left">    **Описания в Магазине**  </td><td>    Можно просматривать страницы [описания в Магазине](create-app-store-listings.md) для отправок продукта.  </td><td>    Можно просматривать и редактировать страницы [описания в Магазине](create-app-store-listings.md) для отправок продукта и добавлять новые описания в Магазине для разных языков.     </td><td>    Можно просматривать страницы [описания в Магазине](create-add-on-store-listings.md) для отправок надстроек.            </td><td>    Можно просматривать и редактировать страницы [описания в Магазине](create-add-on-store-listings.md) для отправок надстроек и добавлять новые описания в Магазине для разных языков.                 </td></tr>
    <tr><td align="left">    **Отправка в Магазин**     </td><td>    Доступ не предоставляется, если настроено разрешение "Только чтение".           </td><td>    Можно отправить продукт в Магазин и просмотреть отчеты о сертификации. Включает новые и обновленные отправки. </td><td>Доступ не предоставляется, если настроено разрешение "Только чтение".     </td><td>    Можно отправить надстройку в Магазин и просмотреть отчеты о сертификации. Включает новые и обновленные отправки.</td></tr>
    <tr><td align="left">    **Создание новой отправки**       </td><td>    Доступ не предоставляется, если настроено разрешение "Только чтение".        </td><td>    Можно создать новые [отправки](app-submissions.md) для продукта.  </td><td>    Доступ не предоставляется, если настроено разрешение "Только чтение".   </td><td>    Можно создать новые [отправки](add-on-submissions.md) для надстройки.        </td></tr>
    <tr><td align="left">    **Новые надстройки**    </td><td>    Доступ не предоставляется, если настроено разрешение "Только чтение". </td><td>    Можно [создавать новые надстройки](set-your-add-on-product-id.md) для продукта. </td><td>    Н/Д    </td><td>    Н/Д        </td></tr>
    <tr><td align="left">    **Резервирование имен**   </td><td>    Можно просматривать страницу [Управление именами приложений](manage-app-names.md) для продукта.</td><td>    Можно просматривать и редактировать страницу [Управление именами приложений](manage-app-names.md) для продукта, в том числе резервировать дополнительные имена и удалять зарезервированные имена. </td><td>   * Можно просматривать зарезервированные имена для надстройки.    </td><td>   * Можно просматривать и редактировать зарезервированные имена для надстройки.          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Имя&nbsp;разрешения</th>
    <th align="left">Только&nbsp;чтение</th>
    <th align="left">Чтение и запись</th>
    <th align="left">Только&nbsp;чтение&nbsp;(Надстройка) </th>
    <th align="left">Чтение и запись&nbsp;(надстройка)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Каналы приложений**&nbsp;\*</td><td>    Недоступно  </td><td>    Можно публиковать рекламные видеоканалы на консоли Xbox для просмотра через OneGuide.  </td><td>  Н/Д </td><td> Недоступно </td></tr>
    <tr><td align="left">    **Конфигурация службы**&nbsp;\*    </td><td>    Можно просматривать параметры, связанные с достижениями, многопользовательским режимом, списками лидеров и другими настройками Xbox Live для продукта.  </td><td>    Можно просматривать и изменять параметры, связанные с достижениями, многопользовательским режимом, списками лидеров и другими настройками Xbox Live для продукта.  </td><td>    Н/Д     </td><td>    Н/Д                      </td></tr>
</tbody>
</table>

\ * Разрешения, помеченные звездочкой (*), предоставляют доступ к функциям, которые доступны не для всех учетных записей. Если эти функции не включены для вашей учетной записи, выбор вами тех или иных разрешений не будет иметь никакого эффекта.  