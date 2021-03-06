---
title: "Использование средства миграции Power BI Embedded | Документация Майкрософт"
description: "Средство миграции Power BI Embedded можно использовать для копирования отчетов из коллекций рабочей области Power BI в Power BI Embedded."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 0b7b5089045daf6dd88fcd84e316b2bd44f8c927
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-power-bi-embedded-migration-tool"></a>Использование средства миграции Power BI Embedded

Средство миграции Power BI Embedded можно использовать для копирования отчетов из коллекций рабочей области Power BI в Power BI Embedded.

Перенос содержимого из коллекций рабочих областей в службу Power BI может выполняться параллельно с работой вашего текущего решения и не вызывает простоев.

## <a name="limitations"></a>Ограничения

* Отправленные наборы данных будет невозможно скачать, и их потребуется создать повторно с помощью интерфейсов REST API Power BI для службы Power BI.
* Скачать PBIX-файлы, импортированные до 26 ноября 2016 года, будет невозможно.

## <a name="download"></a>Загрузка

Вы можете скачать пример средства миграции с сайта [GitHub](https://github.com/Microsoft/powerbi-migration-sample). Можно скачать ZIP-файл репозитория или клонировать его на локальный компьютер. После скачивания файл *powerbi миграции sample.sln* можно открыть в Visual Studio, чтобы выполнить сборку и запуск средства миграции.

## <a name="migration-plans"></a>Планы миграции

План миграции — это просто метаданные, позволяющие систематизировать содержимое в коллекциях рабочих областей Power BI и указать способ их публикации в Power BI Embedded.

### <a name="start-with-a-new-migration-plan"></a>Начало работы с новым планом миграции

План миграции — это метаданные элементов, доступных в коллекциях рабочих областей Power BI, которые необходимо перенести в Power BI Embedded. План миграции хранится в виде XML-файла.

Процедуру переноса потребуется начать с создания плана миграции. Чтобы создать план миграции, выполните следующие действия.

1. Выберите **Файл** > **New Migration Plan** (Создать план миграции).

    ![Новый план миграции](media/migrate-tool/migrate-tool-plan.png)

2. В диалоговом окне **Select Power BI Workspace Collections Resource Group** (Выбор группы ресурсов коллекций рабочих областей Power BI) следует щелкнуть раскрывающийся список **Среда** и выбрать рабочую среду.

3. Вам будет предложено войти в систему. Для этого следует использовать имя для входа подписки Azure.

    > [!IMPORTANT]
    > Это **не** учетная запись организации Office 365, с помощью которой вы вошли в Power BI.

4. Выберите подписку Azure, в которой хранится ресурс коллекций рабочей области Power BI.

    ![Выбор подписки Azure](media/migrate-tool/migrate-tool-select-resource-group.png)

5. Под списком подписок выберите **группу ресурсов**, содержащую коллекции рабочих областей, и щелкните **Выбрать**.

    ![Выбор группы ресурсов](media/migrate-tool/migrate-tool-select-resource-group2.png)

6. Щелкните **Анализ**. Отобразится перечень элементов в подписке Azure, чтобы вы могли приступить к созданию плана.

    ![Нажатие кнопки "Анализ"](media/migrate-tool/migrate-tool-analyze-group.png)

    > [!NOTE]
    > Процесс анализа может занять несколько минут. Это зависит от количества коллекций рабочих областей и объема содержимого в каждой из них.

7. По завершении **анализа** вам будет предложено сохранить план миграции.

На этом этапе план миграции подключен к подписке Azure. Ниже описывается последовательность действий с планом миграции. Сюда входят анализ и планирование переноса, скачивание, создание групп и передача.

### <a name="save-your-migration-plan"></a>Сохранение плана миграции

Вы можете сохранить план миграции для последующего использования. При этом будет создан XML-файл, содержащий все данные плана миграции.

Чтобы сохранить план миграции, выполните следующие действия.

1. Выберите **Файл** > **Save Migration Plan** (Сохранить план миграции).

    ![Пункт меню "Save Migration Plan" (Сохранить план миграции)](media/migrate-tool/migrate-tool-save-plan.png)

2. Присвойте файлу имя или используйте автоматически созданное имя файла. Нажмите кнопку **Сохранить**.

### <a name="open-an-existing-migration-plan"></a>Открытие имеющегося плана миграции

Вы можете открыть сохраненный план миграции, чтобы продолжить перенос.

Чтобы открыть имеющийся план миграции, выполните следующие действия.

1. Выберите **Файл** > **Open Existing Migration Plan** (Открыть имеющийся план миграции).

    ![Пункт меню "Open Existing Migration Plan" (Открыть имеющийся план миграции)](media/migrate-tool/migrate-tool-open-plan.png)

2. Выберите файл для переноса и щелкните **Открыть**.

## <a name="step-1-analyze-and-plan-migration"></a>Шаг 1. Анализ и планирование переноса

На вкладке **Analyze & Plan Migration** (Анализ и планирование переноса) можно получить представление о том, что в настоящее время находится в группе ресурсов вашей подписки Azure.

![Вкладка "Analyze & Plan Migration" (Анализ и планирование переноса)](media/migrate-tool/migrate-tool-step1.png)

Для примера рассмотрим группу ресурсов *SampleResourceGroup*.

### <a name="paas-topology"></a>Топология PaaS

Это перечень ваших рабочих областей, доступных по пути *Группа ресурсов > Коллекций рабочих областей > Рабочие области*. Для группы ресурсов группы и коллекций рабочих областей будут отображаться понятные имена. Для рабочих областей будут отображаться идентификаторы GUID.

Элементы в списке будут также отображены разным цветом и с числами в формате (#/##). Эти числа означают количество отчетов, которые могут быть скачаны.

Черный цвет означает, что могут быть скачаны все отчеты. Красный цвет означает, что некоторые отчеты не могут быть скачаны. Число слева будет означать общее количество отчетов, которые могут быть скачаны. Число справа означает общее количество отчетов внутри группы.

Можно выбрать элемент в топологии PaaS, чтобы отобразить отчеты в разделе "Отчеты".

### <a name="reports"></a>Отчеты

В разделе "Отчеты" приведен список доступных отчетов и указано, можно ли их скачать.

![Список отчетов в коллекциях рабочих областей Power BI](media/migrate-tool/migrate-tool-analyze-reports.png)

### <a name="target-structure"></a>Целевая структура

В разделе **Target structure** (Целевая структура) можно указать средству миграции назначение скачивания и способ передачи элементов.

#### <a name="download-plan"></a>План скачивания

Путь будет создан автоматически. При желании его можно изменить. Чтобы изменить путь, необходимо щелкнуть **Update paths** (Обновить пути).

**При этом не выполняется скачивание.** Это действие позволяет только указать структуру, в которую будут скачаны отчеты.

#### <a name="upload-plan"></a>План передачи

Здесь можно указать префикс для рабочих областей приложения, которые будут создаваться в службе Power BI. После префикса будет указан идентификатор GUID рабочей области, с которым она существовала в Azure.

![Указание префикса имени группы](media/migrate-tool/migrate-tool-upload-plan.png)

**Это действие не создает группы в службе Power BI.** Оно только определяет структуру имен для групп.

Если изменить префикс, потребуется выбрать **Generate Upload Plan** (Создать план передачи).

При необходимости можно щелкнуть группу правой кнопкой мыши и выбрать пункт для переименования группы в соответствии с планом передачи.

![Пункт "Переименовать" в контекстном меню](media/migrate-tool/migrate-tool-upload-report-rename-item.png)

> [!NOTE]
> Имя *группы* не должно содержать пробелы или недопустимые знаки.

## <a name="step-2-download"></a>Шаг 2. Скачивание

На вкладке **Скачивание** отображается список отчетов и связанные метаданные. Там же отображается состояние экспорта вместе с предыдущим состоянием экспорта.

![Вкладка "Скачивание"](media/migrate-tool/migrate-tool-download-tab.png)

Это можно сделать двумя способами.

* Выберите определенные отчеты и щелкните **Скачать выбранные**.
* Выберите **Скачать все**.

![Кнопка "Скачать выбранные"](media/migrate-tool/migrate-tool-download-options.png)

Если скачивание выполнено успешно, вы увидите состояние *Готово*. Это значит, что PBIX-файл существует.

После завершения скачивания выберите вкладку **Create Groups** (Создание групп).

## <a name="step-3-create-groups"></a>Шаг 3. Создание групп

После скачивания доступных отчетов можно перейти на вкладку **Create Groups** (Создание групп). На этой вкладке можно создать рабочие области приложения в службе Power BI на основе созданного вами плана миграции. Рабочая область приложения будет создана с именем, которое вы указали на вкладке **Отправка** на странице **Analyze & Plan Migration** (Анализ и планирование переноса).

![Вкладка "Create Groups" (Создание групп)](media/migrate-tool/migrate-tool-create-groups.png)

Чтобы создать рабочие области приложения, можно выбрать **Create Selected Groups** (Создать выбранные группы) или **Create All Missing Groups** (Создать все отсутствующие группы).

При выборе любого из этих параметров отобразится запрос на вход в систему. *Необходимо использовать учетные данные для службы Power BI, в которой требуется создать рабочие области приложения.*

![Экран входа в Power BI](media/migrate-tool/migrate-tool-create-group-sign-in.png)

В службе Power BI будет создана рабочая область приложения. При этом отчеты не будут переданы в нее.

Можно проверить, создана ли рабочая область приложения, войдя в Power BI и убедившись в ее наличии. Вы обратите внимание на то, что в рабочей области ничего нет.

![Рабочая область приложения в службе Power BI](media/migrate-tool/migrate-tool-app-workspace.png)

После создания рабочей области можно перейти на вкладку **Отправка**.

## <a name="step-4-upload"></a>Шаг 4. Передача

На вкладке **Отправка** можно будет передать отчеты в службу Power BI. Вы увидите список отчетов, которые мы скачали на вкладке "Скачивание", а также имя целевой группы в соответствии с планом миграции.

![Вкладка "Отправка"](media/migrate-tool/migrate-tool-upload-tab.png)

Можно передать только выбранные отчеты или все отчеты. Можно также сбросить состояние передачи, чтобы повторно передать элементы.

Также имеется возможность выбрать действие в случае, если отчет с таким же именем уже существует. Можно выбрать действие **Прервать**, **Пропустить** и **Перезаписать**.

![Раскрывающийся список действий в случае, если отчет уже существует](media/migrate-tool/migrate-tool-upload-report-same-name.png)

![Результаты передачи выбранных отчетов](media/migrate-tool/migrate-tool-upload-selected.png)

### <a name="duplicate-report-names"></a>Повторяющиеся имена отчетов

Если у вас есть отчет с таким же именем, но вы знаете, что это другой отчет, необходимо изменить параметр **TargetName** отчета. Имя можно изменить, вручную изменив код XML плана миграции.

Потребуется закрыть средство миграции, чтобы внести изменение, а затем снова открыть средство и план миграции.

В приведенном выше примере произошел сбой передачи одного из клонированных отчетов из-за того, что отчет с таким именем уже существует. Если взглянуть на код XML плана миграции, то вы увидите следующее.

```
<ReportMigrationData>
    <PaaSWorkspaceCollectionName>SampleWorkspaceCollection</PaaSWorkspaceCollectionName>
    <PaaSWorkspaceId>4c04147b-d8fc-478b-8dcb-bcf687149823</PaaSWorkspaceId>
    <PaaSReportId>525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe</PaaSReportId>
    <PaaSReportLastImportTime>1/3/2017 2:10:19 PM</PaaSReportLastImportTime>
    <PaaSReportName>cloned</PaaSReportName>
    <IsPushDataset>false</IsPushDataset>
    <IsBoundToOldDataset>false</IsBoundToOldDataset>
    <PbixPath>C:\MigrationData\SampleResourceGroup\SampleWorkspaceCollection\4c04147b-d8fc-478b-8dcb-bcf687149823\cloned-525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe.pbix</PbixPath>
    <ExportState>Done</ExportState>
    <LastExportStatus>OK</LastExportStatus>
    <SaaSTargetGroupName>SampleMigrate</SaaSTargetGroupName>
    <SaaSTargetGroupId>6da6f072-0135-4e6c-bc92-0886d8aeb79d</SaaSTargetGroupId>
    <SaaSTargetReportName>cloned</SaaSTargetReportName>
    <SaaSImportState>Failed</SaaSImportState>
    <SaaSImportError>Report with the same name already exists</SaaSImportError>
</ReportMigrationData>
```

Для элемента, вызвавшего сбой, можно изменить имя SaaSTargetReportName.

```
<SaaSTargetReportName>cloned2</SaaSTargetReportName>
```

Затем можно повторно открыть план в средстве миграции и передать отчет, вызвавший сбой.

Вернувшись в Power BI, мы увидим, что отчеты и наборы данных были переданы в рабочую область приложения.

![Список отчетов в службе Power BI](media/migrate-tool/migrate-tool-upload-app-workspace.png)

<a name="upload-local-file"></a>
### <a name="upload-a-local-pbix-file"></a>Передача локального PBIX-файла

Можно передать локальную версию файла Power BI Desktop. Необходимо будет закрыть средство, изменить код XML и указать полный путь к локальному PBIX-файлу в свойстве **PbixPath**.

```
<PbixPath>[Full Path to PBIX file]</PbixPath>
```

После изменения XML повторно откройте план в средстве миграции и передайте отчет.

<a name="directquery-reports"></a>
### <a name="directquery-reports"></a>Отчеты DirectQuery

Необходимо будет обновить строку подключения для отчетов DirectQuery. Это можно сделать на сайте *powerbi.com*. Можно также программно запросить строку подключения из Power BI Embedded (Paas). Пример приведен в разделе [Извлечение строки подключения DirectQuery из отчета PaaS](migrate-code-snippets.md#extract-directquery-connection-string-from-power-bi-workspace-collections).

Затем можно обновить строку подключения для набора данных в службе Power BI и задать учетные данные для источника данных. Чтоб узнать, как это сделать, вы можете ознакомиться со следующими примерами.

* [Обновление строки подключения DirectQuery в Power BI Embedded](migrate-code-snippets.md#update-directquery-connection-string-in-power-bi-embedded)
* [Настройка учетных данных DirectQuery в Power BI Embedded](migrate-code-snippets.md#set-directquery-credentials-in-power-bi-embedded)

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда отчеты перенесены из коллекций рабочей области Power BI в Power BI Embedded, можно обновить приложение и начать внедрение отчетов в эту рабочую область приложения.

Дополнительные сведения см. разделе [How to migrate Power BI Workspace Collection content to Power BI Embedded](migrate-from-power-bi-workspace-collections.md) (Как перенести содержимое коллекции рабочих областей Power BI в Power BI Embedded).

У вас имеются и другие вопросы? [Задайте их в сообществе Power BI](http://community.powerbi.com/).