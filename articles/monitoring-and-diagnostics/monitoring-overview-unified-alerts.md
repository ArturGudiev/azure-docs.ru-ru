---
title: "Изучение возможностей новой службы \"Оповещения (предварительная версия)\" в Azure Monitor | Документация Майкрософт"
description: "Узнайте, как с помощью новых простых и масштабируемых возможностей оповещений в Azure можно упростить создание, просмотр оповещений и управление ими"
author: manishsm-msft
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mamit
ms.custom: 
ms.openlocfilehash: 316dcd53509897a6efc387749ca6f9ec268cb7ac
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2018
---
# <a name="explore-the-new-alerts-preview-experience-in-azure-monitor"></a>Изучение возможностей новой службы "Оповещения (предварительная версия)" в Azure Monitor

## <a name="overview"></a>Обзор
 Внешний вид и возможности службы "Оповещения" в Azure обновлены. Она доступна на вкладке **Оповещения (предварительная версия)** в разделе Azure Monitor. Ниже приведены некоторые преимущества использования новой службы "Оповещения (предварительная версия)":

 - **Разделение сработавших оповещений и правил генерации оповещений**. В службе "Оповещения (предварительная версия)" правила генерации оповещений (определение условия, которое инициирует оповещение) и сработавшие оповещения (экземпляр выполнения правила оповещения) дифференцированы, поэтому оперативные представления и представления конфигурации отделены.
 - **Единый процесс создания оповещений о метриках и оповещения журналов**. В новой службе "Оповещения (предварительная версия)" содержатся рекомендации по настройке правила генерации оповещений, благодаря чему можно легко понять, для чего они используются.
 - **Просмотр сработавших оповещений Log Analytics на портале Azure**. В службе "Оповещения (предварительная версия)" теперь можно также просмотреть сработавшие оповещения Log Analytics в подписке.  
 - **Единый процесс создания оповещений в журнале действий**. Теперь можно создать оповещения в журнале действий, непосредственно используя параметр **Отслеживание** > **Оповещения (предварительная версия)**. Раньше это можно было сделать только с помощью параметра **Отслеживание** > **Журнал действий**.

В разделах ниже более подробно описывается принцип работы новой службы.

## <a name="taxonomy"></a>Классификация
В службе "Оповещения (предварительная версия)" для разделения правила генерации оповещений и объектов сработавших оповещений с единым процессом создания разных типов оповещений используются следующие понятия.

- **Целевой ресурс**. Целевым ресурсом может быть любой ресурс Azure. Он определяет область и сигналы, доступные для оповещений. Примеры целевых ресурсов: виртуальная машина, учетная запись хранения, масштабируемый набор виртуальных машин, рабочая область Log Analytics или решение.

- **Критерии**. Представляют собой сочетание сигнала и логики, примененных для целевого ресурса. Примеры критериев: загрузка ЦП > 70 %, время отклика сервера > 4 мс, число результатов запроса журнала > 100 и т. д. 

- **Сигнал**. Сигналы генерируются целевым ресурсом и могут включать несколько типов. В этой предварительной версии поддерживаются такие типы сигналов, как **метрика**, **журнал действий**, **Application Insights** и **журнал**.

- **Логика**. Определенная пользователем логика, позволяющая проверить, соответствует ли сигнал ожидаемым значениям или диапазону.  
 
- **Действие**. Определенный вызов, отправляемый получателю уведомления (например, отправка сообщения на электронный адрес или передача данных на URL-адрес веб-перехватчика). Обычно уведомления могут активировать несколько действий. Типы оповещений, поддерживаемые в этой предварительной версии, поддерживают группы действий.  
 
- **Правило генерации оповещений**. Определение условия, которое инициирует оповещение. В этой предварительной версии правило генерации оповещений записывает целевой ресурс и критерии для создания оповещения. Правило генерации оповещений можно включить или отключить.
 
- **Сработавшее оповещение**. Создается, когда срабатывает включенное правило генерации оповещений. Объект сработавшего оповещения может быть в состоянии "Сработало" или "Не разрешено".

    > [!NOTE]
    > В этом состоит отличие от текущей службы оповещений, в которой оповещение представляет и правило, и сработавшее оповещение, и, таким образом, может быть в одном из следующих состояний: "Предупреждение", "Активно" или "Отключено".
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Единое место для просмотра оповещений и управления ими
Цель службы "Оповещения (предварительная версия)" — предоставить единое расположение для просмотра всех оповещений Azure и управления ими. В следующих подразделах описываются функции каждого отдельного экрана новой службы.

### <a name="alerts-preview-overview-page"></a>Страница обзора службы "Оповещения (предварительная версия)"
На странице со сведениями **отслеживания службы "Оповещения (предварительная версия)"** отображаются статистические сводки обо всех сработавших оповещениях, а также общее количество настроенных или включенных правил генерации оповещений. Кроме того, приводится список всех сработавших оповещений. При изменении подписок или параметров фильтра обновятся статистические сводки и список сработавших оповещений.

> [!NOTE]
> Сработавшие оповещения, показанные в окне службы "Оповещения (предварительная версия)", поддерживают только оповещения журналов и метрик. В разделе Azure Monitor показано число сработавших оповещений, включая оповещения в старой версии службы "Оповещения Azure".

 ![alerts-preview-overview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Управление правилами генерации оповещений
**Monitor - Alerts (Preview)>Rules** (Отслеживание — Оповещения (предварительная версия)>Правила) — это единая страница для управления всеми правилами генерации оповещений в подписках Azure. На этой странице перечислены все правила генерации оповещений (включенные или отключенные), и на ней доступна фильтрация по целевым ресурсам, группам ресурсов, имени правила или состоянию. Здесь также можно включить и отключить правила генерации оповещений, а также изменить их.  

 ![alerts-preview-rules](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Единый интерфейс создания оповещений во всех источниках данных мониторинга
В службе "Оповещения (предварительная версия)" можно согласованно создать оповещения, независимо от службы мониторинга или типа сигнала. Все сработавшие оповещения и связанные сведения доступны на одной странице.  
 
Создание оповещения — это задача из трех шагов, где пользователь сначала выбирает целевой объект для оповещения, затем — нужный сигнал и указывает логику для применения в сигнале как часть правила генерации оповещений. При таком упрощенном процессе создания пользователю больше не нужно знать поддерживаемый источник отслеживания или сигналы перед выбором ресурса Azure. В едином интерфейсе создания можно автоматически отфильтровать список доступных сигналов на основе выбранного целевого ресурса, а также получить справочные сведения о создании логики оповещений.

Дополнительные сведения о том, как создать типы оповещений ниже, см. [здесь](monitor-alerts-unified-usage.md).
- Оповещения о метриках (называются оповещениями на основе метрик практически в реальном времени).
- Оповещения журналов (в Log Analytics).
- Оповещения журналов (в журналах действий).
- Оповещения журналов (в Application Insights).

 

## <a name="alert-types-supported-in-this-preview"></a>Типы оповещений, поддерживаемые в этой предварительной версии


| **Тип сигнала** | **Источник мониторинга** | **Описание** | 
|-------------|----------------|-------------|
| Метрика | Azure Monitor | В текущей версии службы называются [**оповещениями на основе метрик практически в реальном времени**](monitoring-near-real-time-metric-alerts.md). Эти оповещения на основе метрик поддерживают оценку условий метрик с частотой в 1 минуту и позволяют создавать правила на основе нескольких метрик. Список поддерживаемых типов ресурсов доступен [здесь](monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for). Другие оповещения метрик, определенных [здесь](monitoring-overview-alerts.md#alerts-in-different-azure-services), не поддерживаются в службе "Оповещения (предварительная версия)".|
| Журналы  | Служба Log Analytics | Получают уведомления или запускают выполнение автоматизированных действий, когда запрос поиска по журналам для метрики или данных события отвечает соответствующим критериям.|
| Журналы  | Журналы действий | Эта категория содержит записи всех действий создания, обновления и удаления, выполненных с помощью выбранного целевого объекта (ресурса, группы ресурсов или подписки). |
| Журналы  | Журналы работоспособности службы | Не поддерживаются в службе "Оповещения (предварительная версия)".   |
| Журналы  | Application Insights | Эта категория содержит журналы с данными производительности приложения. С помощью аналитического запроса можно определить условия для выполнения действий на основе данных приложения. |
| Метрика | Application Insights | Не поддерживаются в службе "Оповещения (предварительная версия)". |
| Тесты доступности | Application Insights | Не поддерживаются в службе "Оповещения (предварительная версия)". |


## <a name="next-steps"></a>Дополнительная информация
- [Создание и просмотр оповещений, а также и управление ими с помощью Azure Monitor ("Оповещения (предварительная версия)")](monitor-alerts-unified-usage.md)
- [Log alerts in Azure Monitor - Alerts (Preview)](monitor-alerts-unified-log.md) (Оповещения журналов в Azure Monitor ("Оповещения (предварительная версия)"))
- [Оповещения на основе метрик практически в реальном времени (предварительная версия)](monitoring-near-real-time-metric-alerts.md)
- [Создание оповещений журнала действий с помощью новой функции "Оповещения (предварительная версия)"](monitoring-activity-log-alerts-new-experience.md)
