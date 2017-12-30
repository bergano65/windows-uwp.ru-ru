---
author: laurenhughes
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: "Создание и преобразование исходного файла сопоставления группы содержимого"
description: "Чтобы подготовить ваше приложение универсальной платформы Windows (UWP) для потоковой установки, необходимо создать файл сопоставления группы содержимого. Эта статья поможет вам создать и преобразовать файл сопоставления группы содержимого, а также даст некоторые советы и рекомендации."
ms.author: lahugh
ms.date: 4/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, сопоставление группы содержимого, потоковая установка, потоковая установка приложений uwp, исходный файл сопоставления группы содержимого"
ms.openlocfilehash: d27869f349d7ee813c1418cd0d02f82ada05e155
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/26/2017
---
# <a name="create-and-convert-a-source-content-group-map"></a>Создание и преобразование исходного файла сопоставления группы содержимого

Чтобы подготовить ваше приложение универсальной платформы Windows (UWP) для потоковой установки, необходимо создать файл сопоставления группы содержимого. Эта статья поможет вам создать и преобразовать файл сопоставления группы содержимого, а также даст некоторые советы и рекомендации.

## <a name="creating-the-source-content-group-map"></a>Создание исходного файла сопоставления группы содержимого

Вам необходимо создать файл `SourceAppxContentGroupMap.xml`, а затем воспользоваться VisualStudio или программой **MakeAppx.exe**, чтобы преобразовать его в конечную версию: `AppxContentGroupMap.xml`. Можно пропустить шаг, создав файл `AppxContentGroupMap.xml` с нуля, но рекомендуется (и это в целом проще) создать файл `SourceAppxContentGroupMap.xml` и преобразовать его, поскольку подстановочные знаки недопустимы в `AppxContentGroupMap.xml` (а они действительно полезны). 

Рассмотрим простой сценарий, в котором выгодно использовать потоковую установку приложений UWP. 

Предположим, вы создали игру UWP, но размер конечного приложения составляет более 100 ГБ. В этом случае скачивание из Магазина Windows займет много времени, что может быть неудобно. Если вы выбираете использование потоковой установки для приложения UWP, вы можете указать порядок загрузки его файлов. Если указать Магазину, что сначала нужно скачивать основные файлы, пользователь сможет раньше начать работу с вашим приложением, а остальные менее существенные файлы будут скачиваться в фоновом режиме.

> [!NOTE]
> Использование потоковой установки в значительной степени зависит от организации файлов в приложении. Рекомендуется как можно раньше подумать о макете содержимого вашего приложения с учетом потоковой установки, чтобы упростить сегментирование файлов приложения.

Сначала создадим файл `SourceAppxContentGroupMap.xml`.

Прежде чем углубиться в подробности, рассмотрим пример простого полного файла `SourceAppxContentGroupMap.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

Файл сопоставления группы содержимого имеет два основных компонента: **обязательный** раздел, который содержит группу обязательного содержимого, и **автоматический** раздел, который может содержать несколько групп автоматического содержимого.

### <a name="required-content-group"></a>Группа обязательного содержимого

Группа обязательного содержимого— это единственная группа содержимого, находящаяся внутри элемента `<Required>` в файле `SourceAppxContentGroupMap.xml`. В нее должны входить все основные файлы, необходимые для запуска приложения с минимальными возможностями взаимодействия с пользователем. Из-за компиляции .NET Native весь код (исполняемый файл приложения) должен быть частью обязательной группы, а ресурсы и другие файлы необходимо оставить для автоматических групп.

Например, если ваше приложение является игрой, обязательная группа может включать файлы, используемые в главном меню или на начальном экране игры.

Вот фрагмент нашего исходного примера файла `SourceAppxContentGroupMap.xml`: 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

Здесь нужно отметить несколько важных моментов:

- Элемент `<ContentGroup>` внутри элемента `<Required>` **должен** иметь имя "Required" (Обязательный). Это имя зарезервировано исключительно для группы обязательного содержимого, и оно не может использоваться с каким-либо другим элементом `<ContentGroup>` в конечном файле сопоставления группы содержимого.
- Существует только один элемент `<ContentGroup>`. Это сделано намеренно, поскольку должна существовать только одна группа основных файлов.
- В этом примере этим файлом является единственный файл `.exe`. Группа обязательного содержимого не ограничивается одним файлом, их может быть несколько. 

Простой способ начать создание этого файла— это открыть новую страницу в любимом текстовом редакторе и сохранить этот файл под именем `SourceAppxContentGroupMap.xml` в папке проекта вашего приложения.

> [!IMPORTANT]
> Если вы разрабатываете приложение UWP на языке C++, вам необходимо настроить свойства файла `SourceAppxContentGroupMap.xml`. Задайте свойству `Content` значение **true**, а свойству `File Type`— значение **XML File**. 

Когда вы создаете файл `SourceAppxContentGroupMap.xml`, полезно использовать подстановочные знаки в именах файлов. Дополнительные сведения см. в разделе [Советы и рекомендации по использованию подстановочных знаков](#wildcards).

Если вы разрабатываете приложение с помощью Visual Studio, рекомендуется включить в группу обязательного содержимого следующее:

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

При указании имени файла с одним подстановочным символом будут включены файлы, добавленные в каталог проекта из Visual Studio, например исполняемые файлы приложения или библиотеки DLL. Папки WinMetadata и Properties используются, чтобы включить другие папки, созданные Visual Studio. Подстановочные знаки Assets используются, чтобы выбрать изображения логотипа и экрана-заставки, которые необходимы для установки приложения.

Обратите внимание, что не допускается использование двойного подстановочного знака "**" в корне структуры файлов, чтобы включить в проект все файлы, поскольку это приведет к ошибке при попытке преобразования `SourceAppxContentGroupMap.xml` в окончательный файл `AppxContentGroupMap.xml`.

Также важно отметить, что в сопоставление группы содержимого не должны включаться файлы занимаемой памяти (AppxManifest.xml, AppxSignature.p7x, resources.pri и др.). Если файлы занимаемой памяти входят в одно из указанных вами имен файлов с подстановочными знаками, они будут проигнорированы.

### <a name="automatic-content-groups"></a>Группы автоматического содержимого

Группы автоматического содержимого— это те ресурсы, которые скачиваются в фоновом режиме, пока пользователь взаимодействует с уже загруженными группами содержимого. Они содержат все дополнительные файлы, которые не являются необходимыми для запуска приложения. Например, вы можете разбить группы автоматического содержимого на различные уровни, определяя каждый уровень как отдельную группу. Как было отмечено в разделе группы обязательного содержимого, из-за компиляции .NET Native весь код (исполняемый файл приложения) должен быть частью обязательной группы, а ресурсы и другие файлы необходимо оставить для автоматических групп.

Давайте подробнее рассмотрим группу автоматического содержимого из нашего примера `SourceAppxContentGroupMap.xml`:
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

Макет автоматической группы очень похож на макет обязательной группы за некоторыми исключениями:

- Представлено несколько групп содержимого.
- Группы автоматического содержимого могут иметь уникальные имена, исключение составляет имя "Required", которое зарезервировано для группы обязательного содержимого.
- Группы автоматического содержимого не могут включать **какие бы то ни было** файлы из группы обязательного содержимого. 
- Группа автоматического содержимого может включать в себя файлы, которые указаны в других группах автоматического содержимого. Файлы будут скачаны только один раз с первой группой автоматического содержимого, в которую они включены.

#### Советы и рекомендации по использованию подстановочных знаков<a name="wildcards"></a>

Макет файла для сопоставления групп содержимого всегда размещается относительно корневой папки вашего проекта.

В нашем примере подстановочные знаки используются внутри двух элементов `<ContentGroup>`, чтобы извлечь все файлы внутри одного уровня "Assets\Level2" или "Assets\Level3". Если вы используете более глубокую структуру папок, можно использовать двойной подстановочный знак:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

Подстановочные знаки также могут использоваться с текстом для имен файлов. Например, если вы хотите включить в папку "Assets" все файлы, в имени которых содержится строка "Level2", можно указать что-то подобное:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>Преобразование SourceAppxContentGroupMap.xml в AppxContentGroupMap.xml

Чтобы преобразовать файл `SourceAppxContentGroupMap.xml` в конечную версию— `AppxContentGroupMap.xml`,— вы можете использовать VisualStudio2017 либо программу командной строки **MakeAppx.exe**.

Для преобразования файла сопоставления группы содержимого с помощью Visual Studio:
1. Добавьте файл `SourceAppxContentGroupMap.xml` в папку вашего проекта.
2. В окне "Свойства" измените "Действие сборки" для `SourceAppxContentGroupMap.xml` на "AppxSourceContentGroupMap".
2. В обозревателе решений щелкните проект правой кнопкой мыши.
3. Перейдите в Магазин -> Преобразовать файл сопоставления группы контента

Если вы не разрабатывали приложение в VisualStudio или просто предпочитаете работать с командной строкой, воспользуйтесь программой **MakeAppx.exe** для преобразования файла `SourceAppxContentGroupMap.xml`. 

Простая команда **MakeAppx.exe** может выглядеть примерно так:
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

Параметр /s указывает путь к файлу `SourceAppxContentGroupMap.xml`, а /f— путь к файлу `AppxContentGroupMap.xml`. Последний параметр /d указывает на каталог, который следует использовать для расширяющихся подстановочных знаков имен файлов; в данном случае это каталог проекта приложения.

Чтобы получить дополнительные сведения о параметрах, доступных в программе **MakeAppx.exe**, откройте окно командной строки, перейдите к **MakeAppx.exe** и введите:

```syntax
MakeAppx convertCGM /?
```

Это все, что требуется, чтобы получить окончательный готовый файл `AppxContentGroupMap.xml` для вашего приложения! Вам по-прежнему предстоит проделать много работы перед тем, как ваше приложение будет полностью готово для Магазина Windows. Дополнительные сведения о полном процессе добавления потоковой установки в ваше приложение UWP см. в [этой записи блога](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).