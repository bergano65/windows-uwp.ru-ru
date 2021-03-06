---
Description: Набор многоязычных приложений (Toolkit) 4,0 интегрируется с Microsoft Visual Studio 2019 для предоставления приложений UWP с поддержкой перевода, управления файлами перевода и инструментами для редактора.
title: Использование набора средств для многоязычных приложений
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: windows 10, uwp, глобализация, локализуемость, локализация
ms.localizationpriority: medium
ms.openlocfilehash: 34bc609d06705f1dfa6a5c7370ce6022ae9c3ff8
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684239"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Использование набора средств для многоязычных приложений версии 4.0

Набор многоязычных приложений (Toolkit) 4,0 интегрируется с Microsoft Visual Studio 2019 для предоставления приложений UWP с поддержкой перевода, управления файлами перевода и инструментами для редактора. Вот некоторые преимущества этого набора средств.

- Помогает управлять изменениями ресурсов и состоянием перевода во время разработки.
- Предоставляет пользовательский интерфейс для выбора языков на основе настроенных поставщиков перевода.
- Поддерживает формат файлов XLIFF, соответствующий отраслевым стандартам локализации.
- Предоставляет механизм псевдоязыка, помогающий выявить проблемы с переводом во время разработки.
- Обеспечивает удобный доступ к переведенным строкам и терминологии через Языковой портал Майкрософт.
- Подключается к Microsoft Translator и быстро предлагает варианты перевода.

## <a name="how-to-use-the-toolkit"></a>Использование набора средств

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Шаг 1. Режим для глобализации и локализации приложения

Прежде чем вы сможете эффективно использовать MAT ваше приложение должно быть подготовлено к локализации. В частности, проект должен содержать один или несколько файлов ресурсов (.resw), содержащих строки вашего приложения на языке по умолчанию. Дополнительные сведения см. в разделе [Локализация строк в манифесте пакета приложения и интерфейсе пользователя](../../app-resources/localize-strings-ui-manifest.md). После этого вы сможете быстро и просто добавлять другие языки с помощью набора средств.

Сведения о преимуществах глобализации и локализации, а также определения терминов **глобализация**, **локализуемость** и **локализация** см. в разделе [Глобализация и локализация](globalizing-portal.md).

См. также разделы [Руководство по глобализации](guidelines-and-checklist-for-globalizing-your-app.md) и [Подготовка приложения к локализации](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Шаг 2. Скачайте и установите набор средств для многоязычных приложений версии 4.0

Есть два компонента набора средств для многоязычных приложений версии 4.0 (MAT 4.0), у каждого из которых есть свой собственный установщик.

- [Расширение пакета многоязыкового набора приложений 4,0 для Visual Studio 2017 и более поздних версий](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Он содержит расширение "центр обработки 4,0" для Visual Studio 2019 в виде установщика VSIX.
- [Редактор набора средств для многоязычных приложений версии 4.0](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit). Этот компонент содержит автономное средство "Редактор из набора средств для многоязычных приложений версии 4.0" в виде установщика .msi. Он также включает в себя расширение MAT 4.0 для Visual Studio 2015 и для Visual Studio 2013.

Если вы используете Visual Studio 2017 или Visual Studio 2019, скачайте и запустите оба установщика, один за другим. Если вы используете Visual Studio 2015 или Visual Studio 2013, скачайте и запустите установщик .msi.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Шаг 3. Включение набора средств для многоязычных приложений для вашего проекта

Для того чтобы вы смогли начать локализацию приложения, MAT должен быть включен для вашего проекта. Вот как включить набор средств.

- Откройте решение проекта в Visual Studio.
- Выберите необходимый проект в обозревателе решений.
- В меню **Сервис** выберите **Набор средств для многоязычных приложений** > **Включить**. 

В окне вывода (в котором отображаются выходные данные из набора средств для многоязычных приложений) следите за появлением следующего сообщения: `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`. Если это сообщение отобразилось, значит MAT готов к использованию.

### <a name="step-4-add-languages-to-your-project"></a>Шаг 4. Добавление языков в проект

Выполните следующие действия, чтобы добавить языки в проект.

1. В обозревателе решений щелкните узел проекта правой кнопкой мыши.
2. Выберите **Набор средств для многоязычных приложений** > **Добавить языки перевода...** .
3. В диалоговом окне "Языки перевода" выберите языки, поддержку которых необходимо обеспечить, и нажмите кнопку "ОК".

В ответ на ваши действия набор средств выполняет следующие действия.

- Для каждого добавленного языка создается новая папка по имени [языкового тега BCP-47](https://tools.ietf.org/html/bcp47) соответствующего языка. В этой папке создаются новые файлы ресурсов (.resw). Они будут совпадать с файлами ресурсов, содержащими строки языка по умолчанию.
- Если вы добавляете язык в первый раз, в проект добавляется папка с именем `MultilingualResources`. В этой папке для каждого языка добавляется файл .xlf. Файлы .xlf содержат единицу перевода для каждой строки в каждом файле ресурсов (.resw) вашего проекта.
- В окне вывода отображается подтверждение добавленных языков.

Всякий раз при добавлении/удалении файла ресурсов (.resw) языка по умолчанию или при добавлении/удалении строки внутри файла ресурсов (.resw) языка по умолчанию необходимо перестроить проект, чтобы повторно синхронизировать файлы .xlf. Это гарантирует, что файлы .xlf будут содержать объединение строк на языке по умолчанию.

Для перевода ресурсов приложения можно использовать установленные поставщики перевода, такие как [Языковой портал Майкрософт](https://www.microsoft.com/Language/) и [Microsoft Translator](https://www.microsofttranslator.com/). Если поставщик поддерживает определенный язык, рядом с названием языка отображается значок поставщика в диалоговом окне "Языки перевода".

В диалоговом окне "Языки перевода" любые существующие языки на основе XLF, обнаруживаемые набором средств, получают отметку в соответствующем поле выбора, указывающую на то, что данный язык уже включен в проект.

После добавления в проект язык нельзя удалить снятием флажка в диалоговом окне "Языки перевода". Чтобы удалить язык, правой кнопкой мыши щелкните соответствующий XLF-файл и выберите пункт **Удалить**. При выборе этого пункта также происходит удаление соответствующего файла ресурсов (.resw).

### <a name="step-5-test-your-app-using-pseudo-language"></a>Шаг 5. Тестирование приложения с помощью псевдоязыка

Псевдоязык — это искусственная модификация программного продукта, имитирующая настоящую локализацию для языка, но при этом воспринимаемая носителями данного языка. Псевдоперевод заменяет символы и увеличивает длину строки ресурсов для поиска потенциальных проблем с локализацией или ошибок на раннем этапе проекта и до того, как начнется непосредственно сама локализация.

Выполните следующие действия, чтобы псевдолокализовать и протестировать проект.

1. Используйте диалоговое окно "Языки перевода" для добавления в проект псевдоязыка (псевдо) [qps ploc].
2. Щелкните правой кнопкой мыши файл `<project-name>.qps-ploc.xlf` в обозревателе решений и выберите **Набор средств для многоязычных приложений** > **Создать машинный перевод**.
3. В разделе **Параметры** > **Время и язык** > **Язык и региональные стандарты** > **Языки** нажмите кнопку **Добавить язык**.
5. В поле поиска введите `qps-ploc`.
6. Нажмите `English (qps-ploc)`, чтобы добавить язык.
7. В списке языков выберите `English (qps-ploc)` и нажмите кнопку **По умолчанию**.
8. Протестируйте свое псевдолокализованнное приложение. Например, проверьте приложение на наличие проблем макета пользовательского интерфейса, таких как отображение не всех строк (строки обрезаны) или наличие непереведенных строк (вместо этого строки жестко закодированы).

Помимо замены символов и удлинения строк, псевдообработчик обеспечивает каждому ресурсу уникальный отслеживающий идентификатор. Такой трекер присоединяется к началу каждой строки и заключается в квадратные скобки `[xxxxx]`. Эти трекеры можно использовать в ходе тестирования визуальной проверки пользовательского интерфейса. Они могут помочь в отслеживании конкретных ресурсов в продукте, особенно если у нескольких ресурсов имеется похожий или дублирующийся текст.

В следующем примере текста "Hello, World!" псевдоперевод увеличивает занимаемое им место на экране приблизительно на 30 % и применяет трекер ресурса.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>Шаг 6. Перевод приложения на выбранные языки

Набор средств для многоязычных приложений участвует в процессе сборки. Во время каждой сборки обновленные строки автоматически добавляются в каждый языковой XLF-файл.
После того как вы протестировали свое приложение с помощью псевдоязыка, вы можете выбрать один из трех вариантов перевода своего приложения на другие языки для выпуска.

#### <a name="option-1-translate-the-strings-yourself"></a>Вариант 1. Переведите строки самостоятельно

Для самостоятельного перевода строк можно использовать многоязычный редактор. Как уже упоминалось, он входит в состав [установщика MSI](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit).

- Правой кнопкой мыши щелкните XLF-файл, который вы хотите перевести.
- Нажмите кнопку **Открыть с помощью...**  и выберите многоязычный редактор. Можно также нажать кнопку **По умолчанию** .
- Для каждой строки параметр **Источник** содержит исходную строку на языке по умолчанию. В поле **Перевод** введите перевод строки на соответствующем языке для XLF-файла, который требуется изменить.
- Когда все будет готово, сохраните и закройте файл.

Перестройте проект, чтобы переведенные строки скопировались в файл ресурсов (.resw), соответствующий XLF-файлу, который вы только что редактировали.

Также можно запустить многоязычный редактор следующим образом. Перейдите в меню "Пуск", отобразите все приложения, откройте папку набора средств для многоязычных приложений и нажмите кнопку "Многоязычный редактор", чтобы запустить его.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Вариант 2. Отправка XLF-файлов на перевод третьим лицам

Чтобы нанять внешних исполнителей для выполнения перевода и редактирования, выберите нужные XLF-файлы в обозревателе решений, щелкните их правой кнопкой мыши и выберите **Набор средств для многоязычных приложений** > **Экспортировать перевод...** .

Выберите **Выходные данные: адресат** в диалоговом окне "Экспорт ресурсов строк" и нажмите кнопку "OK" — ваши файлы будут добавлены в ZIP-архив и вложены в новое электронное письмо. Выберите **Выходные данные: расположение папки с файлами**, найдите папку и нажмите кнопку "ОК". При необходимости выберите файлы, чтобы добавить их в ZIP-архив и снова нажмите кнопку "OK" — ваши файлы будут (добавлены в ZIP-архив) и сохранены в выбранном расположении, в новой папке с именем вашего проекта.

После того как локализаторы завершат перевод и вы получите переведенные XLF-файлы, вы можете импортировать эти файлы в свой проект. В обозревателе решений выберите нужные XLF-файлы, щелкните их правой кнопкой мыши и выберите **Набор средств для многоязычных приложений** > **Импорт/повторное использование перевода...** . Нажмите кнопку **Добавить**, перейдите к XLF-файлу или ZIP-файлам и нажмите кнопку **Импорт**.

**Примечание.** Перед импортом выполняется базовая проверка. Это гарантирует, что целевая языковая и региональная информация в импортируемых XLF-файлах соответствует информации в существующих XLF-файлах.

Перестройте проект, чтобы переведенные строки скопировались в файл ресурсов (.resw), соответствующий XLF-файлу, который вы только что импортировали.

Следующие сторонние поставщики предоставляют услуги по локализации программных продуктов и готовы помочь вам выйти на международный уровень.

- [Elanex](https://www.strakertranslations.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.rws.com/what-we-do/rws-moravia/)
- [SDL](https://www.sdl.com/translate/get-started/instant-quote.html)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> Список указанных поставщиков услуг предоставлен исключительно в информационных целях и не означает, что корпорация Майкрософт отказывает им какую-либо поддержку. Корпорация Майкрософт не делает никаких заявлений и не принимает на себя никаких обязательств в отношении этих поставщиков или их услуг, а также ни при каких обстоятельствах не несет ответственность в отношении вашего сотрудничества с такими поставщиками или использования вами их услуг. Любые вопросы, жалобы или требования относительно таких поставщиков или их услуг следует направлять в адрес соответствующего поставщика.

#### <a name="option-3-use-the-integrated-translation-services"></a>Вариант 3. Использование интегрированных служб перевода

Службы перевода интегрированы в среду IDE Visual Studio, как и в многоязычный редактор. Это обеспечивает удобный доступ к службам перевода в процессе разработки продукта и локализации ресурсов. Для использования этой службы вам потребуется подписка на учетную запись Azure, как описано в разделе [Microsoft Translator переходит на портал Azure](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal).

Чтобы получить доступ к службам перевода из Visual Studio, выберите или щелкните правой кнопкой мыши один или несколько XLF-файлов в обозревателе решений и выберите пункт **Создать машинный перевод**.

Многоязычный редактор обеспечивает такую же поддержку перевода и добавление интерактивных вариантов перевода, позволяя выбрать перевод, наиболее точно соответствующий строкам ресурсов. После добавления варианта перевода вы можете отредактировать строку в соответствии со своим стилем перевода.

В набор средств для многоязычных приложений входят два поставщика.

- [Языковой портал Майкрософт](https://www.microsoft.com/Language/) поддерживает многократное использование переводов и сопоставление с терминологией на основе переводов пользовательского интерфейса для продуктов и служб Майкрософт.
- [Microsoft Translator](https://www.microsofttranslator.com/) предоставляет службы машинного перевода по требованию.

Вы и ваши переводчики можете управлять статусом переводов в многоязыковом редакторе, чтобы позднее просмотреть переводы, вызывающие сомнения. Можно настроить состояние каждой строки на вкладке **Свойства**. Значения состояния: **New** (новый), **Needs review** (необходима проверка), **Translated** (переведено), **Final** (окончательная версия) и **Signed off** (одобрено). Индикатор слева от строки показывает состояние. Когда все строки в многоязычном редакторе отмечены зеленым, перевод завершен.

Перестройте проект, чтобы переведенные строки скопировались в файл ресурсов (.resw), соответствующий XLF-файлу, который вы только что редактировали.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Шаг 7. Отправка приложения в Microsoft Store

Перед началом сертификации для Microsoft Store вам необходимо убрать файл `<project-name>.qps-ploc.xlf` из проекта. Псевдоязык используется для обнаружения потенциальных проблем или ошибок, связанных с возможностями локализации, но не является допустимым языком Microsoft Store. Если этот файл не будет удален, приложение не пройдет сертификацию для Microsoft Store.

## <a name="related-topics"></a>Связанные темы

* [Локализация строк в манифесте пакета приложения и интерфейсе пользователя](../../app-resources/localize-strings-ui-manifest.md)
* [Глобализация и локализация](globalizing-portal.md)
* [Рекомендации по глобализации](guidelines-and-checklist-for-globalizing-your-app.md)
* [Сделайте приложение локализованным](prepare-your-app-for-localization.md)
* [Тег языка BCP-47](https://tools.ietf.org/html/bcp47)

## <a name="downloads"></a>Загрузки

* [Установщик многоязычного приложения 4,0. VSIX](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Установщик пакета многоязыкового набора приложений 4,0. msi](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Служба перевода

* [Языковой портал Майкрософт](https://www.microsoft.com/Language/)
* [Microsoft Research](https://www.microsofttranslator.com/)
