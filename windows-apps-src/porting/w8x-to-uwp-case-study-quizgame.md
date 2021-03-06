---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: В этом разделе представлен пример того, как перенести работоспособное приложение из одноранговой игровой инфраструктуры WinRT 8,1 в приложение Windows 10 универсальная платформа Windows (UWP).
title: Пример переноса среды выполнения Windows 8.x в UWP - образец однорангового приложения QuizGame
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b03e13352717c5e414dda60fe413b00edc9ba827
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260119"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Пример переноса из среды выполнения Windows 8.x на UWP: образец приложения QuizGame




В этом разделе представлен пример того, как перенести работоспособное приложение из одноранговой игровой инфраструктуры WinRT 8,1 в приложение Windows 10 универсальная платформа Windows (UWP).

Универсальное приложение 8,1 создает две версии одного приложения: один пакет приложения для Windows 8.1 и другой пакет приложения для Windows Phone 8,1. Версия QuizGame для WinRT 8.1 использует проект универсального приложения для Windows, но выбирает другой подход и выполняет сборку функционально отдельных приложений для двух платформ. Пакет приложения Windows 8.1 выступает в роли узла для сеанса головоломки, а пакет приложения Windows Phone 8,1 играет роль клиента на узле. Две части игры-опроса связаны через одноранговую сеть.

Имеет смысл настроить эти части для использования на ПК и телефоне соответственно. Но было бы удобнее не запускать и узел, и клиент на каждом устройстве, не так ли? В этом примере мы будем переносить оба приложения в Windows 10, где они будут создаваться в одном пакете приложений, который пользователи могут устанавливать на широком спектре устройств.

Приложение использует шаблоны для представлений и моделей представлений. В результате такого чистого разделения процесс переноса этого приложения будет очень простым, как мы увидим дальше.

**Обратите внимание** ,  в этом примере предполагается, что ваша сеть настроена на отправку и получение пакетов многоадресной рассылки по UDP-группам (большинство домашних сетей, хотя ваша рабочая сеть может быть недоступна). В примере также отправляются и принимаются пакеты TCP.

 

**Обратите внимание** ,   при открытии QuizGame10 в Visual Studio, если отображается сообщение "требуется обновление Visual Studio", выполните действия, описанные в [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

 

## <a name="downloads"></a>Загрузки

[Скачать универсальное приложение QuizGame для версии 8.1](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame). Это начальное состояние приложения до переноса. 

[Скачайте приложение QuizGame10 Windows 10](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame10). Это состояние приложения сразу после переноса. 

[См. последнюю версию этого примера в GitHub](https://github.com/microsoft/Windows-appsample-networkhelper).

## <a name="the-winrt-81-solution"></a>Решение для WinRT 8.1


Вот как выглядит приложение QuizGame, которое мы собираемся переносить.

![Приложение узла QuizGame, запущенное в Windows](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

Приложение узла QuizGame, запущенное в Windows

 

![Клиентское приложение QuizGame, запущенное в Windows Phone](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

Клиентское приложение QuizGame, запущенное в Windows Phone

## <a name="a-walkthrough-of-quizgame-in-use"></a>Пошаговое руководство использования QuizGame

Это короткое описание предполагаемого использования приложения, которое предоставляет полезные сведения, если вы хотите испробовать приложение для личных целей по беспроводной сети.

Игра-опрос будет происходит в баре. В этом баре есть большой телевизор, который виден каждому. Ведущий игры-опроса использует компьютер, данные с которого выводятся на экран телевизора. На компьютере запущено "приложения узла". Всякий, кто хочет принять участие в опросе, просто устанавливает "клиентское приложение" на свой телефон или в Surface.

Приложение узла находится в режиме ожидания, а на экране телевизора воспроизводится сообщение о том, что оно готово для подключения клиентских приложений. Джоан запускает клиентское приложение на своем мобильном устройстве. Она вводит свое имя в текстовом поле **Имя игрока** и нажимает кнопку **Присоединиться к игре**. Приложение узла подтверждает то, что Джоан присоединилась к игре, отображая ее имя на экране, а клиентское приложение Джоан показывает, что ожидается начало игры. Далее Maxwell выполняет те же шаги на своем мобильном устройстве.

Ведущий нажимает кнопку **Начать игру** и приложение узла отображает вопросы и возможные ответы на них (также отображается список игроков обычным шрифтом серого цвета). Одновременно, ответы отображаются в виде кнопок на экранах подключенных клиентских устройств. Джоан нажимает кнопку с ответом "1975", после чего все кнопки на ее устройстве отключаются. В приложении узла имя Джоан отображается теперь зеленым цветом (и выделяется жирным шрифтом) для подтверждения получения ее ответа. Максвелл также дает свой ответ. Ведущий, отметив, что имена всех игроков стали зелеными, нажимает кнопку **Следующий вопрос**.

Опрос продолжается таким же способом. Когда на приложении узла отображается последний вопрос, содержимое кнопки **Следующий вопрос** меняется на **Показать результаты**. При нажатии на кнопку **Показать результаты** отображаются результаты игры-опроса. Щелчок по кнопке **Вернуться на основную страницу** возвращает игру в начало ее цикла за исключением того, что присоединенные игроки остаются присоединенными. Но возврат на основную страницу дает возможность присоединиться новым игрокам, а также удобное время для выхода присоединенных игроков из приложения (хотя присоединенный игрок и так может выйти в любое время, нажав кнопку **Выйти из игры**).

## <a name="local-test-mode"></a>Режим локального тестирования

Чтобы протестировать приложение и его взаимодействия на одном компьютере (вместо нескольких устройств), можно выполнить построение приложения узла в режиме локального тестирования. Этот режим полностью обходит использование сети. Вместо этого, пользовательский интерфейс приложения узла отображается в левой части окна, а в правой части окна вертикальной стопкой отображаются две копии пользовательского интерфейса клиентского приложения (обратите внимание, что в этой версии размер пользовательского интерфейса режима локального тестирования зафиксирован для использования на экране компьютера, он не адаптируется для использования на небольших устройствах). Эти сегменты пользовательского интерфейса, собранные вместе в одном приложении, связываются друг с другом через клиентский коммуникатор, который имитирует взаимодействия, которые иначе происходили бы по сети.

Чтобы активировать режим локального тестирования, укажите **LOCALTESTMODEON** (в свойствах проекта) как символ условной компиляции и выполните перестроение.

## <a name="porting-to-a-windows10-project"></a>Перенос в проект Windows 10

В QuizGame присутствуют следующие элементы.

-   P2PHelper. Эта переносимая библиотека классов, которая содержит логику одноранговых сетей.
-   QuizGame.Windows. Это проект, который создает пакет приложения для ведущего приложения, предназначенного для Windows 8.1.
-   QuizGame.WindowsPhone. Этот проект создает пакет приложения для клиентского приложения, которое будет работать в Windows Phone 8.1.
-   QuizGame.Shared. Этот проект содержит исходный код, файлы разметки и другие ресурсы, которые используются обоими приложениями.

В этом примере мы выберем обычные параметры, описанные в разделе [Если у вас универсальное приложение для версии 8.1](w8x-to-uwp-root.md), которые относятся к поддержке устройств.

На основе этих параметров мы будем переносить Куизгаме. Windows в новый проект Windows 10 с именем Куизгамехост. Мы передадим Куизгаме. WindowsPhone в новый проект Windows 10 с именем Куизгамеклиент. Эти проекты предназначены для семейства универсальных устройств, поэтому они будут работать на любом устройстве. А исходные файлы QuizGame.Shared и пр. мы оставим в той папке, где они находятся, и будем взаимодействовать с этими общими файлами в двух новых проектах. Как и раньше, мы будем использовать одно решение и назовем его QuizGame10.

**Решение QuizGame10**

-   Создайте новое решение (**Новый проект** &gt; **других типов проектов** &gt; **решений Visual Studio**) и назовите его QuizGame10.

**P2PHelper**

-   В решении создайте новый проект библиотеки классов Windows 10 (**Новый проект** &gt; библиотека классов **универсальных** &gt; Windows **(Windows Universal)** ) и назовите ее P2PHelper.
-   Удалите файл Class1.cs из нового проекта.
-   Скопируйте файлы P2PSession.cs, P2PSessionClient.cs и P2PSessionHost.cs в папку нового проекта и добавьте скопированные файлы в новый проект.
-   Проект будет создан без дальнейших изменений.

**Общие файлы**

-   Скопируйте общие папки, модель, представление и ViewModel из \\Куизгаме. Shared\\ в \\\\QuizGame10.
-   Папки "Common", "Model", "View" и "ViewModel" — это то, что мы будем иметь в виду, ссылаясь на общие папки на диске.

**куизгамехост**

-   Создайте новый проект приложения Windows 10 (**добавьте** &gt; **Новый проект** &gt; **универсальное** &gt; Windows **пустое приложение (Windows Universal)** ) и назовите его куизгамехост.
-   Добавьте ссылку на P2PHelper (**Добавить ссылочные** &gt; **проекты** &gt; **решение** &gt; **P2PHelper**).
-   В **Обозревателе решений** создайте новую папку для каждой из общих папок на диске. В свою очередь щелкните правой кнопкой мыши каждую созданную папку и выберите команду **добавить** &gt; **существующий элемент** и перейдите к папке. Откройте соответствующую общую папку, выберите все файлы и нажмите **Добавить как связь**.
-   Скопируйте MainPage. XAML из \\Куизгаме. Windows\\ в \\Куизгамехост\\ и измените пространство имен на Куизгамехост.
-   Скопируйте App. XAML из \\Куизгаме. Shared\\ в \\Куизгамехост\\ и измените пространство имен на Куизгамехост.
-   Мы не будем перезаписывать файл app.xaml.cs, а сохраним его версию в новом проекте и просто внесем одно изменение для обеспечения поддержки режима локального тестирования. В файле app.xaml.cs замените строку кода:

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

на

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   В **свойствах** &gt; **Сборка** &gt; **символы условной компиляции**добавьте локалтестмодеон.
-   Теперь вы сможете вернуться к коду, который вы добавили в файл app.xaml.cs и решить вопрос с типом TestView.
-   В файле package.appxmanifest, измените имя возможности "internetClient" на "internetClientServer".

**куизгамеклиент**

-   Создайте новый проект приложения Windows 10 (**добавьте** &gt; **Новый проект** &gt; **универсальное** &gt; Windows **пустое приложение (Windows Universal)** ) и назовите его куизгамеклиент.
-   Добавьте ссылку на P2PHelper (**Добавить ссылочные** &gt; **проекты** &gt; **решение** &gt; **P2PHelper**).
-   В **Обозревателе решений** создайте новую папку для каждой из общих папок на диске. В свою очередь щелкните правой кнопкой мыши каждую созданную папку и выберите команду **добавить** &gt; **существующий элемент** и перейдите к папке. Откройте соответствующую общую папку, выберите все файлы и нажмите **Добавить как связь**.
-   Скопируйте MainPage. XAML из \\Куизгаме. WindowsPhone\\ в \\Куизгамеклиент\\ и измените пространство имен на Куизгамеклиент.
-   Скопируйте App. XAML из \\Куизгаме. Shared\\ в \\Куизгамеклиент\\ и измените пространство имен на Куизгамеклиент.
-   В файле package.appxmanifest, измените имя возможности "internetClient" на "internetClientServer".

Теперь вы сможете выполнить построение и запуск.

## <a name="adaptive-ui"></a>Адаптивный пользовательский интерфейс

Приложение Куизгамехост Windows 10 выглядит нормально при работе приложения в широком окне (это возможно только на устройстве с большим экраном). Когда окно приложения узкое (это часто случается на небольших устройствах, но может также произойти и на большом устройстве), интерфейс сжимается настолько, что становится нечитабельным.

Чтобы это исправить, мы можем использовать адаптивную функцию диспетчера визуальных состояний, как объяснялось в разделе [Практический пример. Bookstore2](w8x-to-uwp-case-study-bookstore2.md). Сначала задайте свойства визуальных элементов, так чтобы по умолчанию пользовательский интерфейс помещался в узком состоянии. Все эти изменения выполняются в \\View\\Хоствиев. XAML.

-   В главном элементе **Grid** измените значение параметра **Height** первого **RowDefinition** с "140" на "Auto".
-   В элементе **Grid**, который содержит **TextBlock** под названием `pageTitle` установите значения `x:Name="pageTitleGrid"` и `Height="60"`. Эти первые два шага нужны для эффективного управления высотой **RowDefinition** с помощью метода задания в визуальном состоянии.
-   Для `pageTitle` установите `Margin="-30,0,0,0"`.
-   Для **Grid** с комментарием `<!-- Content -->` установите `x:Name="contentGrid"` и `Margin="-18,12,0,0"`.
-   Для **TextBlock** сразу над комментарием `<!-- Options -->` установите `Margin="0,0,0,24"`.
-   В стиле по умолчанию **TextBlock** (первый источник в файле) измените значение **FontSize** метода задания на "15".
-   В `OptionContentControlStyle` измените значение **FontSize** метода задания на "20". Этот и предыдущий шаги обеспечивают возможность использования набора шрифтов, которые будут хорошо смотреться на всех устройствах. Это гораздо более гибкий размер, чем 30, который мы использовали для Windows 8.1ного приложения.
-   Наконец, добавьте соответствующую разметку диспетчера визуальных состояний в корневой элемент **Grid**.

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>Универсальное оформление


Вы заметите, что в Windows 10 кнопки не имеют одинаковых полей сенсорного ввода в шаблоне. Это можно исправить двумя небольшими изменениями. Сначала добавьте эту разметку в файл app.xaml в QuizGameHost и в QuizGameClient.

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

Во вторых, добавьте это средство задания в `OptionButtonStyle` в \\View\\Клиентвиев. XAML.

```xml
<Setter Property="Margin" Value="6"/>
```

Благодаря этому последнему изменению приложение будет реагировать и выглядеть так же, как и до переноса, но будет иметь дополнительное преимущество — оно будет запускаться на любом устройстве.

## <a name="conclusion"></a>Заключение

Приложение, которое мы переносили в данном примере, было относительно сложным, поскольку включало несколько проектов, библиотеку классов и достаточно большой объем кода и пользовательского интерфейса. Даже так, перенос его был достаточно простым. Некоторые из простоты переноса можно напрямую присвоить сходству между платформой разработчика Windows 10 и платформой Windows 8.1 и Windows Phone 8,1. А частично — это заслуга способа разработки начального приложения, который обеспечил возможность отделения моделей, моделей представлений и представлений.
