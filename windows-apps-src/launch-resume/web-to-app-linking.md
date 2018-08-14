---
author: TylerMSFT
title: Включение приложений для веб-сайтов с помощью обработчиков URI приложения
description: Диск взаимодействия пользователей с вашего приложения за счет поддержки приложений для функции веб-сайтов.
keywords: Глубокие связи Windows
ms.author: twhitney
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 8482c3b14a6845dc3bfd5912c8260b5cd3214249
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2018
ms.locfileid: "958317"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>Включение приложений для веб-сайтов с помощью обработчиков URI приложения

Ваше приложение связывает приложений для веб-сайтов с веб-сайта, чтобы при открытии ссылка на веб-сайте ваше приложение запускается вместо открытия в браузере. Если ваше приложение не установлен, веб-сайта как обычно открывается в браузере. Пользователи могут доверять такому подходу, поскольку только проверенные владельцы содержимого могут зарегистрироваться для предоставления ссылки. Пользователи смогут проверить все их зарегистрированных ссылки веб приложения, перейдя на параметры > приложения > приложений для веб-сайтов.

Включение веб к app связывание вам понадобится:
- Определите URI, которые ваше приложение будет обрабатывать, в файле манифеста
- Файл JSON, который определяет связь между вашего приложения и веб-сайта. с приложением имя семейства пакет в корне узла же как приложение манифеста объявления.
- Обработка активации в приложении.

> [!Note]
> Начиная с обновления создателей 10 Windows, поддерживаемые выбранных ссылок в пограничного сервера Microsoft запуска соответствующего приложения. Поддерживаемые выбранных ссылок в других браузерах (например Internet Explorer, и т.д.), будет сохранено в работу в Интернете.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>Регистрация для обработки ссылок вида http и https в манифесте приложения

Ваше приложение должно распознавать URI для веб-сайтов, которые оно будет обрабатывать. Для этого добавьте регистрацию расширения **Windows.appUriHandler** в файл манифеста вашего приложения **Package.appxmanifest**.

Например, если адрес вашего сайта — msn.com, следует внести следующую запись в манифест приложения:

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

Приведенное выше объявления регистрирует ваше приложение для обработки ссылок, относящихся к указанному узлу. Если у вашего сайта несколько адресов (например, m.example.com, www.example.com и example.com), добавьте отдельную запись `<uap3:Host Name=... />` в разделе `<uap3:AppUriHandler>` для каждого адреса.

## <a name="associate-your-app-and-website-with-a-json-file"></a>Связывание приложения и веб-сайта в JSON-файле

Для обеспечения возможности открытия расположенного на сайте содержимого вашим приложением добавьте имя семейства пакетов вашего приложения в JSON-файл, расположенный в корневом каталоге вашего веб-сервера или в известном каталоге в домене. Это указывает, что ваш сайт дает согласие на открытие содержимого в перечисленных приложениях. Имя семейства пакетов можно найти в разделе Packages в конструкторе манифеста приложения.

>[!Important]
> JSON-файл не должен иметь расширение .json.

Создайте JSON-файл (без расширения .json) с именем **windows-app-web-link** и укажите имя семейства пакетов вашего приложения. Пример:

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows установит https-соединение с вашим сайтом и будет искать соответствующий JSON-файл на вашем сервере.

### <a name="wildcards"></a>Подстановочные символы

В приведенном выше примере JSON-файла демонстрируется использование подстановочных символов. Подстановочные символы позволяют поддерживать разнообразные ссылки, используя меньше строк кода. Привязка приложений к Интернету поддерживает два типа подстановочных символов в файле JSON:

| **Подстановочный символ** | **Описание**               |
|--------------|-------------------------------|
| **\***       | Представляет любую подстроку      |
| **?**        | Представляет единичный символ |

Например, `"excludePaths" : [ "/news/*", "/blog/*" ]` в приведенном выше примере приложение будет поддерживать все пути, начинающиеся со адрес веб-сайта (например msn.com), **за исключением** тех, в разделе `/news/` и `/blog/`. **msn.com/weather.html** будет поддерживаться, а ****msn.com/news/topnews.html**** — нет.

### <a name="multiple-apps"></a>Несколько приложений

Если вы хотите привязать к своему сайту два приложения, укажите оба имени семейства пакетов приложения в вашем JSON-файле **windows-app-web-link**. Возможна поддержка обоих приложений. Пользователю будет предлагаться выбрать способ открытия ссылки по умолчанию, если установлены оба приложения. Если в дальнейшем пользователь захочет изменить способ открытия ссылок по умолчанию, это можно сделать в разделе **Параметры > Приложения для веб-сайтов**. Разработчики также могут изменить файл JSON в любое время, и изменения могут вступить в силу в тот же день, но не позднее чем через 8 дней после обновления.

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, e.g. MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

Для обеспечения наилучшего взаимодействия пользователя с приложением используйте пути исключений, чтобы предотвратить обращение к доступному только через Интернет содержимому в поддерживаемых путях в файле JSON.

Пути исключений проверяются в первую очередь, и, если обнаруживается соответствие, эта страница будет открыта в браузере, а не в заданном приложении. В приведенном выше примере "/news/\*" включает в себя все страницы с таким путем, тогда как "/news\*" (без косой черты после "news") включает в себя все пути, начинающиеся с "news\*", такие как "newslocal/", "newsinternational/" и т. п.

## <a name="handle-links-on-activation-to-link-to-content"></a>Обработка ссылок на активацию для привязки ссылки к содержимому

Перейдите к файлу **App.xaml.cs** в вашем проекте Visual Studio и добавьте обработку связанного содержимого в разделе **OnActivated()**. В следующем примере страница, открываемая в приложении, зависит от URI:

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Важно!** Не забудьте заменить заключительный фрагмент `if (rootFrame.Content == null)` кодом `rootFrame.Navigate(deepLinkPageType, e);`, как показано в примере выше.

## <a name="test-it-out-local-validation-tool"></a>Тестирование: локальное средство проверки

Вы можете тестировать конфигурацию своего приложения и веб-сайта путем запуска средства проверки регистрации хоста приложения, расположенного в папке:

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

Проверяйте конфигурацию своего приложения и веб-сайта путем запуска этого средства со следующими параметрами:

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   Hostname: ваш веб-сайт (например, microsoft.com)
-   Package Family Name: имя семейства пакетов вашего приложения
-   File path: JSON-файл для локальной проверки (например, C:\\SomeFolder\\windows-app-web-link)

Если средство не возвращает все действия, проверки будет работать с этим файлом при загрузке. Если код ошибки, оно не будет работать.

Можно включить следующий раздел реестра для принудительного соответствия для загрузки со стороны приложений при проверке локальных путей:

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

Keyname: `ForceValidation` значение: `1`

## <a name="test-it-web-validation"></a>Тестирование: проверка в Интернете

Закройте свое приложение, чтобы убедиться, что приложение запускается, когда вы нажимаете на ссылку. Затем скопируйте адрес одного из поддерживаемых путей на вашем веб-сайте. Например, если адрес вашего веб-сайта — msn.com, и один из путей поддержки — path1, следует использовать `http://msn.com/path1`

Убедитесь, что ваше приложение закрыто. Нажмите **клавишу Windows + R**, чтобы открыть диалоговое окно **Выполнить**, и вставьте ссылку в этом окне. Вместо браузера должно запуститься ваше приложение.

Кроме того, можно протестировать приложение, запустив его из другого приложения с помощью API [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx). Можно также использовать этот API для тестирования на телефонах.

Если вы хотите отслеживать логику активации протокола, установите точку останова в обработчике событий **OnActivated**.

## <a name="appurihandlers-tips"></a>Советы по AppUriHandlers.

- Следите за тем, чтобы были указаны только те ссылки, которые ваше приложение может обработать.
- Перечисляйте все узлы, которые вы будете поддерживать.  Обратите внимание, что www.example.com и example.com — это различные узлы.
- Пользователи могут выбрать, какое приложение они предпочитают использовать для открытия сайтов, в Параметрах.
- Файл JSON должен быть размещен на https-сервере.
- Если вам нужно изменить пути, которые вы хотите поддерживать, можно повторно опубликовать JSON-файл, не переиздавая приложение. Пользователи увидят изменения через 1-8 дней.
- Все неопубликованные приложения с AppUriHandlers будут иметь проверенные ссылки для данного узла при установке. Для проверки функциональности не требуется передавать JSON-файл на сайт.
- Эта функция работает во всех случаях, когда ваше приложение является приложением UWP и запускается с помощью [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx) или является приложением для настольной версии Windows и запускается с помощью [ShellExecuteEx](https://msdn.microsoft.com/library/windows/desktop/bb762154(v=vs.85).aspx). Если URL-адрес совпадает с зарегистрированным обработчиком URI приложения, вместо браузера будет запущено приложение.

## <a name="see-also"></a>См. также

[Пример веб-приложения project](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[windows.protocol регистрации](https://msdn.microsoft.com/library/windows/apps/br211458.aspx)
[Обрабатывать активации URI](https://msdn.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[связь запуск примера](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) показано, как использовать LaunchUriAsync() API.