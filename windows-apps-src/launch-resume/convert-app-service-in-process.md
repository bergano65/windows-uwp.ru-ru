---
author: TylerMSFT
title: "Преобразование службы приложения для ее запуска в одном процессе с основным приложением"
description: "Преобразуйте код службы приложений, выполняемый в отдельном фоновом процессе, в код, выполняемый в одном процессе с поставщиком службы приложений."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.openlocfilehash: 015a37c8d85174e6494690a67b4dc23b1cce5712
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>Преобразование службы приложений для ее запуска в одном процессе с ведущим приложением

[AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) позволяет другому приложению выводить ваше приложение из фонового режима и устанавливать прямую линию связи с ним.

С появлением служб приложений, работающих в процессе, два запущенных на переднем плане приложения могут устанавливать прямую линию обмена данными через подключение служб приложения. Службы приложения теперь могут выполняется в том же процессе, что и приложение переднего плана, что упрощает обмен данными между приложениями и исключает необходимость выделения кода службы в отдельный проект.

Для перехода от модели службы приложения, выполняемой вне процесса, к модели с выполнением внутри процесса необходимо сделать два изменения. Первое изменение вносится в манифест.

> ```xml
>  <uap:Extension Category="windows.appService">
>          <uap:AppService Name="InProcessAppService" />
>  </uap:Extension>
> ```

Удалите атрибут `EntryPoint`. Теперь в качестве метода обратного вызова при вызове службы приложения будет использоваться обратный вызов [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Второе изменение заключается в переносе логики службы из отдельного проекта фоновой задачи в методы, которые можно вызывать из метода **OnBackgroundActivated()**.

Теперь приложение может напрямую выполнять службу приложения.  Пример.

> ``` cs
> private AppServiceConnection appServiceconnection;
> private BackgroundTaskDeferral appServiceDeferral;
> protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
> {
>     base.OnBackgroundActivated(args);
>     IBackgroundTaskInstance taskInstance = args.TaskInstance;
>     AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
>     appServiceDeferral = taskInstance.GetDeferral();
>     taskInstance.Canceled += OnAppServicesCanceled;
>     appServiceConnection = appService.AppServiceConnection;
>     appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
>     appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
> }
>
> private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
> {
>     AppServiceDeferral messageDeferral = args.GetDeferral();
>     ValueSet message = args.Request.Message;
>     string text = message["Request"] as string;
>              
>     if ("Value" == text)
>     {
>         ValueSet returnMessage = new ValueSet();
>         returnMessage.Add("Response", "True");
>         await args.Request.SendResponseAsync(returnMessage);
>     }
>     messageDeferral.Complete();
> }
>
> private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
> {
>     appServiceDeferral.Complete();
> }
>
> private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
> {
>     appServiceDeferral.Complete();
> }
> ```

В приведенном выше коде метод `OnBackgroundActivated` обрабатывает активацию службы приложения. Регистрируются все события, необходимые для обмена данными через [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx), и сохраняется объект откладывания задачи, чтобы ее можно было пометить как выполненную после завершения обмена данными между приложениями.

Когда приложение получает запрос, оно считывает предоставленный [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) для проверки наличия строк `Key` и `Value`. Если они существуют, служба приложения возвращает пару строковых значений `Response` и `True` в приложение на другой стороне **AppServiceConnection**.

Подробнее о подключении к другим приложением и обмене данными с ними см. в разделе [Создание и использование службы приложения](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).