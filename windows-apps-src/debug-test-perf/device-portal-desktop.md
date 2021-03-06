---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Портал устройств для компьютеров с Windows
description: Узнайте, как с помощью портала устройств Windows получить доступ к средствам диагностики и автоматизации на компьютере с Windows.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, портал устройств
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681975"
---
# <a name="device-portal-for-windows-desktop"></a>Портал устройств для компьютеров с Windows

С помощью портала устройств Windows можно отображать диагностические сведения и взаимодействовать с ПК по протоколу HTTP из окна браузера. Используя портал устройств, можно выполнять следующие действия:
- Просматривать список запущенных процессов и управлять им
- Устанавливать, удалять, запускать и завершать работу приложений
- Изменять профили Wi-Fi и просматривать сведения об уровне сигнала (ipconfig)
- Просматривать графики в режиме реального времени со сведениями об использовании ЦП, памяти, устройств ввода-вывода и GPU
- Собирать дампы процессов
- Собирать трассировки событий Windows 
- Работать с изолированным хранилищем неопубликованных приложений

## <a name="set-up-device-portal-on-windows-desktop"></a>Настройка портала устройств на компьютере под управлением Windows

### <a name="turn-on-developer-mode"></a>Включите режим разработчика

Начиная с Windows 10 версии 1607, некоторые новые функции для компьютеров доступны только при включенном режима разработчика. Сведения о том, как включить режим разработчика, см. в разделе[Подготовка устройства для разработки](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> Иногда из-за проблем сети или совместимости режим разработчика может установиться на устройстве неправильно. Инструкции по устранению этих проблем см. в разделе [Включение устройства для разработки](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package).

### <a name="turn-on-device-portal"></a>Включите портал устройств

Вы можете включить портал устройств в разделе **Для разработчиков** раздела **Параметры**. При включении портала устройств вы должны также создать соответствующие имя пользователя и пароль. Не используйте учетную запись Майкрософт или другие учетные данные Windows. 

![Раздел "Портал устройств" в приложении "Параметры"](images/device-portal/device-portal-desk-settings.png) 

После включения портала устройств в нижней части раздела отобразятся соответствующие веб-ссылки. Запомните номер порта, расположенный в конце указанных URL-адресов: этот номер порта создается случайным образом при включении портала устройств, однако он должен оставаться одинаковым при каждой перезагрузке компьютера. 

Эти ссылки предоставляют возможность подключиться к порталу устройств двумя способами: через локальный узел и по локальной сети (включая VPN).

### <a name="connect-to-device-portal"></a>Подключение к порталу устройств

Чтобы подключиться через локальный узел, откройте окно браузера и введите адрес, указанный здесь для соответствующего типа подключения, который вы используете.

* Localhost: `http://127.0.0.1:<PORT>` или `http://localhost:<PORT>`
* Локальная сеть: `https://<IP address of the desktop>:<PORT>`

Для проверки подлинности и безопасного обмена данными необходима поддержка протокола HTTPS.

Если вы используете портал устройств в защищенной среде, например, лаборатории тестирования, в которой вы доверяете всем пользователям в локальной сети, на устройстве нет личных сведений и у вас имеются особые требования, вы можете отключить проверку подлинности. В этом случае шифрование связи будет отключено, и любой пользователь сможет подключиться к вашему ПК и управлять им, зная его IP-адрес.

## <a name="device-portal-content-on-windows-desktop"></a>Подключение портала устройств на компьютере с Windows

Портал устройств на ПК с Windows предоставляет стандартный набор страниц. Подробные описания см. в разделе [Обзор портала устройств Windows](device-portal.md).

- Диспетчер приложений
- Проводник
- Запущенные процессы
- События
- Debug
- Трассировка событий Windows (ETW)
- Трассировка производительности
- "диспетчер устройств"
- Возможности работы с сетями в
- Данные о сбоях
- Возможности
- Смешанная реальность
- Отладчик потоковой установки
- Расположение
- Рабочая зона

## <a name="more-device-portal-options"></a>Дополнительные параметры портала устройств

### <a name="registry-based-configuration-for-device-portal"></a>Настройка на основе реестра для портала устройств

Если вы хотите выбрать номера портов для портала устройств (например, 80 и 443), настройте следующие разделы реестра:

- В разделе `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: обязательный параметр DWORD. Задайте этому параметру значение 0, чтобы сохранить номера портов, которые вы выбрали.
    - `HttpPort`: обязательный параметр DWORD. Содержит номер порта, который портал устройств будет прослушивать на предмет активных подключений HTTP.    
    - `HttpsPort`: обязательный параметр DWORD. Содержит номер порта, который портал устройств будет прослушивать на предмет активных подключений HTTPS.
    
В том же пути раздела реестра можно также отключить требование к проведению проверки подлинности:
- `UseDefaultAuthorizer` - `0` отключена, `1` включен.  
    - Этот параметр управляет одновременно требованием к проведению обычной проверки подлинности для каждого подключения и перенаправлением из HTTP в HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Параметры командной строки для портала устройств
Из командной строки с правами администратора можно включить и настроить части портала устройств. Чтобы просмотреть последний набор команд, поддерживаемых в сборке, можно запустить `webmanagement /?`

- `sc start webmanagement` или `sc stop webmanagement` 
    - Включение или отключение службы. Для этого режим разработчика по-прежнему должен быть включен. 
- `-Credentials <username> <password>` 
    - Задайте имя пользователя и пароль для портала устройств. Имя пользователя должно соответствовать стандартам базовой проверки подлинности, поэтому не может содержать двоеточие (:) и должны быть построены из стандартных символов ASCII, например [a-zA-Z0-9], так как браузеры не анализируют полный набор символов стандартным способом.  
- `-DeleteSSL` 
    - В результате это происходит сброс кэша SSL-сертификата, используемого подключениями по протоколу HTTPS. При возникновении ошибок подключения по протоколу TLS, которые нельзя обойти, (в отличие от ожидаемого предупреждения сертификате), это, возможно, поможет устранить проблему. 
- `-SetCert <pfxPath> <pfxPassword>`
    - См. раздел [Подготовка портала устройств с использованием пользовательского сертификата SSL](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl).  
    - Это позволяет установить сертификат SSL и закрыть страницу предупреждения SSL, которая обычно появляется на портале устройств. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Запустите автономную версию портала устройств определенной конфигурации и видимыми сообщениями отладки. Это особенно полезно для создания [упакованных подключаемый модулей](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Сведения о том, как выполнять запуск в качестве системы для полной проверки упакованного подключаемого модуля см. [в статье из журнала MSDN Magazine](https://msdn.microsoft.com/magazine/mt826332.aspx).

## <a name="common-errors-and-issues"></a>Распространенные ошибки и проблемы

Ниже приведены некоторые распространенные ошибки, которые могут возникнуть при настройке портала устройств.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>Виндовсупдатесеарч возвращает недопустимое число обновлений (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Эта ошибка может возникнуть при попытке установить пакеты разработчика в предварительной сборке Windows 10. Эти пакеты по требованию (FoD) размещаются на Центр обновления Windows и их загрузка в предварительных сборках требует участия в полете. Если установка не включена в рейс для правильной комбинации сборки и кольца, полезные данные не будут скачаны. Дважды проверьте следующее:

1. Выберите **параметры > обновить & безопасность > программе предварительной оценки Windows** и убедитесь, что раздел **учетная запись Windows Insider** содержит правильные сведения об учетной записи. Если этот раздел не отображается, выберите **связать учетную запись Windows Insider Preview**, добавьте учетную запись электронной почты и убедитесь, что она отображается под заголовком **учетной записи Windows Insider** (для фактической связи с добавленной учетной записью может потребоваться установить флажок **связать учетную запись Windows для участников предварительной оценки** ).
 
2. В **каком типе содержимого вы хотите получить?** убедитесь, что выбрана **активная разработка Windows** .
 
3. В **каком темпе вы хотите получить новые сборки?** убедитесь, что выбрана **Быстрая Предварительная загрузка Windows** .
 
4. Теперь вы можете установить Фодс. Если вы подтвердили, что ваша программа предварительной оценки Windows быстро и по-прежнему не можете установить Фодс, оставьте отзыв и вложите файлы журнала в **к:\виндовс\логс\кбс**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>SC StartService: сбой OpenService 1060: указанная служба не существует в качестве установленной службы

Эта ошибка может возникнуть, если пакеты разработчика не установлены. Без пакетов разработчиков служба веб-управления отсутствует. Попробуйте установить пакеты разработчика еще раз.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>Невозможно начать скачивание CBS, так как система находится в сети с лимитным тарифным планом (CBS_E_METERED_NETWORK)

Эта ошибка может возникать, если вы используете лимитное подключение к Интернету. Вы не сможете скачать пакеты разработчиков в отношении лимитного подключения.

## <a name="see-also"></a>См. также статью

* [Общие сведения о портале устройств Windows](device-portal.md)
* [Справочник по API для ядра портала устройств](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
