---
title: Трехмерная печать из приложения
description: Узнайте, как добавить функцию трехмерной печати в универсальное приложение для Windows. Эта статья содержит сведения о том, как вызвать диалоговое окно трехмерной печати, убедившись в том, что трехмерная модель пригодна для печати и представлена в нужном формате.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 3dprinting трехмерной печати
ms.localizationpriority: medium
ms.openlocfilehash: e2ed99720afdccef297d46853d4a2445b497195e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321698"
---
# <a name="3d-printing-from-your-app"></a>Трехмерная печать из приложения

**Важные API**

-   [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D)

Узнайте, как добавить функцию трехмерной печати в универсальное приложение для Windows. В этом разделе рассматривается, как загрузить трехмерные геометрические данные в приложение и открыть диалоговое окно трехмерной печати после того, как вы убедитесь, что ваша трехмерная модель пригодна для печати и выполнена нужном формате. Работающий пример этих процедур см. в статье [Пример трехмерной печати UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> В примерах кода в этом руководстве обработка ошибок и отчетность значительно упрощены.

## <a name="setup"></a>Установка


Добавьте пространство имен [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D) в класс приложения, где требуется реализовать возможности трехмерной печати.

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

В этом руководстве будут использоваться следующие дополнительные пространства имен.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Затем добавьте в класс некоторые полезные поля-члены. Объявите объект [**Print3DTask**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTask), представляющий задачу печати, которую требуется передать драйверу печати. Объявите объект [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) для хранения исходного файла трехмерных данных, которые будут загружаться в приложение. Объявите объект [**Printing3D3MFPackage**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage), представляющий готовую для печати трехмерную модель со всеми необходимыми метаданными.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Создание простого пользовательского интерфейса

В этом примере используется три пользовательских элемента управления: кнопка загрузки (для переноса файла в память программы), кнопка исправления (для изменения файла согласно необходимости) и кнопка печати (для запуска задания печати). Следующий код позволяет создать эти кнопки (с соответствующими обработчиками события нажатия) в XAML-файле вашего CS-класса.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Добавьте поле **TextBlock** для обратной связи от пользовательского интерфейса.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Получение трехмерных данных


Методы, с помощью которых ваше приложение получает трехмерные геометрические данные, будут варьироваться. Приложение может извлечь трехмерные данные из сканирования, скачать данные модели с веб-ресурса или создать трехмерную сетку программным способом с помощью математических формул или пользовательского ввода. Для простоты в этом примере мы загрузим файл с трехмерными данными (любого из нескольких распространенных типов файлов) в память программы памяти устройства. [Библиотека моделей 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) предоставляет разнообразные модели, которые легко можно загрузить на устройство.

В методе `OnLoadClick` используйте класс [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) для загрузки отдельного файла в память приложения.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Использование конструктора 3D Builder для преобразования в трехмерный производственный формат (.3mf)

На этом этапе можно загрузить файл данных 3D в памяти приложения. Однако геометрические данные 3D могут быть представлены в самых разных форматах, не все из которых подходят для трехмерной печати. В Windows 10 используется тип файла в формате создания 3D (.3mf) для всех задач трехмерной печати.

> [!NOTE]  
> Тип файлов 3MF поддерживает в том числе функциональные возможности, не описанные в этом руководстве. Подробнее о типе файлов 3MF и функциональных возможностях, предоставляемых производителям и потребителям трехмерных продуктов, см. в [спецификации 3MF](https://3mf.io/what-is-3mf/3mf-specification/). Чтобы узнать, как использовать эти возможности с помощью API-интерфейсов Windows 10, ознакомьтесь с учебником [Создание пакета 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

Приложение [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) может открывать файлы наиболее распространенных 3D-форматов и сохранять их в виде 3MF-файлов. В этом примере, где тип файла может варьироваться, простейшим решением будет открыть приложение 3D Builder и предложить пользователю сохранить импортированные данные в виде 3MF-файла, а затем заново загрузить их.

> [!NOTE]  
> Помимо преобразования форматов файлов конструктор 3D Builder предоставляет простые инструменты для редактирования моделей, добавления данных о цвете и выполнения других операций печати, поэтому конструктор имеет смысл интегрировать в любые приложения, где используется трехмерная печать.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Восстановление данных модели для трехмерной печати

Не все данные трехмерной модели можно напечатать, даже в 3MF-файле. Чтобы принтер мог правильно определить, какое пространство заполнять, а какое — оставлять пустым, печатаемые модели должны представлять собой цельную бесшовную структуру, иметь обращенные наружу нормали к поверхности и разностороннюю геометрию. С этим могут возникать самые разные проблемы, и обнаружить их в сложных формах весьма затруднительно. К счастью, современные программные решения отлично справляются с задачей преобразования необработанных геометрических данных в доступные для печати трехмерные фигуры. Этот процесс называется *восстановлением* модели и выполняется методом `OnFixClick`.

Необходимо преобразовать файл трехмерных данных, чтобы реализовать интерфейс [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream), который затем можно использовать для создания объекта [**Printing3DModel**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3DModel).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

Все ошибки в объекте **Printing3DModel** теперь устранены, он доступен для печати. Воспользуйтесь методом [**SaveModelToPackageAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync), чтобы назначить модель объекту **Printing3D3MFPackage**, объявленному при создании класса.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Выполнение задачи печати: создание обработчика TaskRequested


В дальнейшем, когда пользователю отображается диалоговое окно трехмерной печати и пользователь решает начать печать, приложению потребуется передать нужные параметры в конвейер трехмерной печати. API трехмерной печати вызывает событие **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** . Необходимо написать метод для правильной обработки этого события. Как всегда метод обработчика должен иметь тот же тип, как его событие: **TaskRequested** событие имеет параметры [ **Print3DManager** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (ссылка на свой объект отправителя) и [  **Print3DTaskRequestedEventArgs** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) объект, который содержит большинство необходимую информацию.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

Основное назначение этого метода — использовать параметр *args* для отправки пакета **Printing3D3MFPackage** по конвейеру. **Print3DTaskRequestedEventArgs** тип имеет одно свойство: [**Запросить**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Оно имеет тип [**Print3DTaskRequest**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) и представляет один запрос задания печати. Его метод [**CreateTask**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) позволяет программе отправлять корректную информацию для вашего задания печати и возвращает ссылку на объект **Print3DTask**, который был отправлен по конвейеру трехмерной печати.

**CreateTask** имеет следующие входные параметры: string для имени задания печати, string для идентификатора используемого принтера и делегат [**Print3DTaskSourceRequestedHandler**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler). Делегат вызывается автоматически при создании события **3DTaskSourceRequested** (за это отвечает API). Важно отметить, что это делегат вызывается, когда инициируется задание печати, и отвечает за предоставление правильного пакета трехмерной печати.

**Print3DTaskSourceRequestedHandler** принимает один параметр — объект [**Print3DTaskSourceRequestedArgs**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs), предоставляющий данные для отправки. Один открытый метод этого класса — [**SetSource**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource) — принимает печатаемый пакет. Реализуйте делегат **Print3DTaskSourceRequestedHandler** следующим образом.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Затем вызовите **CreateTask**, используя только что определенный делегат `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

Возвращенный объект **Print3DTask** назначается объявленной в начале переменной класса. Теперь при необходимости можно использовать эту ссылку для обработки определенных событий, создаваемых этой задачей.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Необходимо реализовать методы `Task_Submitting` и `Task_Completed`, если требуется зарегистрировать их для этих событий.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Выполнение задачи печати: открытие диалогового окна трехмерной печати


Необходимо финальный фрагмент кода, который открывает диалоговое окно трехмерной печати. Как и в обычном диалоговом окне печати, в диалоговом окне трехмерной печати находится ряд параметров, которые задаются в последний момент. Здесь же можно выбрать принтер для печати (подключенный через USB или по сети).

Зарегистрируйте метод `MyTaskRequested` с событием **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

После регистрации обработчика событий **TaskRequested** можно вызвать метод [**ShowPrintUIAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync), который выведет диалоговое окно трехмерной печати в окне текущего приложения.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Рекомендуется отменять регистрацию обработчиков событий, как только в приложении восстанавливается контроль над процессом.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>См. также

[Создание пакета 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[Трехмерной печати образец универсальной платформы Windows](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
