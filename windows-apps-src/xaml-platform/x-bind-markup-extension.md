---
description: Расширение разметки Ксбинд — это высокопроизводительная альтернатива привязке. Ксбинд-New для Windows 10 — выполняется меньше и меньше памяти, чем привязка и поддерживает лучшую отладку.
title: Расширение разметки xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c8fda22a565972e4157777c1db537a8f8d9ba20
ms.sourcegitcommit: 20af365ce85d3d7d3a8d07c4cba5d0f1fbafd85d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034005"
---
# <a name="xbind-markup-extension"></a>Расширение разметки x:Bind

**Примечание** .  общие сведения об использовании привязки данных в приложении с **{КС:бинд}** (и для сравнения "все сравнение" между **{КС:бинд}** и **{Binding}** ) см. в разделе [Привязка данных в подробном](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)виде.

Расширение разметки **{КС:бинд}** — новое для Windows 10 — альтернатива **{Binding}** . **{КС:бинд}** работает за меньшее время и меньше памяти, чем **{Binding}** , и поддерживает лучшую отладку.

Во время компиляции XAML **{x:Bind}** преобразуется в код, который получает значения из свойства в источнике данных и устанавливает его для свойства, определенного в разметке. Объект привязки можно дополнительно настроить таким образом, чтобы он регистрировал изменения значений свойства источника данных и сам обновлялся на основании этих данных (`Mode="OneWay"`). Кроме того, его можно настроить, чтобы он отправлял изменения собственного значения назад к свойству источника (`Mode="TwoWay"`).

Как правило, объекты привязки, создаваемые с помощью расширений разметки **{x:Bind}** и **{Binding}** , выполняют аналогичные функции. Однако **{x:Bind}** выполняет специальный код, который генерируется во время компиляции, а **{Binding}** использует универсальную проверку объектов среды выполнения. В результате привязки **{x:Bind}** (часто именуемые компилированными привязками) имеют большую производительность, обеспечивают проверку ваших выражений привязки и поддерживают отладку, позволяя задавать точки останова в файлах кода, которые создаются как разделяемый класс для вашей страницы. Эти файлы можно найти в папке `obj` с такими именами, как `<view name>.g.cs` (для C#).

> [!TIP]
> **{x:Bind}** имеет режим по умолчанию **OneTime**, в отличие от **{Binding}** с режимом по умолчанию **OneWay**. Он был выбран в целях повышения производительности, поскольку при использовании **OneWay** создается больший объем кода для подключения и обнаружения изменений. Можно явно задать режим, чтобы использовать привязку OneWay или TwoWay. [x:DefaultBindMode](x-defaultbindmode-attribute.md) можно также использовать, чтобы изменить режим по умолчанию для **{x:Bind}** для определенного сегмента дерева разметки. Выбранный режим применяет любые выражения **{x:Bind}** к этому элементу и его дочерним элементам, которые явным образом не задают режим в качестве части привязки.

**Примеры приложений, демонстрирующие {КС:бинд}**

-   [{КС:бинд} пример](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Основы пользовательского интерфейса XAML — пример](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="xaml-attribute-usage"></a>Использование атрибутов XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| Термин | Описание |
|------|-------------|
| _propertyPath_ | Строка, указывающая путь свойства для привязки. Подробную информацию см. в разделе [Путь свойства](#property-path) ниже. |
| _биндингпропертиес_ |
| =\[_значения_ _Пропнаме_ _,=_ _пропнаме_\]* | Одна или несколько привязок свойств, указанных с помощью синтаксиса пары "имя/значение". |
| _пропнаме_ | Имя строки свойства для установки на объекте привязки. Например, Converter. |
| _value_ | Значение, которое следует задать для свойства. Синтаксис аргумента зависит от задаваемого свойства. Вот пример использования _propName_=_значение_, где значение само является расширением разметки: `Converter={StaticResource myConverterClass}`. Дополнительную информацию см. в разделе [Свойства, которые можно задать с помощью расширения разметки {x:Bind}](#properties-that-you-can-set-with-xbind) ниже. |

## <a name="examples"></a>Примеры

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

В этом примере кода XAML используется **{x:Bind}** со свойством **ListView.ItemTemplate**. Обратите внимание на объявление значения **x:DataType**.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Путь к свойству

*PropertyPath* задает значение **Path** для выражения **{x:Bind}** . **Path** — путь к свойству, в котором определено значение свойства, подсвойства, поля или метода, к которому выполняется привязка (источник). Можно упомянуть точное имя свойства **Path**: `{x:Bind Path=...}`. Или его можно не указывать: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Разрешение пути свойства

Расширение разметки **{x:Bind}** не использует **DataContext** в качестве источника по умолчанию — оно использует страницу или пользовательский элемент управления. Таким образом оно будет выглядеть в коде программной части вашей страницы или пользовательского элемента управления для свойств, полей и методов. Чтобы предоставить модель представления для расширения разметки **{x:Bind}** , обычно нужно добавить новые поля или свойства в код программной части для страницы или пользовательского элемента управления. Этапы в пути к свойству разделены точками (.), и вы можете добавить несколько разделителей для прохождения по иерархии. Используйте разделительные точки независимо от языка программирования, используемого для реализации объекта, к которому осуществляется привязка.

Например, на странице: **Text="{x:Bind Employee.FirstName}"** будет искать участника **Employee** на странице, а затем участника **FirstName** в объекте, возвращенном участником **Employee**. Если бы элемент управления элементами привязывался к свойству, содержащему подчиненных сотрудников, то путем свойства мог бы быть Employee.Dependents, а шаблон элемента управления элементами, отобразил бы элементы в Dependents.

В случае использования языков C++/CX **{x:Bind}** нельзя привязать к частным полям и свойствам в модели страницы или данных — вам потребуется открытое свойство для выполнения привязки. Контактную зону для привязки необходимо предоставлять в качестве классов/интерфейсов CX, чтобы можно было получить соответствующие метаданные. Атрибут **\[bind\]** не требуется.

При использовании **x:Bind** вам не нужно применять **ElementName=xxx** как часть выражения привязки. Вместо этого можно использовать имя элемента в качестве первой части пути для привязки, так как именованные элементы становятся полями в пределах страницы или пользовательского элемента управления, представляющего корневой источник привязки. 


### <a name="collections"></a>Коллекции

Если источником данных выступает коллекция, то в пути свойства можно указывать элементы коллекции по их позиции или индексу. Например, "Teams\[0\]. Players ", где литерал"\[\]"заключает" 0 ", который запрашивает первый элемент в коллекции с нулевым индексом.

Чтобы воспользоваться индексатором, модели необходимо реализовать **IList&lt;T&gt;** или **IVector&lt;T&gt;** в типе свойства, которое подлежит индексации. (Обратите внимание, что Иреадонлилист&lt;T&gt; и IVectorView&lt;T&gt; не поддерживают синтаксис индексатора.) Если тип индексированного свойства поддерживает **INotifyCollectionChanged** или **иобсерваблевектор** , а привязка имеет значение OneWay или TwoWay, то она регистрирует и прослушивает уведомления об изменениях этих интерфейсов. Логика отслеживания изменений обновляется с учетом всех изменений коллекции, даже если эти изменения не влияют на конкретное индексированное значение. Это обусловлено тем, что логика ожидания передачи данных общая для всех экземпляров коллекции.

Если источником данных выступает словарь или карта, то в пути свойства можно указывать элементы коллекции по имени строки. Например **&lt;TextBlock Text = "{X:BIND players\[" John Smith "\]}"/&gt;** будет искать элемент в словаре с именем "John Smith". Имя необходимо заключить в кавычки, которые могут быть как двойными, так и одинарными. Для экранирования кавычек в строках можно использовать символ карет (^). Обычно проще использовать альтернативные кавычки, используемые для атрибута XAML. (Обратите внимание, что IReadOnlyDictionary&lt;T&gt; и IMapView&lt;T&gt; не поддерживают синтаксис индексатора.)

Чтобы воспользоваться индексатором строки, модели необходимо реализовать **IDictionary&lt;string, T&gt;** or **IMap&lt;string, T&gt;** в типе свойства, которое подлежит индексации. Если тип индексированного свойства поддерживает **IObservableMap**, а привязка относится к типу OneWay или TwoWay, то она будет принимать и регистрировать уведомления об изменениях на этих интерфейсах. Логика отслеживания изменений обновляется с учетом всех изменений коллекции, даже если эти изменения не влияют на конкретное индексированное значение. Это обусловлено тем, что логика ожидания передачи данных общая для всех экземпляров коллекции.

### <a name="attached-properties"></a>Вложенные свойства

Чтобы выполнить привязку к [присоединенным свойствам](./attached-properties-overview.md), необходимо поставить имя класса и свойства в круглые скобки после точки. Например, **Text="{x:Bind Button22.(Grid.Row)}"** . Если свойство не объявляется в пространстве имен Xaml, нужно установить для него префикс пространства имен xml, которое необходимо сопоставить с пространством имен кода в заголовке документа.

### <a name="casting"></a>Приведение

Скомпилированные привязки строго типизированы и распознают тип каждого этапа в пути. Если возвращаемый тип не содержит элемент, во время компиляции будет возвращена ошибка. Можно сообщать привязке реальный тип объекта, определив приведение. В следующем случае **obj** является свойством объекта типа, но содержит текстовое поле, что позволяет использовать **Text="{x:Bind ((TextBox)obj).Text}"** или **Text="{x:Bind obj.(TextBox.Text)}"** .
Поле **groups3** в **тексте = "{x:bind ((Data: SampleDataGroup) groups3\[0\]). Title} "** — это словарь объектов, поэтому его необходимо привести к типу **данных: SampleDataGroup**. Обратите внимание на то, как используется префикс пространства имен XML **data:** для сопоставления типа объектов с пространством имен кода, не являющимся частью пространства имен XAML по умолчанию.

_Примечание. синтаксис C#приведения в стиле является более гибким, чем синтаксис присоединенного свойства, и является рекомендуемым синтаксисом._

## <a name="functions-in-binding-paths"></a>Функции в путях привязки

Начиная с Windows 10 версии 1607 **{x: Bind}** поддерживает использование функции на конечном этапе шаге пути привязки. Это мощная функция для привязки данных, которая позволяет выполнять несколько сценариев в разметке. Дополнительные сведения см. в разделе [привязки функций](../data-binding/function-bindings.md) .

## <a name="event-binding"></a>Привязка события

Привязка события – это уникальная функция для компилированной привязки. Она позволяет определять обработчик для события с использованием привязки, а не применяется в качестве метода в коде программной части. Например, **Click="{x:Bind rootFrame.GoForward}"** .

В случае работы с событиями метод не должен быть перегружен; он должен соответствовать таким условиям:

- соответствовать подписи события;
- или не иметь параметров;
- или иметь такое же количество параметров типов, которые назначаются из типов параметров событий.

В созданном коде программной части скомпилированная привязка обрабатывает событие и направляет его к методу в модели, анализируя путь выражения привязки при возникновении события. Это означает, что, в отличие от привязок свойства, она не отслеживает изменения в модели.

Дополнительную информацию о синтаксисе строк для пути свойства см. в разделе [Синтаксис пути свойства](property-path-syntax.md), учитывая отличия **{x:Bind}** , описанные в данной статье.

## <a name="properties-that-you-can-set-with-xbind"></a>Свойства, которые можно задать с помощью расширения разметки {x:Bind}

**{x:Bind}** демонстрируется с помощью замещающего синтаксиса *bindingProperties*, поскольку есть несколько свойств, доступных для чтения и записи, которые можно задавать в данном случае использования расширения разметки. Свойства можно задавать в любом порядке с парами *propName*=*value*, разделенными запятыми. Обратите внимание, что разрывы строк недопустимы в выражении привязки. Для некоторых свойств требуются типы, не предусматривающие преобразования; им необходимы их собственные расширения разметки, вложенные в **{x:Bind}** .

Эти свойства работают практически так же, как и свойства класса [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding).

| Свойство | Описание |
|----------|-------------|
| **Путь** | См. раздел [Путь к свойству](#property-path) выше. |
| **Типов** | Указывает объект преобразователя, вызываемый механизмом привязки. Преобразователь можно задать в коде XAML, но только в случае, если вы ссылаетесь на экземпляр объекта, присвоенный в ссылке на [расширение разметки {StaticResource}](staticresource-markup-extension.md) этому объекту в словаре ресурсов. |
| **конвертерлангуаже** | Указывает язык и региональные параметры, используемые преобразователем. (Если задается свойство **ConverterLanguage**, также следует задать свойство **Converter**.) Язык и региональные параметры задаются как стандартный идентификатор. Подробнее: [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Указывает параметр преобразователя, который можно использовать в логике преобразователя. (Если задается **ConverterParameter**, также следует задать свойство **Converter**.) Большая часть преобразователей использует простую логику для получения всей необходимой информации из переданного значения, и им не нужно значение **ConverterParameter**. Параметр **ConverterParameter** используется для более расширенных преобразователей, у которых больше одной логики и которым недостаточно информации, переданной в **ConverterParameter**. Вы также можете написать преобразователь, который использует нестроковые значения, но это используется редко. Подробнее см. в разделе "Примечания" статьи [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter). |
| **фаллбакквалуе** | Задает значение, которое отображается, когда не удается разрешить источник или путь. |
| **Режим** | Указывает режим привязки как одну из этих строк: OneTime, OneWay или TwoWay. Значение по умолчанию — OneTime. Обратите внимание, что это поведение отличается от шаблона по умолчанию для привязки **{Binding}** , которая в большинстве случаев имеет значение OneWay. |
| **таржетнуллвалуе** | Задает значение, которое отображается, когда значение источника разрешается, но оно явно равно **null**. |
| **биндбакк** | Определяет функцию, используемую для обратного направления двусторонней привязки. |
| **UpdateSourceTrigger** | Указывает, когда возвращать изменения в элементе управления модели в привязках TwoWay. Значение по умолчанию для всех свойств, кроме TextBox. Text, — PropertyChanged; TextBox. Text — это потеря фокуса.|

> [!NOTE]
> Если вы преобразуете разметку **{Binding}** в **{x:Bind}** , следует учитывать различия между значениями по умолчанию для свойства **Mode**.
> [**КС:дефаултбиндмоде**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) можно использовать для изменения режима по умолчанию для x:BIND для определенного сегмента дерева разметки. Выбранный режим будет применять любые выражения x:Bind к этому элементу и его дочерним элементам, которые явным образом не задают режим в качестве части привязки. OneTime отличается более высокой производительностью, чем OneWay, поскольку в результате использования OneWay создается больший объем кода для подключения и обнаружения изменений.

## <a name="remarks"></a>Примечания

Поскольку **{x:Bind}** использует сформированный код, во время компиляции ему необходима информация о типе, чтобы воспользоваться всеми доступными преимуществами. Это означает, что, не зная заранее тип, вы не сможете выполнить привязку к свойствам. По этой причине невозможно использовать **{x:Bind}** со свойством **DataContext**, которое относится к типу **Object** и может во время выполнения изменяться.

При использовании **{КС:бинд}** с шаблонами данных необходимо указать тип, к которому выполняется привязка, задав значение **КС:дататипе** , как показано в разделе " [примеры](#examples) ". Вы также можете задавать тип для интерфейса или базового класса, а затем использовать приведения, если понадобится сформулировать полное выражение.

Скомпилированные привязки зависят от создания кода. Если вы используете **{x:Bind}** в словаре ресурсов, тогда словарь ресурсов должен иметь класс кода программной части. Пример кода см. в разделе [Словари ресурсов с привязкой {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind).

Страницы и пользовательские элементы управления, содержащие скомпилированные привязки, имеют в сформированном коде свойство "Bindings". Включены следующие методы:

- **Update()** — обновляет значения всех скомпилированных привязок. Все односторонние или двухсторонние привязки имеют прослушиватели для отслеживания изменений.
- **Initialize()** — Если привязки еще не инициализированы, будет вызван метод Update() для инициализации привязок.
- **StopTracking()** — отключает все прослушиватели, созданные для одно- и двухсторонних привязок. Прослушиватели можно инициализировать повторно с помощью метода Update().

> [!NOTE]
> Начиная с Windows 10 версии 1607, платформа XAML предоставляет встроенный преобразователь Boolean в Visibility. Преобразователь сопоставляет значение **true** значению перечисления **Visible**, а значение **false** значению **Collapsed**, поэтому можно осуществить привязку свойства Visibility к Boolean без создания преобразователя. Обратите внимание, что это не особенность привязки функций, а исключительно привязка свойств. Для использования встроенного преобразователя минимальная версия целевого пакета SDK вашего приложения должна быть 14393 или более поздней. Вы не сможете использовать преобразователь, если ваше приложение предназначено для более ранних версий Windows 10. Дополнительные сведения о целевых версиях см. в статье [Адаптивный к версии код](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Совет**   если необходимо указать одну фигурную скобку для значения, например в [**path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) или [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), перед ней следует использовать обратную косую черту: `\{`. Также можно включить всю строку, содержащую скобки, которые нужно преобразовать, в дополнительный набор кавычек, например: `ConverterParameter='{Mix}'`.

[**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) и **ConverterLanguage** связаны с сценарием преобразования значения или типа из источника привязки в тип или значение, совместимые со свойством цели привязки. Более подробную информацию и примеры см. в разделе "Преобразования данных" статьи [Подробно о привязке данных](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:Bind}** является исключительно расширением разметки и не дает возможности создавать или управлять такими привязками программным способом. Подробнее о расширениях разметки см. в разделе [Обзор языка XAML](xaml-overview.md).

