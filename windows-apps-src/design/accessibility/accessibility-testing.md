---
author: Xansky
Description: Testing procedures to follow to ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: Проверка специальных возможностей
label: Accessibility testing
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b059a3088f3f8f9491cb3cccfdddee0a3eeb707
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
ms.locfileid: "1655223"
---
# <a name="accessibility-testing"></a>Проверка специальных возможностей  

Процедуры проверки работоспособности специальных возможностей в приложении универсальной платформы Windows (UWP).

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>

## <a name="run-accessibility-testing-tools"></a>Запуск средств проверки специальных возможностей  
Пакет средств разработки программного обеспечения для Windows (SDK) включает несколько средств проверки работоспособности специальных возможностей, например [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239), [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) и [**UI Accessibility Checker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985). Эти средства помогут вам проверить работу специальных возможностей приложения. Обязательно проверьте все сценарии приложения и элементы пользовательского интерфейса.

Можно запустить средства проверки специальных возможностей из командной строки Microsoft Visual Studio или из папки средств Windows SDK (вложенная папка bin, в которой на компьютере разработки установлен пакет Windows SDK).

<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>

### **<a name="accscope"></a>AccScope**  

Средство [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) позволяет разработчикам и тест-инженерам оценивать специальные возможности своих приложений в процессе разработки, возможно даже на стадии прототипа, а не на поздних этапах тестирования в цикле разработки приложения. Программное средство, в основном, предназначено для тестирования сценариев специальных возможностей экранного диктора в приложении.

<span id="inspect"/>
<span id="INSPECT"/>

### **<a name="inspect"></a>Средство Inspect**  

[Средство **Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) позволяет выбрать любой элемент пользовательского интерфейса и просмотреть данные о его специальных возможностях. Можно просмотреть свойства модели автоматизации пользовательского интерфейса и шаблоны элементов управления, а также проверить навигационную структуру элементов автоматизации в дереве автоматизации пользовательского интерфейса. Используйте средство **Inspect** в процессе разработки пользовательского интерфейса, чтобы проверить, как атрибуты специальных возможностей отображаются в модели автоматизации пользовательского интерфейса. В некоторых случаях атрибуты определяются поддержкой модели автоматизации пользовательского интерфейса, уже реализованной для элементов управления XAML по умолчанию. В других случаях атрибуты определяются конкретными значениями, заданными в разметке XAML, как присоединенные свойства [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties).

На следующем изображении показано средство [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521), запрашивающее свойства модели автоматизации пользовательского интерфейса для элемента меню **Правка** в Блокноте.

![Снимок экрана средства Inspect.](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>

### **<a name="ui-accessibility-checker"></a>Средство UI Accessibility Checker**  
**Средство проверки специальных возможностей пользовательского интерфейса (AccChecker)** помогает выявлять проблемы со специальными возможностями во время выполнения. Когда ваш пользовательский интерфейс будет завершен и начнет работать, воспользуйтесь **AccChecker**, чтобы проверить работу различных сценариев, правильность сведений о специальных возможностях среды выполнения и выявить возможные проблемы выполнения. Можно запустить **AccChecker** в режиме пользовательского интерфейса или из командной строки. Чтобы запустить средство в режиме пользовательского интерфейса, откройте каталог **AccChecker** в каталоге bin Windows SDK, запустите файл acccheckui.exe и выберите пункт меню **Справка**.

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>

### **<a name="ui-automation-verify"></a>Средство UI Automation Verify**  
**Проверка модели автоматизации пользовательского интерфейса (UIA Verify)** — это платформа для автоматизированного тестирования и проверки реализации моделей автоматизации пользовательского интерфейса. **UIA Verify** можно интегрировать в тестовый код, чтобы проводить регулярное автоматическое тестирование или точечную проверку сценариев модели автоматизации пользовательского интерфейса. Чтобы запустить средство **UIA Verify**, запустите файл VisualUIAVerifyNative.exe из вложенной папки UIAVerify.

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>

### **<a name="accessible-event-watcher"></a>Средство Accessible Event Watcher**  
[**Средство отслеживания доступных событий (AccEvent)**](https://msdn.microsoft.com/library/windows/desktop/Dd317979) проверяет, запускают ли элементы пользовательского интерфейса приложения соответствующие события модели автоматизации пользовательского интерфейса и Microsoft Active Accessibility при изменении элементов интерфейса. Изменения в пользовательском интерфейсе могут произойти при изменении фокуса или при вызове элемента интерфейса, его выборе или изменении его состояния или свойств.

> [!NOTE]
> Большинство указанных в документации средств для тестирования специальных возможностей работает на стационарном компьютере под управлением Windows, а не на телефоне. Вы можете запускать некоторые из этих средств во время разработки и использования эмулятора, но большинство из них не предоставляют дерево автоматизации пользовательского интерфейса в эмуляторе.

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>

## <a name="test-keyboard-accessibility"></a>Проверка специальных возможностей клавиатуры  
Лучший способ проверить специальные возможности вашей клавиатуры — отключить мышь или воспользоваться экранной клавиатурой, если вы пользуетесь планшетом. Проверьте навигацию с помощью специальных возможностей клавиатуры посредством клавиши _TAB_. У вас должна быть возможность перемещаться от одного интерактивного элемента пользовательского интерфейса к другому по кругу с помощью клавиши _TAB_. При наличии составных элементов пользовательского интерфейса проверьте, можете ли вы перемещаться между вложенными элементами с помощью клавиш со стрелками. Например, должна быть возможность перемещения между элементами списка с помощью клавиш. Затем убедитесь, что вы можете задействовать любые элементы пользовательского интерфейса с помощью клавиатуры при помещении на них фокуса (обычно для этого используется клавиша ПРОБЕЛ или клавиша ВВОД).

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>

## <a name="verify-the-contrast-ratio-of-visible-text"></a>Проверка коэффициента контрастности видимого текста  
Проверьте допустимость контрастности видимого текста средствами измерения цветового контраста. Исключениями являются неактивные элементы пользовательского интерфейса, а также логотипы или декоративный текст, который не содержит информации и который можно преобразовать без изменения смысла. Дополнительную информацию о коэффициенте контрастности и исключениях см. в разделе [Требования специальных возможностей для текста](accessible-text-requirements.md). Сведения о средствах проверки коэффициента контрастности см. в разделе [Методики WCAG 2.0 G18 (Раздел ресурсов)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Некоторые средства, перечисленные в методиках для WCAG2.0 G18, нельзя интерактивно использовать с приложениями UWP. Возможно, вам потребуется вручную указать в программном средстве значения цветов фона и переднего плана, выполнить снимки экрана пользовательского интерфейса приложения, а затем обработать их средством измерения цветового контраста. Также можно запустить средство при открытии исходных файлов bitmap в графическом редакторе вместо загрузки этого изображения приложением.

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>

## <a name="verify-your-app-in-high-contrast"></a>Проверка приложения при высокой контрастности  
Используйте ваше приложение, активизировав тему с высокой контрастностью, чтобы убедиться, что все элементы пользовательского интерфейса отображаются правильно. Весь текст должен хорошо читаться, а изображения должны быть четкими. При обнаружении проблем с элементами управления настройте ресурсы словаря тем XAML или шаблоны элементов управления. В тех случаях, когда выясняется, что заметные проблемы высокой контрастности вызваны не темами или элементами управления (например, из-за файлов изображений), разработайте отдельные версии для работы с включенной темой высокой контрастности.

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>

## <a name="verify-your-app-with-display-settings"></a>Проверка приложения с параметрами экрана  

Используя системные параметры отображения, регулирующие количество точек на дюйм, убедитесь в правильности масштабирования пользовательского интерфейса вашего приложения при изменении этого значения. (Некоторые пользователи меняют значения DPI в качестве параметра специальных возможностей; эта функция доступна в разделе **Специальные возможности** и в свойствах экрана.) При возникновении любых проблем следуйте [рекомендациям по масштабированию макетов](https://msdn.microsoft.com/library/windows/apps/Dn611863) и предоставьте дополнительные ресурсы для разных коэффициентов масштабирования.

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>

## <a name="verify-main-app-scenarios-by-using-narrator"></a>Проверка основных сценариев приложения с помощью экранного диктора  
Протестируйте качество считывания экрана с помощью экранного диктора.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**Чтобы протестировать ваше приложение с помощью экранного диктора, выполните следующие действия с клавиатурой и мышью.**
1.  Включите экранный диктор, нажав клавиши _Windows + Ctrl + ВВОД_. В версиях, предшествующих Windows 10 версии 1607, используйте для запуска экранного диктора _Windows + ВВОД_.
2.  Переходите к разным элементам приложения с помощью клавиши _TAB_, клавиш со стрелками, а также клавиши _CAPS LOCK+клавиш со стрелками_.
3.  При этом внимательно слушайте, как экранный диктор озвучивает элементы пользовательского интерфейса, проверяя следующие моменты.
    * Для каждого элемента управления убедитесь, что экранный диктор читает все видимое содержимое. Проверьте также, озвучивает ли экранный диктор имя каждого элемента управления, все применимые состояния (установлен, выбран ит.д.) и тип элемента управления (кнопка, флажок, элемент списка ит.д.).
    * Нажав клавиши _CAPS LOCK+ВВОД_, убедитесь, что экранный диктор может вызывать действие интерактивного элемента.
    * Для всех таблиц убедитесь в том, что экранный диктор правильно читает имя таблицы, описание таблицы (если доступно), а также заголовки столбцов и строк.

4.  Нажмите клавиши _CAPS LOCK+ВВОД_, чтобы вызвать функцию поиска, и убедитесь, что в списке поиска представлены все элементы управления, причем все имена элементов управления локализованы и читаются.
5.  Выключите монитор и попробуйте выполнить основные сценарии приложения с помощью только клавиатуры и экранного диктора. Чтобы вывести на экран список всех команд и клавиатурных сокращений экранного диктора, нажмите клавиши _CAPS LOCK+F1_.

Начиная с Windows 10 версии 1607 в программе экранного диктора используется новый режим разработчика. Включите режим разработчика, когда экранный диктор уже запущен, нажав _CAPS LOCK+SHIFT+ F12_. Когда режим разработчика включен, экран будет замаскирован, при этом будут выделены только доступные объекты и соответствующий текст, который доступен программно экранному диктору. Это дает хорошее визуальное представление о сведениях, которые предоставляется в экранному диктору.

**С помощью тех же действий протестируйте приложение в сенсорном режиме экранного диктора.**

> [!NOTE]
> Экранный диктор автоматически переходит в сенсорный режим на устройствах, которые поддерживают четыре и более контактных точек. Экранный диктор не поддерживает сценарии с несколькими мониторами или мультисенсорные дигитайзеры на основном экране.

1.  Познакомьтесь с пользовательским интерфейсом и изучите макет приложения.

    * **Используйте для перемещения по пользовательскому интерфейсу жесты прокрутки одним пальцем.** Используйте прокрутку влево и вправо для перемещения между элементами и прокрутку вверх и вниз для изменения категории элементов, по которым осуществляется навигация. К категориям относятся все элементы, ссылки, таблицы, заголовки ит.д. Навигация с помощью жестов прокрутки одним пальцем аналогична использованию клавиши _CAPS LOCK и клавиш со стрелками_.
    * **Используйте TAB-жесты для перемещения между фокусируемыми элементами.** Прокрутка тремя пальцами вправо или влево по функции аналогична переходу между элементами с помощью клавиш _TAB_ и _SHIFT+TAB_ на клавиатуре.
    * **Исследуйте пространство пользовательского интерфейса одним пальцем.** Проведите пальцем вверх и вниз или влево и вправо по экрану, чтобы экранный диктор озвучивал элементы, над которыми находится ваш палец. В качестве альтернативы можно использовать мышь, потому что ее логика соответствует логике перемещения одного пальца.
    * **Прочитайте все элементы управления окна и его содержимое с помощью прокрутки вверх тремя пальцами**. Это эквивалентно использованию клавиш _CAPS LOCK+W_ на клавиатуре.

    Если вы обнаружите, что важный элемент управления недоступен, возможно, существует ошибка в программировании специальных возможностей.

2.  Поработайте с элементом управления, чтобы протестировать его основные и дополнительные действия, а также поведение при прокрутке.

    К основным действиям относятся активация кнопки, размещение курсора и установка фокуса на элемент управления. Дополнительные действия— это, например, выбор элемента из списка или раскрытие кнопки, предлагающей несколько вариантов.

    * Чтобы протестировать основное действие, выполните двойное касание или нажмите одним пальцем и прикоснитесь другим.
    * Чтобы протестировать дополнительное действие, выполните тройное касание или нажмите одним пальцем и дважды прикоснитесь другим.
    * Чтобы протестировать прокрутку, попытайтесь прокрутить содержимое окна, проводя двумя пальцами в нужном направлении.

    Некоторые элементы управления предоставляют дополнительные действия. Чтобы отобразить полный список, прикоснитесь к экрану четырьмя пальцами одновременно.

    Если элемент управления реагирует на мышь или клавиатуру, но не реагирует на основные или дополнительные взаимодействия при помощи касаний, то, возможно, для этого элемента управления придется реализовать дополнительные шаблоны [Модели автоматизации пользовательского интерфейса](https://msdn.microsoft.com/library/windows/desktop/Ee684009).

Вам также следует рассмотреть возможность использования программного средства [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) для тестирования сценариев специальных возможностей экранного диктора в приложении. В [**разделе, посвященном инструменту AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239), описана настройка **AccScope** для проверки сценариев экранного диктора.

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>

## <a name="examine-the-ui-automation-representation-for-your-app"></a>Изучите представление модели автоматизации пользовательского интерфейса для вашего приложения  
Некоторые из ранее описанных инструментов тестирования для модели автоматизации пользовательского интерфейса позволяют просматривать приложение так, что его внешний вид не учитывается и приложение представляется структурой элементов автоматизации пользовательского интерфейса. Таким образом клиенты модели автоматизации пользовательского интерфейса (в основном, специальные возможности) будут взаимодействовать с приложением в сценариях с поддержкой специальных возможностей.

Инструмент [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) обеспечивает особенно интересное представление приложения, позволяя просматривать элементы автоматизации пользовательского интерфейса либо в визуальном представлении, либо в виде списка. Если используется визуальное представление, то можно переходить к его частям таким образом, чтобы сопоставить их с внешним видом пользовательского интерфейса приложения. Более того, вы можете проверять специальные возможности прототипов пользовательского интерфейса на самых ранних этапах разработки, еще до назначения всей программной логики интерфейсу. Это позволяет поддерживать баланс между визуальным взаимодействием и навигацией с поддержкой специальных возможностей в вашем приложении.

Один из тестируемых аспектов— наличие лишних элементов в представлении элементов для модели автоматизации пользовательского интерфейса. Если обнаруживаются элементы, которые не следует включать в представление, или некоторые необходимые элементы отсутствуют, можно использовать присоединенное свойство XAML [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview), чтобы настроить отображение элементов управления XAML в представлениях специальных возможностей. Просмотрев основные представления специальных возможностей, рекомендуется проверить последовательность переходов с помощью клавиши TAB и возможности навигации с помощью клавиш со стрелками, чтобы убедиться, что пользователь может перейти к каждому интерактивному компоненту, доступному в представлении элементов управления.

<span id="related_topics"/>

## <a name="related-topics"></a>Связанные темы  
* [Специальные возможности](accessibility.md)
* [Нерекомендуемые методики](practices-to-avoid.md)
* [Модель автоматизации пользовательского интерфейса](https://msdn.microsoft.com/library/windows/desktop/Ee684009)
* [Специальные возможности в Windows](http://go.microsoft.com/fwlink/p/?LinkId=320802)
* [Начало работы с экранным диктором](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)