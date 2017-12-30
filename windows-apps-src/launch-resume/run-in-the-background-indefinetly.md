---
author: TylerMSFT
title: "Выполнение в фоновом режиме в течение неограниченного срока"
description: "Используйте возможность extendedExecutionUnconstrained для выполнения фоновой задачи или расширенного сеанса выполнения в фоновом режиме в течение неограниченного срока."
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: "фоновая задача, расширенное выполнение, ресурсы, ограничения"
ms.author: twhitney
ms.date: 10/3/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
localizationpriority: medium
ms.openlocfilehash: 36e1c299ed361d9dbf5de0902279f629e68b4e4f
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2017
---
# <a name="run-in-the-background-indefinitely"></a>Выполнение в фоновом режиме в течение неограниченного срока

Windows ограничивает использование ресурсов приложениями универсальной платформы Windows (UWP), чтобы обеспечить максимальное удобство работы пользователей. Приложения переднего плана получают наибольший объем памяти и время выполнения, а фоновым приложениямвыделяется меньше ресурсов. Таким образом, пользователи защищены от снижения производительности приложений переднего плана и чрезмерного потребления заряда аккумулятора.

Тем не менее, при разработке приложений UWP для личного использования (то есть параллельно загружаемых приложений, которые не будут опубликованы в Microsoft Store) или корпоративных приложений UWP разработчикам может потребоваться все доступные на устройстве ресурсы без регулирования фоновых задач или расширенного выполнения. Приложения UWP для бизнеса и личного использования могут использовать API-интерфейсы в Windows Creators Update (версии 1703) для отключения регулирования. Имейте в виду, что есть в приложении используются эти API-интерфейсы, его невозможно разместить в Microsoft Store.

## <a name="run-while-minimized"></a>Выполнение при сворачивании

Приложения UWP переходят в приостановленное состояние, если они не выполняются на переднем плане. На рабочем столе это происходит, когда пользователь сворачивает приложение. Приложения используют расширенный сеанс выполнения, чтобы продолжить выполнение в свернутом состоянии. Подробное описание API-интерфейсов расширенного выполнения, которые принимаются в Microsoft Store, см. в разделе [Задержка приостановки приложения с помощью расширенного сеанса выполнения](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution).

При разработке приложения, не предназначенного для отправки в Microsoft Store, можно использовать [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) с ограниченной возможностью `extendedExecutionUnconstrained`, чтобы приложение продолжало работу в свернутом виде независимо от уровня заряда устройства.  

Возможность `extendedExecutionUnconstrained` добавляется в манифест приложения как ограниченная возможность. Дополнительные сведения об ограниченных возможностях см. в разделе [Объявления возможностей приложения](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

_Package.appxmanifest_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

При использовании возможности `extendedExecutionUnconstrained`, [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) и [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) используются вместо [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) и [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). Применяется та же схема создания сеанса, установки членов и асинхронного запроса расширения: 

```cs
var newSession = new ExtendedExecutionForegroundSession();  
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;  
newSession.Description = "Long Running Processing";  
newSession.Revoked += SessionRevoked;  
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();  
switch (result)  
{  
    case ExtendedExecutionResult.Allowed:  
        DoLongRunningWork();  
        break;  

    default:  
    case ExtendedExecutionResult.Denied:  
        DoShortRunningWork();  
        break;  
}
```

Можно запросить этот расширенный сеанс выполнения, как только приложение перейдет на передний план. Неограниченные расширенные сеансы выполнения не ограничиваются квотами энергопотребления или функцией экономии заряда операционной системы. Пока существует ссылка на объект сеанса, приложение будет оставаться в состоянии выполнения и не перейдет в приостановленное состояние. Если пользователь закроет приложение, сеанс будет отозван.

Регистрация события **Revoked** позволит приложению выполнить все необходимые действия по очистке. В приостановленном состоянии можно создать расширенный сеанс выполнения с использованием [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason), чтобы сохранить данные пользователя, прежде чем приложение будет завершено и удалено из памяти.

## <a name="run-background-tasks-indefinitely"></a>Выполнение фоновых задач в течение неограниченного срока

В универсальной платформе Windows фоновые задачи— это процессы, выполняемые в фоновом режиме без какого-либо пользовательского интерфейса. Как правило, фоновые задачи выполняются не более 25 секунд, а затем отменяются. Некоторые задачи, выполняемые в течение длительного срока, выполняют проверку на наличие простаивающих или использующих память фоновых задач. В Windows Creators Update (версии 1703) была реализована ограниченная возможность [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations), призванная убрать эти ограничения. Возможность **extendedBackgroundTaskTime** добавляется в файл манифеста приложения как ограниченная возможность:

_Package.appxmanifest_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

Эта возможность снимает ограничения времени выполнения и удаляет средство наблюдения за простаивающими задачами. После запуска с помощью триггера или вызова службы приложения и создания отсрочки в [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance), предоставленном методом **Run**, фоновая задача может выполняться в течение неограниченного срока. Если предложению задано значение **Управляется Windows**, возможно, к нему до сих пор применяется квота энергопотребления, а его фоновые задачи не будут активироваться, пока действует экономия заряда.Это можно изменить с помощью параметров операционной системы. Дополнительные сведения см. в разделе [Оптимизация фоновых процессов](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

Универсальная платформа Windows отслеживает выполнение фоновых задач, чтобы обеспечить экономию уровня заряда и высокую производительность задач на переднем плане. Тем не менее, с помощью расширенного выполнения и возможности **extendedBackgroundTaskTime** можно создавать приложения для личного пользования и корпоративные бизнес-приложения с неограниченным сроком выполнения независимо от объема доступных ресурсов приложения.

Имейте в виду, что возможности **extendedExecutionUnconstrained** и **extendedBackgroundTaskTime** могут переопределить политику по умолчанию для приложений UWP и привести к быстрому снижению уровня заряда. Прежде чем использовать эти возможности, убедитесь, что стандартные политики расширенного выполнения и времени выполнения фоновых задач не удовлетворяют ваши потребности, а также проведите тестирование в условиях с ограниченным уровнем заряда, чтобы оценить влияние приложения на устройство.

## <a name="see-also"></a>См. также:

[Удаление ограничений ресурсов фоновых задач](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)