---
title: "Отслеживание изменений с помощью службы автоматизации Azure | Документация Майкрософт"
description: "Решение для отслеживания изменений помогает выявлять изменения программного обеспечения и служб Windows в среде."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: automation
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: gwallace
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79c5f354c3e63856474e46e2b6928af829604e15
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Отслеживание изменений программного обеспечения в среде с помощью решения для отслеживания изменений

В этой статье приведены сведения о настройке решения "Отслеживание изменений", позволяющего легко обнаруживать изменения в среде. Это решение отслеживает изменения в программном обеспечении и файлах Windows и Linux, разделах реестра Windows, службах Windows и управляющих программах Linux. что, в свою очередь, позволяет точно определять проблемы с работоспособностью.

Данные об изменениях установленного ПО, служб, файлов и реестров Windows и управляющих программ Linux на наблюдаемых серверах отправляются в службу Log Analytics в облаке для обработки. К полученным данным применяется логика и облачная служба записывает данные. С помощью сведений на панели мониторинга «Отслеживание изменений» можно без труда обнаружить изменения, внесенные в инфраструктуру серверов.

## <a name="enable-change-tracking-and-inventory"></a>Включение решения для отслеживания изменений и инвентаризации


Чтобы начать отслеживание изменений, включите решение отслеживания изменений и инвентаризации для учетной записи службы автоматизации.

1. На портале Azure перейдите к учетной записи службы автоматизации.
1. Выберите **Отслеживание изменений** в разделе **Конфигурация**.
2. Выберите имеющуюся рабочую область Log Analytics или **создайте новую** и нажмите кнопку **Включить**.

Таким образом вы включите решение для вашей учетной записи службы автоматизации. Этот процесс может занять до 15 минут. После включения решения появится синий баннер. Чтобы управлять решением, вернитесь к странице **Отслеживание изменений**.

## <a name="configuring-change-tracking-and-inventory"></a>Настройка решения для отслеживания изменений и инвентаризации

Сведения о подключении компьютеров к решению см. в статье [Onboard Update Management, Change Tracking, and Inventory solutions](automation-onboard-solutions-from-automation-account.md) (Подключение решений "Управление обновлениями", "Отслеживание изменений" и "Инвентаризация"). Если вы добавляете новый файл или раздел реестра для отслеживания, они включаются для обоих решений: "Отслеживание изменений" и "Инвентаризация".

### <a name="configure-linux-files-to-track"></a>Настройка отслеживания файлов Linux

Чтобы настроить отслеживание файла на компьютере с Linux, сделайте следующее:

1. В учетной записи службы автоматизации в разделе **Управление конфигурацией** выберите **Отслеживание изменений**. Щелкните **Изменить параметры** (символ шестеренки).
2. На странице **Отслеживание изменений** выберите **Файлы Linux**, нажмите кнопку **+ Добавить**, чтобы добавить новый файл для отслеживания.
3. В окне **Add Linux File for Change Tracking** (Добавление файла Linux для отслеживания изменений) введите сведения о файле или каталоге, для которых необходимо выполнить отслеживание, и щелкните **Сохранить**.

|Свойство  |ОПИСАНИЕ  |
|---------|---------|
|Включено     | Определяет, применяется ли параметр.        |
|Имя элемента     | Понятное имя файла для отслеживания.        |
|Группа     | Имя группы для логического группирования файлов.        |
|Укажите путь     | Путь для проверки наличия файла. Например: /etc/*.conf.       |
|Тип пути     | Тип элемента для отслеживания (возможными значениями могут быть "Файл" и "Каталог").        |
|Рекурсия     | Определяет, используется ли рекурсия при поиске отслеживаемого элемента.        |
|Использовать sudo     | Этот параметр определяет, используется ли sudo для проверки наличия элемента.         |
|Ссылки     | Этот параметр определяет работу символических ссылок при обзоре каталогов.<br> **Игнорировать** — игнорирование символических ссылок без включения ссылочных файлов и каталогов.<br>**Переходить** — переход по символическим ссылкам во время рекурсий и включение ссылочных файлов и каталогов.<br>**Управлять** — переход по символическим ссылкам и разрешение изменения полученного содержимого.     |

> [!NOTE]
> Однако использовать ссылку "Управлять" не рекомендуется. Получение содержимого файлов не поддерживается.

### <a name="configure-windows-files-to-track"></a>Настройка отслеживания файлов Windows

Чтобы настроить отслеживание изменений в файлах на компьютере Windows, сделайте следующее:

1. В учетной записи службы автоматизации в разделе **Управление конфигурацией** выберите **Отслеживание изменений**. Щелкните **Изменить параметры** (символ шестеренки).
2. На странице **Отслеживание изменений** выберите **Файлы Windows**, нажмите кнопку **+ Добавить**, чтобы добавить новый файл для отслеживания.
3. В окне **Add Windows File for Change Tracking** (Добавление файла Windows для отслеживания изменений) введите сведения о файле, для которого необходимо выполнить отслеживание, и щелкните **Сохранить**.

|Свойство  |ОПИСАНИЕ  |
|---------|---------|
|Включено     | Определяет, применяется ли параметр.        |
|Имя элемента     | Понятное имя файла для отслеживания.        |
|Группа     | Имя группы для логического группирования файлов.        |
|Укажите путь     | Путь для проверки наличия файла. Например, c:\temp\myfile.txt.       |

### <a name="configure-windows-registry-keys-to-track"></a>Настройка отслеживания разделов реестра Windows

Чтобы настроить разделы реестра для отслеживания на компьютерах с Windows, сделайте следующее:

1. В учетной записи службы автоматизации в разделе **Управление конфигурацией** выберите **Отслеживание изменений**. Щелкните **Изменить параметры** (символ шестеренки).
2. На странице **Отслеживание изменений** выберите **Реестр Windows** и нажмите кнопку **+ Добавить**, чтобы добавить новый раздел реестра для отслеживания.
3. В окне **Add Windows Registry for Change Tracking** (Добавление реестра Windows для отслеживания изменений) введите сведения о ключе, для которого необходимо выполнить отслеживание, и щелкните **Сохранить**.

|Свойство  |ОПИСАНИЕ  |
|---------|---------|
|Включено     | Определяет, применяется ли параметр.        |
|Имя элемента     | Понятное имя файла для отслеживания.        |
|Группа     | Имя группы для логического группирования файлов.        |
|Раздел реестра Windows   | Путь для проверки наличия файла. Например, HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup.      |

## <a name="limitations"></a>Ограничения

В настоящее время решение для отслеживания изменений не поддерживает следующие элементы.

* Папки (каталоги) для отслеживания файлов Windows
* Рекурсия для отслеживания файлов Windows
* Подстановочные знаки для отслеживания файлов Windows
* Переменные пути
* Сетевые файловые системы
* Содержимое файла

Другие ограничения:

* в текущей версии столбец и значения **Максимальный размер файла** не используются;
* при сборе более 2500 файлов в течение тридцатиминутного цикла производительность решения может снизиться;
* при интенсивном сетевом трафике изменение записей может занять до 6 часов;
* если изменение конфигурации выполняется на отключенном компьютере, он может внести изменения, относящиеся к предыдущей конфигурации.

## <a name="known-issues"></a>Известные проблемы

В настоящее время известны следующие проблемы решения для отслеживания изменений.
* Для компьютеров Windows 10 Creators Update и Windows Server 2016 Core RS3 не собираются исправления.

## <a name="change-tracking-data-collection-details"></a>Сведения о сборе данных отслеживания изменений.

В следующей таблице указана частота сбора данных для типов изменений. Моментальный снимок данных текущего состояния для каждого типа обновляется по меньшей мере каждые 24 часа:

| **Тип изменения** | **Frequency** |
| --- | --- |
| Реестр Windows | 50 минут | 
| Файловый ресурс Windows | 30 минут | 
| Файловый ресурс Linux | 15 минут | 
| Службы Windows | 30 минут | 
| Управляющие программы Linux | 5 мин |
| Программное обеспечение Windows | 30 минут | 
| Программное обеспечение Linux | 5 мин | 

### <a name="registry-key-change-tracking"></a>Отслеживание изменений в разделах реестра

Целью отслеживания изменений в разделах реестра является точное определение точек расширения, в которых может быть активирован сторонний код или вредоносная программа. Ниже представлен список предварительно настроенных разделов реестра. Эти разделы настроены, но не включены для отслеживания. Чтобы отслеживать эти разделы реестра, необходимо включить каждый из них.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются общие записи автозапуска, привязывающиеся непосредственно к проводнику Windows и выполняющиеся с внутрипроцессным файлом Explorer.exe.    |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются сценарии, выполняемые при запуске.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются сценарии, выполняемые при завершении работы.     |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются разделы, загружаемые перед входом пользователя в учетную запись Windows. Этот раздел используется для 32-разрядных программ, выполняющихся на 64-разрядных компьютерах.    |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются изменения параметров приложения.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются общие записи автозапуска, привязывающиеся непосредственно к проводнику Windows и выполняющиеся с внутрипроцессным файлом Explorer.exe.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются общие записи автозапуска, привязывающиеся непосредственно к проводнику Windows и выполняющиеся с внутрипроцессным файлом Explorer.exe.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживается регистрация обработчика дополнительных значков.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживается регистрация обработчика дополнительных значков для 32-битных программ, работающих на 64-разрядных компьютерах.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются новые подключаемые модули вспомогательных объектов браузера для Internet Explorer. Используется для доступа к модели DOM текущей страницы и управления навигацией.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются новые подключаемые модули вспомогательных объектов браузера для Internet Explorer. Используется для получения доступа к модели DOM текущей страницы и управления навигацией для 32-разрядных программ, работающих на 64-разрядных компьютерах.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются новые расширения Internet Explorer, например пользовательские меню инструментов и пользовательские кнопки панели инструментов.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются новые расширения Internet Explorer, например пользовательские меню инструментов и пользовательские кнопки панели инструментов для 32-битных программ, работающих на 64-разрядных компьютерах.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются 32-битные драйверы, связанные с устройствами wavemapper — wave1 и wave2, msacm.imaadpcm, .msadpcm, .msgsm610 и vidc. Ознакомьтесь с разделом о [драйверах] в файле SYSTEM.INI.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживаются 32-битные драйверы, связанные с устройствами wavemapper — wave1 и wave2, msacm.imaadpcm, .msadpcm, .msgsm610 и vidc для 32-битных программ, работающих на 64-разрядных компьютерах. Ознакомьтесь с разделом о [драйверах] в файле SYSTEM.INI.|
> |**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживается список известных или часто используемых системных библиотек DLL. Эта система предотвращает использование ненадежных разрешений каталога приложений за счет удаления версий вредоносной программы типа "Троянский конь" системных библиотек DLL.|
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;Отслеживается список пакетов, получающих уведомления о событиях из Winlogon — интерактивной модели поддержки входа в систему для операционной системы Windows.|

## <a name="use-change-tracking"></a>Использование функции отслеживания изменений

После того как решение включено, вы можете просмотреть сводку изменений для ваших отслеживаемых компьютеров, выбрав **Отслеживание изменений** в разделе **Управление конфигурацией** в вашей учетной записи автоматизации.

Вы можете просмотреть изменения, внесенные на компьютерах, и более подробно рассмотреть каждое событие. Вверху диаграммы находятся раскрывающиеся списки. С их помощью можно отфильтровать диаграмму и подробные сведения по типу изменения и диапазону времени. Кроме того, вы можете щелкнуть на диаграмме и перетащить, чтобы выбрать нужный диапазон времени.

![image of Change Tracking dashboard](./media/automation-change-tracking/change-tracking-dash01.png)

Если щелкнуть изменение или событие, появятся подробные сведения об этом изменении. Как видно из примера, тип запуска службы был изменен с "Ручной" на "Автоматический".

![изображения со сведениями об отслеживании изменений](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Поиск по журналам

Кроме подробных сведений, которые предоставляются на портале, можно выполнить поиск по журналам. Откройте страницу **Отслеживание изменений** и щелкните **Log Analytics**. Откроется страница **Поиск по журналу**.

### <a name="sample-queries"></a>Примеры запросов

Следующая таблица содержит примеры запросов поиска по журналу для получения записей об изменениях, собранных этим решением.

|Запрос  |ОПИСАНИЕ  |
|---------|---------|
|ConfigurationData<br>&#124; where   ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"<br>&#124; where SvcState == "Stopped"<br>&#124; summarize arg_max(TimeGenerated, *) by SoftwareName, Computer         | Показывает самые последние записи инвентаризации для служб Windows, для которых установлено значение "Автоматический" и которые были остановлены.<br>Результаты ограничены самой последней записью для определенного компьютера и имени ПО.      |
|ConfigurationChange<br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Removed"<br>&#124; order by TimeGenerated desc|Показывает записи об изменениях для удаленного программного обеспечения.|

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об использовании решения отслеживания изменений см. в следующем руководстве:

> [!div class="nextstepaction"]
> [Устранение неполадок при изменениях в среде](automation-tutorial-troubleshoot-changes.md)

* Используйте [Поиск по журналам в Log Analytics](../log-analytics/log-analytics-log-searches.md) для просмотра подробных данных по отслеживанию изменений.
