---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Ознакомьтесь с рекомендациями по пользовательскому интерфейсу и взаимодействию с пользователем для рекламы в приложениях.
title: Рекомендации по пользовательскому интерфейсу и взаимодействию с пользователем для рекламы
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, рекламные объявления, реклама, руководство, советы и рекомендации
ms.localizationpriority: medium
ms.openlocfilehash: d64d5c544f6ec9e1356cc024e634286336dc9f91
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506798"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Рекомендации по пользовательскому интерфейсу и взаимодействию с пользователем для рекламы

>[!WARNING]
> Начиная с 1 июня 2020 г. платформа Microsoft AD монетизацию для приложений Windows UWP будет выключена. [Подробнее](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

В этой статье представлены рекомендации по повышению удобства работы с рекламными баннерами, межстраничными и собственными объявлениями в ваших приложениях. Общие рекомендации по проектированию интерфейса приложений см. в разделе [Проектирование и пользовательский интерфейс](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Любое использование рекламы в вашем приложении должно соответствовать политикам Microsoft Store, включая, помимо прочего, [политику 10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (Поведение рекламы и содержимое). В частности, реализация рекламных баннеров и промежуточной рекламы в вашем приложении должна соответствовать [политике 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) Microsoft Store. В этой статье приводятся примеры реализаций, нарушающих эту политику. Эти примеры приводятся исключительно в информационных целях, чтобы помочь вам лучше понять политику. Эти примеры не являются исчерпывающими, существует множество других нарушений политик Microsoft Store, не перечисленных в этой статье.

## <a name="general-best-practices"></a>Общие рекомендации

Перед ознакомлением с нашими рекомендациями для различных типов рекламы в этой статье сначала ознакомьтесь с общими рекомендациями для повышения прибыли от вашей рекламы.

* [Тщательно планируйте размещение своей рекламы](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). См. наше руководство по [оптимизации просматриваемости ваших рекламных блоков](optimize-ad-unit-viewability.md).
* [Используйте межстраничные баннеры для дублирования промежуточной видеорекламы](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video/).
* [Изучайте свою аудиторию, чтобы повысить эффективность целевой рекламы](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Используйте последние библиотеки Microsoft Advertising](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Задавайте правильные параметры COPPA для вашего приложения](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app/).


## <a name="guidelines-for-banner-ads"></a>Рекомендации для рекламных баннеров

В следующих разделах приводятся рекомендации по реализации [баннеров](banner-ads.md) в вашем приложении с использованием элемента [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol), а также примеры реализаций, нарушающих [политику 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) Microsoft Store.

### <a name="best-practices"></a>Рекомендации

При реализации рекламных баннеров в своем приложении выполняйте следующие рекомендации:

* [Соблюдайте стандарты Некоммерческого партнерства содействия развитию интерактивной рекламы по размерам](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes/), которые соответствуют размеру и положению дисплея устройства.

* Большую часть пользовательского интерфейса вашего приложения должны составлять функциональные элементы управления и содержимое.

* Интегрируйте рекламу в пользовательский интерфейс. Предоставьте своим дизайнерам пример рекламы, чтобы они могли спланировать, как она будет выглядеть. Примерами хорошо спланированной рекламы могут служить макеты «реклама как содержимое» и макеты с разделением.

  Чтобы увидеть, как реклама различных размеров выглядит и работает в вашем приложении на этапе разработки и тестирования, воспользуйтесь нашими [тестовыми рекламными блоками](set-up-ad-units-in-your-app.md#test-ad-units). После завершения тестирования не забудьте [обновить приложение, добавив реальные рекламные блоки](set-up-ad-units-in-your-app.md#live-ad-units) перед отправкой приложения на сертификацию.

* Следует предусмотреть случаи, когда рекламные объявления недоступны. Могут быть случаи, когда объявления не отправляются в приложение. Создавайте макеты страниц таким образом, чтобы они хорошо выглядели как с рекламой, так и без нее. Дополнительные сведения см. в разделе [Обработка ошибок рекламы](error-handling-with-advertising-libraries.md).

* Если удобнее всего оповестить пользователя о чем-либо с помощью наложения, вызовите метод [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend), пока отображается наложение, а затем вызовите метод [AdControl.Resume](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume), когда сценарий оповещения будет выполнен.

### <a name="practices-to-avoid"></a>Нерекомендуемые методики

При реализации рекламных баннеров в своем приложении не следует:

* Располагать рекламу на открытом пространстве. Размещать место для объявления в первом попавшемся свободном пространстве. Вместо этого рекламу следует встроить в общую компоновку приложения.

* Перенасыщать приложение рекламой. Слишком большое количество рекламы ухудшает внешний вид и удобство использования приложения. Желание получить больше прибыли от рекламы не должно влиять на само приложение.

* Отвлекать пользователя от ключевых задач. Основное внимание всегда должно уделяться приложению. Реклама должна быть встроена так, чтобы оставаться на втором плане.

### <a name="examples-of-policy-violations"></a>Примеры нарушения политики

В этом разделе приводятся примеры использования рекламных баннеров с нарушением [политики 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) Microsoft Store. Эти примеры приводятся исключительно в справочных целях, чтобы помочь вам лучше понять политику. Эти примеры не являются исчерпывающими, существует множество других нарушений политики 10.10.1, не перечисленных здесь.

* Запрещено совершать какие-либо действия, препятствующие просмотру рекламного баннера пользователем, например менять прозрачность [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) или размещать другой элемент управления поверх **AdControl** (кроме случаев, когда сначала вызывается метод [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Запрещено требовать, чтобы пользователи щелкнули рекламный баннер для выполнения какой-либо задачи в приложении, или принуждать пользователей щелкнуть рекламные баннеры потому, что таков дизайн вашего приложения.

* Запрещено каким-либо образом обходить встроенный таймер минимального периода обновления рекламных баннеров, включая (среди прочего) замену объектов [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) или принудительное обновление страницы без вмешательства пользователя.

* Использование активных единиц AD (т. е. единиц AD, получаемых из центра партнеров) во время разработки и тестирования, а также в эмуляторе.

* Запрещено записывать или распространять код, вызывающий рекламные службы через средства, отличные от библиотек Microsoft Advertising, которые выполняются в контексте вашего приложения.

* Запрещено взаимодействовать с недокументированными интерфейсами или дочерними объектами, созданными библиотеками Microsoft Advertising, такими как **WebView** или **MediaElement**.

* Размещение рекламы в Viewbox, чтобы уменьшить размер рекламы, чтобы обеспечить более подробную рекламу на странице, чем обычная.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Рекомендации для промежуточной рекламы

При аккуратном использовании [межстраничные объявления](interstitial-ads.md) могут значительно увеличить доход приложения без отрицательного влияния на удовлетворенность пользователей. Неправильное использование таких рекламных объявлений может привести к совершенно противоположному эффекту.

В следующих разделах приводятся рекомендации по реализации межстраничной видеорекламы и межстраничных баннеров в вашем приложении с использованием элемента [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad), а также примеры реализаций, нарушающих [политику 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) Microsoft Store. Поскольку вы знаете собственное приложение лучше других, окончательное решение остается за вами за исключением случаев, описанных в политике. Очень важно иметь в виду, что оценки вашего приложения и доход от него тесно связаны.

### <a name="best-practices"></a>Рекомендации

При реализации промежуточной рекламы в своем приложении выполняйте следующие рекомендации:

* Встраивайте промежуточную рекламу в естественный поток приложения, например между игровыми уровнями.

* Связывайте рекламу с ощутимыми преимуществами, среди которых:

    * Подсказки по прохождению уровней.

    * Дополнительное время для повторного прохождения уровня.

    * Функции настройки вида персонажа, такие как татуировка или шляпа.

* Если в приложении требуется, чтобы межстраничная видеореклама была просмотрена до конца, упомяните об этом правиле заранее, чтобы пользователи, нажавшие на кнопку закрытия, не удивлялись сообщению об ошибке.

* Получайте рекламное объявление предварительно (путем вызова [InterstitialAd.RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)), лучше всего за 30–60 секунд до его отображения.

* Подпишитесь на все четыре события, представленные в классе [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) (**Canceled**, **Completed**, **AdReady**, and **ErrorOccurred**), и используйте их для принятия соответствующих решений в приложении.

* Подготовить встроенные возможности, которые можно использовать вместо полученного с сервера объявления. Это будет полезно в ряде случаев:

    * В автономном режиме, когда доступ к рекламным серверам невозможен.

    * При возникновении события **ErrorOccurred**.

    * Если вы предпочтете сэкономить трафик пользователя на основе данных о подключении [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile), в этом вам помогут API-интерфейсы класса **ConnectionProfile**.

* Использовать время ожидания по умолчанию (30 секунд) при отсутствии объективных причин поступить иначе. Если причины есть, не устанавливайте время ожидания менее 10 секунд. Загрузка межстраничных объявлений занимает гораздо больше времени, чем загрузка стандартных рекламных баннеров, особенно в странах, где скорость подключения невысока.

* Учитывать особенности тарифного плана пользователя. Например, не отображайте межстраничную видеорекламу или предупреждайте пользователя перед показом рекламы на мобильном устройстве, лимит трафика которого превышен или скоро будет достигнут. В этом вам помогут API-интерфейсы класса [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile).

* Продолжать улучшать приложение после начальной отправки. Ознакомьтесь с [отчетами о рекламе](../publish/advertising-performance-report.md) и внесите изменения в дизайн, чтобы увеличить отношение показов к попыткам и процент полного просмотра межстраничной видеорекламы.

### <a name="practices-to-avoid"></a>Нерекомендуемые методики

При реализации промежуточной рекламы в своем приложении не следует:

* Переусердствовать. Не показывайте рекламу принудительно чаще одного раза в 5 минут, если пользователь не запросил получение ощутимой выгоды, а просто играет в игру.

* Не показывайте межстраничные объявления при запуске приложения, так как пользователь может решить, что щелкнул не ту плитку.

* Не показывайте межстраничные объявления при выходе. Это плохое решение, поскольку соотношение полного просмотра будет близким к нулю.

* Не показывайте две или более промежуточные рекламы подряд. Пользователи будут расстроены, увидев, что индикатор выполнения показа объявления вернулся на начальную позицию. Многие из них сочтут, что это программная ошибка или ошибка доставки рекламы.

* Не запрашивайте межстраничную видеорекламу более, чем за 5 минут до вызова метода [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show). Правильное размещение увеличит конверсию предварительно полученных рекламных объявлений в оплачиваемые показы.

* Не наказывайте пользователя за сбои при доставке рекламы, такие как недоступность объявлений. Например, если вы добавляете в пользовательский интерфейс элемент "Посмотрите рекламу, чтобы получить *xxx*", вы должны предоставить *xxx*, если пользователь выполнил свою часть условия. Существует два варианта решения задачи:

    * Не отображать возможность до тех пор, пока не будет инициировано событие [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready).

    * Встройте в приложение возможности, дающие то же преимущество, что и просмотр реального объявления.

* Не используйте промежуточную рекламу, чтобы позволить пользователю получить конкурентное преимущество в многопользовательской игре. Например, не предоставляйте пользователю более мощное оружие в стрелялке за просмотр промежуточной рекламы. Можно дать пользователю возможность выбрать рубашку для игрового персонажа, если, конечно, она не имеет защитной раскраски!

### <a name="examples-of-policy-violations"></a>Примеры нарушения политики

В этом разделе приводятся примеры использования промежуточной рекламы с нарушением [политики 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) Microsoft Store. Эти примеры приводятся исключительно в справочных целях, чтобы помочь вам лучше понять политику. Эти примеры не являются исчерпывающими, существует множество других нарушений политики 10.10.1, не перечисленных здесь.

* Размещение элемента пользовательского интерфейса над контейнером промежуточной рекламы.

* Вызов метода [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show), пока пользователь взаимодействует с приложением.

* Использование промежуточной рекламы для получения объектов, которые можно использовать в качестве валюты или для обмена с другими пользователями.

* Запрос новой промежуточной рекламы в контексте обработчика события [InterstitialAd.ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Это может привести к образованию бесконечного цикла и возникновению проблем в работе службы рекламы.

* Запрос промежуточной рекламы только для того, чтобы получить резервную рекламу для бесконечной последовательности рекламных роликов. Если вы запросите промежуточную рекламу, а затем получите событие [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready), следующей промежуточной рекламой, которая отображается в вашем приложении, должна стать реклама, подготовленная к показу с помощью метода [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

* Использование активных единиц AD (т. е. единиц AD, получаемых из центра партнеров) во время разработки и тестирования, а также в эмуляторе.

* Запрещено записывать или распространять код, вызывающий рекламные службы через средства, отличные от библиотек Microsoft Advertising, которые выполняются в контексте вашего приложения.

* Запрещено взаимодействовать с недокументированными интерфейсами или дочерними объектами, созданными библиотеками Microsoft Advertising, такими как **WebView** или **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Рекомендации для собственных объявлений

[Собственные объявления](native-ads.md) предоставляют вам полный контроль над тем, как показывать рекламное содержимое своим пользователям. Выполните эти требования и рекомендации, чтобы гарантировать, что сообщение рекламодателя получит внимание пользователей, а также предотвратить путаницу при показе собственных рекламных объявлений вашим пользователям.

### <a name="register-the-container-for-your-native-ad"></a>Зарегистрируйте контейнер для своего собственного объявления

В коде необходимо вызвать метод [RegisterAdContainer](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) объекта [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) для регистрации элемента пользовательского интерфейса, который действует как контейнер для собственных рекламных объявлений, и при необходимости любые отдельные элементы управления, которые вы хотите зарегистрировать в качестве реагирующих на нажатия элементов при показе рекламы. Это требуется для правильного подсчета показов рекламных объявлений и нажатий по ним.

Существует две перегрузки метода **RegisterAdContainer**, которые можно использовать.

* Если вы хотите, чтобы весь контейнер для всех отдельных элементов собственной рекламы реагировал на нажатия, вызовите **RegisterAdContainer(FrameworkElement)** и передайте элемент управления контейнера в этот метод. Например, если вы показываете все элементы управления собственной рекламой в виде отдельных элементов управления, размещенных на панели **StackPanel**, и хотите, чтобы вся панель **StackPanel** реагировала на нажатия, передайте **StackPanel** этому методу.

* Если вы хотите, чтобы на нажатия реагировали только определенные элементы управления собственной рекламы, вызовите метод **RegisterAdContainer (FrameworkElement, IVector(FrameworkElement))** . Только те элементы управления, которые вы передаете второму параметру, будут реагировать на нажатия.

### <a name="required-native-ad-elements"></a>Обязательные элементы собственной рекламы

В ваших собственных рекламных конструкциях, как минимум, пользователю всегда должны отображаться следующие элементы собственных рекламных объявлений, предоставляемые свойствами объекта [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2). Если вы не включите эти элементы, вероятно, производительность рекламы и доходы от вашей группы объявлений будут низкими.

1. Всегда отображайте заголовок собственной рекламы (доступен в свойстве **Title**). Предоставляйте достаточно места для отображения как минимум 25 символов. Если заголовок длиннее, замените не помещающийся на экран текст многоточием.
2. Всегда отображайте как минимум один из следующих элементов, чтобы помочь отличить собственную рекламу от остальных частей приложения и ясно указать, что определенное содержимое предоставляется рекламодателем.
    * Заметный значок *реклама* (доступен в свойстве **AdIcon**). Этот значок предоставляется корпорацией Майкрософт.
    * Надпись *спонсируется* (доступна в свойстве **SponsoredBy**). Этот текст предоставляется рекламодателем.
    * В качестве альтернативы для надписи *спонсируется* можно выбрать для отображения какой-либо другой текст, который помогает отличить собственную рекламу от остальных частей приложения, например «Оплаченное содержимое», «Рекламное содержимое», «Рекомендуемое содержимое» и т. п.

### <a name="user-experience"></a>Взаимодействие с пользователем

Ваша собственная реклама должна быть четко отделена от остальных частей приложения и вокруг нее должно быть свободное пространство, чтобы предотвратить случайные нажатия. Используйте границы, другой фон и некоторые другие элементы пользовательского интерфейса для отделения рекламного содержимого от остального приложения. Следует помнить, что в долгосрочной перспективе случайные нажатия не увеличат вашу прибыль от рекламы и не улучшат впечатление пользователей от приложения.

### <a name="description"></a>Описание

Если вы решили показывать описание для рекламы (доступно в свойстве **Description** объекта **NativeAdV2**), выделите достаточно места для отображения по крайней мере 75 символов. Рекомендуется использовать анимацию для отображения всего содержимого описания рекламы.

### <a name="call-to-action"></a>Призыв к действию

Текст *призыва к действию* (доступен в свойстве **CallToAction** объекта **NativeAdV2**) является важнейшим компонентом рекламы. Если вы решили показывать этот текст, выполняйте следующие рекомендации.

* Всегда отображайте текст *призыва к действию* пользователю на реагирующих на нажатия элементах управления, например кнопках и гиперссылках.
* Всегда отображайте текст *призыва к действию* в полном объеме.
* Убедитесь, что текст *призыва к действию* отделен от другого рекламного текста рекламодателя.
