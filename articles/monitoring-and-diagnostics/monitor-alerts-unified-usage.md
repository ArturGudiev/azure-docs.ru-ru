---
title: "Создание и просмотр оповещений, а также и управление ими с помощью Azure Monitor (\"Оповещения (предварительная версия)\") | Документация Майкрософт"
description: "Используйте новые унифицированные возможности оповещений Azure для централизованного создания и просмотра метрик и правил оповещений журналов, а также для управления ими."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 36729da3-e002-4a64-86b2-2513ca2cbb58
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: vinagara
ms.openlocfilehash: b537bb42d43c4232c100061322e09bf492f2a20f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/24/2018
---
# <a name="create-view-and-manage-alerts-using-azure-monitor---alerts-preview"></a>Создание и просмотр оповещений, а также управление ими с помощью Azure Monitor — интерфейс оповещений (предварительная версия)

## <a name="overview"></a>Обзор
В этой статье показано, как настроить оповещения с помощью нового интерфейса службы "Оповещения (предварительная версия)" на портале Azure. Определение правила оповещений состоит из трех частей:
- Цель: определенный ресурс Azure, который следует отслеживать.
- Критерий: определенное условие или логика в сигнале, которые должны вызывать действие.
- Действие: конкретный вызов, отправленный получателю уведомления — электронное сообщение, текстовое сообщение, веб-перехватчик и т. д.

В интерфейсе оповещений (предварительная версия) используется термин **оповещения журнала**. Это оповещения, для которых сигналом выступает пользовательский запрос на основе [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) или [Azure Application Insights](../application-insights/app-insights-analytics.md). Функция оповещения метрик [оповещение на основе метрик практически в реальном времени](monitoring-near-real-time-metric-alerts.md) при использовании имеющихся оповещений называется **оповещением метрики** в службе "Оповещения (предварительная версия)". В *оповещениях метрик* некоторые типы ресурсов предоставляют [многомерные метрики](monitoring-metric-charts.md) для определенных ресурсов Azure, которые можно уточнить при использовании дополнительных фильтров по измерениям. Такие оповещения называются **многомерными оповещениями метрик**.
Служба "Оповещения Azure (предварительная версия)" также обеспечивает единое представление обо всех правилах оповещений и предоставляет возможность централизованного управления ими, включая просмотр любых неразрешенных оповещений. Дополнительные сведения о функциях см. в статье [Explore the new Alerts (Preview) experience in Azure Monitor](monitoring-overview-unified-alerts.md) (Исследование новой службы "Оповещения (предварительная версия)" в Azure Monitor).

> [!NOTE]
> Служба "Оповещения Azure (предварительная версия)" предлагает новые улучшенные возможности для создания оповещений в Azure. Существующий интерфейс [оповещений Azure](monitoring-overview-alerts.md) остается доступным
>

Ниже приводится пошаговое руководство по использованию интерфейса оповещений Azure (предварительная версия).

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Создание правила оповещений с помощью портала Azure
1. На [портале](https://portal.azure.com/) выберите **Monitor**, а затем в разделе Monitor выберите **Оповещения (предварительная версия)**.  
    ![Мониторинг](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

2. Нажмите кнопку **Новое правило генерации оповещений**, чтобы создать оповещение в Azure.
    ![Добавление оповещения](./media/monitor-alerts-unified/AlertsPreviewOption.png)

3. Появится раздел "Создать оповещение" с тремя элементами: *Define alert condition* (Определите условие оповещения), *Define alert details* (Определите сведения об оповещении) и *Define action group* (Определите группу действий).

    ![Создание правила](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

4.  Определите условие оповещения, используя ссылку **Выбор ресурса** и указав ресурс. Отфильтруйте соответствующим образом и выберите необходимую *подписку*, *тип ресурса*, а затем выберите требуемый *ресурс*.

    >[!NOTE]

    > Прежде чем продолжить, проверьте доступные сигналы для выбранного ресурса.

    ![Выбор ресурса](./media/monitor-alerts-unified/Alert-SelectResource.png)

 Служба "Оповещения Azure (предварительная версия)" позволяет создавать различные типы оповещений в едином интерфейсе. Выберите следующий шаг с учетом типа желаемого оповещения:

    - Для **оповещения метрик**: выполните шаги с 5 по 7, а затем перейдите к шагу 11.
    - Для **оповещения журналов**: перейдите к шагу 8.

    > [!NOTE]

    > Функция "Унифицированные оповещения (предварительная версия)" также поддерживает оповещения журнала действий. [Узнайте больше](monitoring-activity-log-alerts-new-experience.md).

5. *Метрики оповещений*. Для параметра **Тип ресурса** следует выбрать службу платформы или мониторинга (отличную от *Log Analytics*). Выбрав соответствующий **ресурс**, нажмите кнопку *Готово*, чтобы вернуться к созданию оповещений. Затем нажмите кнопку **Добавить критерии**, чтобы выбрать конкретный сигнал из списка параметров сигнала, службу мониторинга и тип, доступный для ресурса, выбранного ранее.

    ![Выбор ресурса](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]

    > Новые возможности метрик, предоставленные только для быстрых оповещений, включены в типы сигналов в качестве метрик из службы платформы.

6. *Оповещения метрик*. Выбрав сигнал, можно указать логику оповещения. Для справки отображаются исторические данные сигнала с возможностью настройки временного окна от последних шести часов до последней недели с помощью кнопки **Show History** (Показать журнал). После настройки визуализации можно выбрать **логику оповещений** из показанных параметров "Условие", "Агрегирование" и "Пороговое значение". В окне предпросмотра логики отображаются условия в визуализации вместе с историей сигнала, чтобы указать, когда было вызвано оповещение. Затем укажите, за какой период времени оповещение должно искать заданное условие, выбрав параметр **Period** (Период), а также частоту вызова оповещения, выбрав параметр **Frequency** (Частота).

    ![Настройка логики сигналов для метрик](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

7. *Метрики оповещений*. Если сигнал является метрикой, в окне оповещений можно выполнять фильтрацию с использованием нескольких точек данных или измерений для указанного ресурса Azure. Аналогично можно выбрать визуализацию журнала сигналов, указав продолжительность в раскрывающемся списке **Show History** (Показать журнал). Кроме того, для выбранной метрики можно выбрать измерения для фильтрации требуемых временных рядов. Можно использовать до пяти измерений, тем не менее выбирать их не обязательно. Для **логики оповещений** можно выбрать один из показанных параметров "Условие", "Агрегирование" и "Пороговое значение". В окне предпросмотра логики отображаются условия визуализации вместе с историей сигнала, чтобы указать, когда оповещение было вызвано в прошлом. Затем укажите, за какой период времени оповещение должно искать заданное условие, выбрав параметр **Period** (Период), а также частоту вызова оповещения, выбрав параметр **Frequency** (Частота).

    ![Настройка логики сигнала многомерной метрики](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

8. *Оповещения журналов.* В качестве **типа ресурса** выберите источник данных аналитики, например *Log Analytics* или *Application Insights*. Выбрав соответствующий **ресурс**, нажмите кнопку *Готово*. Затем нажмите кнопку **Добавить условие**, чтобы просмотреть список доступных параметров сигнала для ресурса. В списке сигналов выберите **Custom log search** (Пользовательский поиск по журналам) для выбранной службы мониторинга журналов, например *Log Analytics* или *Application Insights*.

   ![Выбор ресурса: пользовательский поиск по журналам](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Списки интерфейса оповещений (предварительная версия) могут импортировать запрос аналитических данных как тип сигнала — **Log (Saved Query)** (Журнал (сохраненный запрос)), как показано на приведенном выше изображении. Таким образом, пользователи могут усовершенствовать ваш запрос в Analytics, а затем сохранить его для дальнейшего использования в оповещениях. Дополнительные сведения о сохранении запросов см. в статьях об [использовании поиска по журналам в Log Analytics](../log-analytics/log-analytics-log-searches.md) или [совместном использовании запроса аналитических данных Application Insights](../log-analytics/log-analytics-overview.md). 

9.  *Оповещения журналов*. После выбора запрос для оповещения можно указать в поле **Search Query** (Поисковой запрос). Если синтаксис запроса неверен, в поле отображается ошибка, выделенная красным цветом. Если синтаксис запроса верен, появятся исторические данные указанного запроса для справки в виде графика с возможностью настройки временного интервала от последних шести часов до последней недели.

 ![Настройка правил оповещений](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Визуализация исторических данных может отображаться, только если в результатах запроса есть данные о времени. Если при выполнении запроса появляются обобщенные данные или значения конкретных столбцов, то они отображаются как отдельный график.

    >  Для оповещений журнала типа "Измерения метрик" с использованием Application Insights вы можете указать конкретную переменную для группирования данных, используя параметр **Агрегация по**, как показано ниже:

    ![параметр "Агрегация по"](./media/monitor-alerts-unified/aggregate-on.png)

10.  *Оповещения журналов*. После настройки визуализации можно выбрать **логику оповещений**из показанных параметров "Условие", "Агрегирование" и "Пороговое значение". В логике укажите время для оценки заданного условия с помощью параметра **Period** (Период). Кроме того, укажите частоту запуска оповещений, выбрав параметр **Frequency** (Частота).
**Оповещения журналов** могут создаваться на основе таких данных:
   - *Число записей*. Оповещение создается, если количество записей, возвращаемых запросом, больше или меньше указанного вами значения.
   - *Измерение метрик*. Оповещение создается, если каждое *статистическое значение* в результатах превышает указанное пороговое значение, которое *группируется* в соответствии с выбранным значением. Количество нарушений для оповещения — это количество превышений порогового значения в течение выбранного периода времени. Можно указать значение свойства Всего брешей, чтобы учитывать любое сочетание нарушений в наборе результатов, или значение свойства Последовательные бреши, чтобы учитывать только нарушения, следующие подряд. Дополнительные сведения см. в статье [Log alerts in Azure Monitor — Alerts (Preview)](monitor-alerts-unified-log.md) (Журнал оповещений в Azure Monitor ("Оповещения (предварительная версия)")).

    > [!TIP]
    > В настоящее время в службе "Оповещения (предварительная версия)" оповещения о поиске по журналам могут принимать пользовательские значения *периода* и *частоты* в минутах. Значения могут находиться в диапазоне от 5 до 1440 минут (24 часа). Если требуется, чтобы период оповещения был в диапазоне трех часов, преобразуйте его в минуты (180 минут), прежде чем использовать.

11. В качестве второго шага в параметрах задайте имя оповещения в поле **Имя правила генерации оповещений**, а также введите подробные сведения в поле **Описание** и **Severity** (Серьезность). Эти данные повторно используются во всех оповещениях по электронной почте, уведомлениях или push-уведомлениях Azure Monitor. Кроме того, пользователь может сразу же активировать правило оповещения при создании, переключив параметр **Включить правило при создании**.

    Для параметра **Оповещения журналов** доступны только некоторые дополнительные функции в разделе "Детали оповещения":

    - **Отключить оповещения**. При включении подавления для правила генерации оповещений действия для этого правила отключаются на заданный промежуток времени после создания оповещения. Правило по прежнему выполняется и создает записи оповещений при соблюдении критериев. Это дает вам время устранить проблему без запуска повторяющихся действий.

        ![Отключение оповещений в оповещениях журналов](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

12. В качестве третьего и последнего шага укажите, нужно ли запускать **группу действий** для правила оповещения, если выполняется условие. Вы можете выбрать любую имеющуюся группу действий с оповещением или создать новую. Согласно выбранной группе действий при срабатывании оповещения Azure отправит электронное письмо, текстовое сообщение, вызовет веб-перехватчик, устранит проблему с помощью модуля Runbook Azure, отправит push-уведомление в инструмент ITSM и т. д. Дополнительные сведения о группах действий см. в статье [Create and manage action groups in the Azure portal](monitoring-action-groups.md) (Создание групп действий и управление ими на портале Azure).

    Для **оповещений журналов** доступны дополнительные функции для переопределения действий по умолчанию:

    - **Уведомление по электронной почте** переопределяет тему в письме, отправленном через группу действий. Основной текст сообщения нельзя изменить.
    - **Включить настраиваемые полезные данные JSON** переопределяет Json веб-перехватчика, используемый группами действий, и заменяет полезные данные по умолчанию на пользовательские полезные данные. Дополнительные сведения о форматах веб-перехватчиков см. в статье [Действия веб-перехватчика для правил оповещений журнала](monitor-alerts-unified-log-webhook.md).

        ![Переопределение действий оповещений журналов](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

13. Если все поля действительны и выделены зеленым цветом, нажмите кнопку **Создать правило оповещения**, чтобы создать оповещение в Azure Monitor ("Оповещения (предварительная версия)"). Все оповещения можно просмотреть с помощью панели мониторинга службы "Оповещения (предварительная версия)".

    ![Создание правила](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

    Через несколько минут оповещение включится и будет активироваться, как было описано выше.

## <a name="view-your-alerts-in-azure-portal"></a>Просмотр оповещений на портале Azure

1. На [портале](https://portal.azure.com/) выберите **Monitor**, а затем в разделе Monitor выберите **Оповещения (предварительная версия)**.  

2. Отображается **панель мониторинга службы "Оповещения (предварительная версия)"**, в которой все оповещения Azure объединены и отображаются в единственной панели мониторинга. ![Панель мониторинга оповещений](./media/monitoring-overview-unified/alerts-preview-overview.png)
3. В верхнем левом углу щелкните панель мониторинга, чтобы просмотреть подробный список:
    - *Сработавшие оповещения.* Количество запущенных оповещений, которые соответствовали условиям логики.
    - *Всего правил генерации оповещений*. Количество активных созданных правил оповещений.
4. Отображается список всех оповещений, которые можно щелкнуть, чтобы просмотреть сведения.
5. Чтобы отфильтровать определенные оповещения, можно использовать параметры раскрывающегося списка *Подписка, Группы ресурсов или Ресурс*. Для неразрешенных оповещений выберите параметр *Filter alert* (Оповещение фильтра) для поиска ключевого слова: *имя, критерии оповещения, группа ресурсов и целевой ресурс*.

## <a name="managing-your-alerts-in-azure-portal"></a>Управление оповещениями на портале Azure
1. На [портале](https://portal.azure.com/) выберите **Monitor**, а затем в разделе Monitor выберите **Оповещения (предварительная версия)**.  
2. Нажмите кнопку **​​Управление правилами** на верхней панели, чтобы перейти к разделу управления правилами, в котором перечислены все созданные правила оповещений, включая отключенные оповещения.
3. Чтобы найти определенные правила оповещений, можно либо использовать раскрывающиеся фильтры в верхней части, позволяющие отобразить короткий список правил оповещений для определенной *подписки, группы ресурсов или ресурса*. Помимо этого при использовании панели поиска *Фильтрация оповещений* можно указать ключевое слово, которое сопоставляется с *именем оповещения, условием и целевым ресурсом* для отображения только совпадающих правил.
   ![Правило управления правилами](./media/monitoring-overview-unified/alerts-preview-rules.png)
4. Чтобы просмотреть или изменить определенное правило оповещения, щелкните его имя, которое будет отображаться в виде ссылки.
5. Оповещение отображается следующим образом в виде трехступенчатой структуры: 1. Определение условия оповещения; 2. Сведения о предупреждении; 3. Группа действий. **Целевые условия**. Щелкните, чтобы изменить логику оповещений или добавить новый критерий с помощью значка "Корзина", чтобы удалить предыдущую логику. В разделе сведений об оповещении можно изменить параметры **Описание** и **Серьезность**. Кроме того, можно изменить группу действий или создать новую с помощью кнопки **Новая группа действий**.

   ![Изменение правил генерации оповещений](./media/monitor-alerts-unified/AlertsPreviewEdit.png)

6. Используя верхнюю панель, щелкните **Сохранить**, чтобы внести изменения, **Отменить**, чтобы не вносить изменения, или **Отключить**, чтобы отключить оповещения. Кроме того, можно выбрать **Удалить**, чтобы удалить все правила создания оповещений в Azure.

## <a name="next-steps"></a>Дополнительная информация

- Узнайте больше об [оповещениях на основе метрик практически в реальном времени (предварительная версия)](monitoring-near-real-time-metric-alerts.md).
- Прочитайте [обзор сбора метрики](insights-how-to-customize-monitoring.md) и узнайте, как можно обеспечить, чтобы служба была доступна и отвечала на запросы.
- Подробнее об [оповещениях журналов в интерфейсе Azure "Оповещения (предварительная версия)"](monitor-alerts-unified-log.md)
- [Создание оповещения в журнале действий, используя новую службу "Оповещения (предварительная версия)"](monitoring-activity-log-alerts-new-experience.md)
