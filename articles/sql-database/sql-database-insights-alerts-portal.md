---
title: "Создание оповещений для базы данных SQL с помощью портала Azure | Документация Майкрософт"
description: "Используйте портал Azure для создания оповещений базы данных SQL, которые могут активировать уведомления или автоматизированные операции при выполнении заданных условий."
author: aamalvea
manager: jhubbard
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: fd21c9b5e573ac6a47fef88c2a9d31c52618ecb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2017
---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Создание оповещений для базы данных SQL Azure и хранилища данных с помощью портала Azure

## <a name="overview"></a>Обзор
В этой статье показано, как настроить оповещения для базы данных SQL Azure и хранилища данных с помощью портала Azure. В этой статье также приведены рекомендации по настройке периодов оповещений.    

Вы можете получать оповещения на основе отслеживания метрик или событий в службах Azure.

* **Значения метрик**. Оповещение активируется, когда значение указанной метрики выходит за рамки заданного порогового значения. То есть сначала оно активируется, когда условие выполняется, а затем — когда условие перестает выполняться.    
* **События журнала действий**. Оповещение может активироваться при *каждом* событии или только тогда, когда выполняется определенное число событий.

Для оповещения можно настроить действие, выполняемое при активации оповещения:

* отправка уведомлений по электронной почте администратору службы и соадминистраторам;
* отправка уведомления на указанные дополнительные электронные адреса;
* вызов webhook;

Для настройки правил генерации оповещений и получении сведений о них можно использовать:

* [портал Azure](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [интерфейс командной строки (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Создание правила генерации оповещений на основе метрики с помощью портала Azure
1. На [портале](https://portal.azure.com/)найдите ресурс, который нужно отслеживать, и выберите его.
2. Этот шаг выполняется по-разному для баз данных SQL Server и эластичных пулов и хранилищ данных SQL. 

   - **Инструкция только для баз данных SQL и эластичных пулов:** в разделе "ОТСЛЕЖИВАНИЕ" выберите пункты **Оповещения** или **Правила генерации оповещений**. Текст и значок для разных ресурсов могут незначительно отличаться.  
   
     ![Мониторинг](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **Инструкция только для хранилищ данных SQL:** выберите пункт **Отслеживание** в разделе "ОБЩИЕ ЗАДАЧИ. Выберите граф **Потребление DWU**.

     ![ОБЩИЕ ЗАДАЧИ](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Выберите **Добавить оповещение** и заполните поля.
   
    ![Добавить оповещение](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. Введите **имя** правила генерации оповещений и укажите **описание**, отображаемое также в уведомлениях по электронной почте.
5. Если вы выбрали **метрику** для отслеживания, то выберите для нее **условие** и **пороговое значение**. Кроме того, выберите **период**, в течение которого должно быть выполнено правило метрики, прежде чем будет активировано оповещение. Например, если используется период "PT5M", а оповещение определяет загрузку ЦП выше 80 %, такое оповещение активируется, если **средняя** загрузка ЦП превышает 80 % не менее 5 минут. После первой активации оповещение активируется повторно, если средняя загрузка ЦП будет ниже 80 % не менее 5 минут. Измерение загрузки ЦП происходит раз в минуту. В следующей таблице представлены поддерживаемые временные окна и типы статистической обработки, используемые каждым оповещением. Не все оповещения используют среднее значение.   
6. Установите флажок **Владельцы, авторы и читатели электронных писем** , если администраторы и соадминистраторы должны получать электронные сообщения при активации оповещения.
7. Если вы хотите отправлять уведомления при активации оповещения на дополнительные электронные адреса, добавьте их в поле **Дополнительные адреса электронной почты администратора** . При указании нескольких электронных адресов разделите их точкой с запятой: *email@contoso.com;email2@contoso.com*
8. Укажите допустимый универсальный код ресурса (URI) в поле **Webhook** , если его необходимо вызывать при срабатывании оповещения.
9. Завершив настройку, нажмите кнопку **ОК** , чтобы создать оповещение.   

Через несколько минут оповещение включится и будет активироваться, как было описано выше.

## <a name="managing-your-alerts"></a>Управление оповещениями
После создания оповещение можно выбрать и:

* просмотреть диаграмму, отображающую пороговое и фактические значения метрики за предыдущий день;
* изменить или удалить его;
* **отключить** или **включить** его, если нужно временно остановить или возобновить получение уведомлений для данного оповещения.


## <a name="sql-database-alert-values"></a>Значения оповещений базы данных SQL

| Тип ресурса | Имя метрики | Понятное имя | Тип статистической обработки | Минимальный интервал времени для оповещений|
| --- | --- | --- | --- | --- |
| База данных SQL | cpu_percent | Процент использования ЦП | Среднее | 5 мин |
| База данных SQL | physical_data_read_percent | Процент операций ввода/вывода данных | Среднее | 5 мин |
| База данных SQL | log_write_percent | Log IO percentage | Среднее | 5 мин |
| База данных SQL | dtu_consumption_percent | Процент использования DTU | Средняя | 5 мин |
| База данных SQL | storage | Total database size | Максимальная | 30 минут |
| База данных SQL | connection_successful | Успешные подключения | Всего | 10 минут |
| База данных SQL | connection_failed | Неудачные подключения | Всего | 10 минут |
| База данных SQL | blocked_by_firewall | Заблокировано брандмауэром | Всего | 10 минут |
| База данных SQL | deadlock | Взаимоблокировки | Всего | 10 минут |
| База данных SQL | storage_percent | Размер базы данных в процентах | Максимальная | 30 минут |
| База данных SQL | xtp_storage_percent | In-Memory OLTP storage percent (Preview) | Среднее | 5 мин |
| База данных SQL | workers_percent | Workers percentage | Средняя | 5 мин |
| База данных SQL | sessions_percent | Sessions percent | Среднее | 5 мин |
| База данных SQL | dtu_limit | DTU limit | Среднее | 5 мин |
| База данных SQL | dtu_used | DTU used | Среднее | 5 мин |
||||||
| Эластичный пул | cpu_percent | Процент использования ЦП | Средняя | 10 минут |
| Эластичный пул | physical_data_read_percent | Процент операций ввода/вывода данных | Средняя | 10 минут |
| Эластичный пул | log_write_percent | Log IO percentage | Среднее | 10 минут |
| Эластичный пул | dtu_consumption_percent | Процент использования DTU | Средняя | 10 минут |
| Эластичный пул | storage_percent | Storage percentage | Средняя | 10 минут |
| Эластичный пул | workers_percent | Workers percentage | Среднее | 10 минут |
| Эластичный пул | eDTU_limit | eDTU limit | Средняя | 10 минут |
| Эластичный пул | storage_limit | Storage limit | Среднее | 10 минут |
| Эластичный пул | eDTU_used | eDTU used | Среднее | 10 минут |
| Эластичный пул | storage_used | Storage used | Среднее | 10 минут |
||||||               
| Хранилище данных SQL | cpu_percent | Процент использования ЦП | Средняя | 10 минут |
| Хранилище данных SQL | physical_data_read_percent | Процент операций ввода/вывода данных | Среднее | 10 минут |
| Хранилище данных SQL | storage | Total database size | Максимальная | 10 минут |
| Хранилище данных SQL | connection_successful | Успешные подключения | Всего | 10 минут |
| Хранилище данных SQL | connection_failed | Неудачные подключения | Всего | 10 минут |
| Хранилище данных SQL | blocked_by_firewall | Заблокировано брандмауэром | Всего | 10 минут |
| Хранилище данных SQL | service_level_objective | Целевой уровень служб базы данных. | Всего | 10 минут |
| Хранилище данных SQL | dwu_limit | Лимит DWU. | Максимальная | 10 минут |
| Хранилище данных SQL | dwu_consumption_percent | DWU percentage | Среднее | 10 минут |
| Хранилище данных SQL | dwu_used | DWU used | Средняя | 10 минут |
||||||


## <a name="next-steps"></a>Дополнительная информация
* [Ознакомьтесь с общими сведениями о мониторинге Azure](../monitoring-and-diagnostics/monitoring-overview.md) , включая типы информации, которую можно собирать и отслеживать.
* Узнайте больше о [настройке веб-перехватчиков webhook в оповещениях](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Ознакомьтесь с [обзором журналов диагностики](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) , чтобы собирать подробные метрики о службе с высокой частотой.
* Прочитайте [обзор сбора метрики](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) и узнайте, как можно обеспечить, чтобы служба была доступна и отвечала на запросы.
