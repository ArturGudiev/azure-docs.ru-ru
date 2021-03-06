---
title: "Вызов модуля Runbook службы автоматизации Azure из оповещения Log Analytics | Документация Майкрософт"
description: "В этой статье приведены общие сведения о том, как вызывать модуль Runbook службы автоматизации из оповещения Log Analytics в Operations Management Suite."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: a10be867965eef9746a0f4cc9b14c4fc429f6e35
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2018
---
# <a name="call-an-azure-automation-runbook-from-a-log-analytics-alert"></a>Вызов модуля Runbook службы автоматизации Azure из оповещения Log Analytics

Вы можете настроить в службе Azure Log Analytics оповещение, запись о котором будет создаваться, если результаты соответствуют заданным критериям. Затем в результате этого оповещения может автоматически запускаться модуль Runbook службы автоматизации Azure, чтобы попытаться автоматически устранить проблему. 

Например, оповещение может указывать на продолжительную пиковую загрузку процессора или сбой процесса приложения, критически важного для работы бизнес-приложения. Затем модуль Runbook может записать соответствующее событие в журнал событий Windows.  

Существует два способа вызвать модуль Runbook в конфигурации оповещения, например:

* использование веб-перехватчика.
   * Это единственный доступный способ в том случае, если рабочая область OMS не связана с учетной записью службы автоматизации.
   * Если у вас есть учетная запись службы автоматизации, связанная с рабочей областью OMS, этот вариант остается доступным.  

* Непосредственный выбор модуля Runbook.
   * Этот способ доступен, только если рабочая область OMS связана с учетной записью службы автоматизации.

## <a name="calling-a-runbook-by-using-a-webhook"></a>Вызов модуля Runbook с помощью веб-перехватчика

Вы можете использовать веб-перехватчик, чтобы запустить определенный модуль Runbook в службе автоматизации Azure с помощью единого HTTP-запроса. Перед настройкой [оповещения Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules) для вызова модуля Runbook с помощью веб-перехватчика в качестве реакции на оповещение необходимо [создать веб-перехватчик](automation-webhooks.md#creating-a-webhook) для модуля Runbook, который вызывается с помощью этого метода. Запишите URL-адрес веб-перехватчика, так как он потребуется при настройке правила оповещения.   

## <a name="calling-a-runbook-directly"></a>Непосредственный вызов модуля Runbook

В рабочей области Operations Management Suite можно установить и настроить предложение службы автоматизации и управления. При настройке параметра действий оповещения для модуля Runbook можно просмотреть все модули Runbook из раскрывающегося списка **Выберите Runbook** и выбрать тот модуль, который следует запускать в ответ на оповещение. Выбранный модуль Runbook можно запустить в рабочей области в Azure или гибридной рабочей роли Runbook. 

После создания оповещения с помощью параметра модуля Runbook для этого модуля создается веб-перехватчик. Веб-перехватчик можно увидеть, если перейти в учетную запись службы автоматизации и открыть область "Веб-перехватчик" выбранного модуля Runbook. 

При удалении оповещения веб-перехватчик не удаляется. Неудаленный объект не является проблемой. Он представляет собой потерянный элемент, который в конечном итоге потребуется удалить вручную для поддержания порядка в учетной записи службы автоматизации.  

## <a name="characteristics-of-a-runbook"></a>Характеристики модуля Runbook

Оба метода вызова модуля Runbook из оповещения Log Analytics обладают характеристиками, которые необходимо изучить, прежде чем настраивать правила генерации оповещения. 

Данные оповещения передаются в формате JSON в одиночное свойство с именем **SearchResult**. Этот формат используется для действий, которые runbook и веб-перехватчик выполняют со стандартными полезными данными. Действиям веб-перехватчика с пользовательскими полезными данными (включая **IncludeSearchResults:True** в **RequestBody**) соответствует свойство **SearchResults**.

Для модуля Runbook необходим входной параметр с именем **WebhookData** типа **Object**. Этот параметр может быть обязательным или необязательным. С его помощью оповещение передает результаты поиска в модуль Runbook.

```powershell
param  
    (  
    [Parameter (Mandatory=$true)]  
    [object] $WebhookData  
    )
```
Необходим также код для преобразования параметра **WebhookData** в объект PowerShell.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
```

Результат поиска **$SearchResult** представляет собой массив объектов. Каждый объект содержит поля со значениями из одного результата поиска.


## <a name="example-walkthrough"></a>Пошаговое руководство по примеру

В примере графического модуля Runbook, приведенном ниже, показано, как работает этот процесс. Модуль запускает службу Windows.

![Графический модуль Runbook для запуска службы Windows](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)

Модуль Runbook содержит один входной параметр типа **Object**, называемый **WebhookData**. Он содержит данные веб-перехватчика, переданные из оповещения, которое содержит результат **SearchResult**.

![Входные параметры Runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)

В этом примере мы создали два настраиваемых поля в Log Analytics: **SvcDisplayName_CF** и **SvcState_CF**. С помощью этих полей из события, записанного в системном журнале событий, извлекается отображаемое имя службы и ее состояние (работает или остановлена). Затем мы создали правило генерации оповещения с помощью поискового запроса ниже, чтобы определить, когда служба диспетчера очереди печати в системе Windows будет остановлена.

`Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` 

Это может быть любая интересующая вас служба, но в этом примере мы используем уже имеющуюся службу, являющуюся частью ОС Windows. Действие оповещения настраивается так, чтобы выполнить наш модуль Runbook, используемый в данном примере, и запустить его в гибридной рабочей роли Runbook, включенной в целевой системе.   

Действие кода Runbook **Get Service Name from LA** (Получить имя службы из LA) преобразовывает строку формата JSON в объектный тип и выполняет фильтрацию по элементу **SvcDisplayName_CF**, чтобы извлечь отображаемое имя службы Windows и передать его в следующее действие, которое проверяет, остановлена ли служба, прежде чем попытаться ее перезапустить. **SvcDisplayName_CF** представляет собой [пользовательское поле](../log-analytics/log-analytics-custom-fields.md), созданное в Log Analytics для демонстрации этого примера.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

При остановке службы правило генерации оповещения в Log Analytics обнаруживает совпадение, запускает модуль Runbook и отправляет в него контекст оповещения. Модуль Runbook выполняет проверку остановки службы. Если обнаруживается, что служба остановлена, модуль Runbook пытается повторно запустить ее, проверяет, запущена ли она соответствующим образом, а затем отображает результаты.     

Кроме того, если у вас нет учетной записи службы автоматизации, связанной с рабочей областью Operations Management Suite, вы можете настроить правило генерации оповещений с помощью действия веб-перехватчика. Действие веб-перехватчика запустит модуль Runbook. С помощью этого действия модуль Runbook также будет настроен для преобразования строки в формате JSON и фильтрации результата **SearchResult** согласно упомянутым выше инструкциям.    

>[!NOTE]
> Если ваша рабочая область переведена на [новый язык запросов Log Analytics](../log-analytics/log-analytics-log-search-upgrade.md), полезные данные веб-перехватчика будут работать по-другому. Подробные сведения о формате см. в документации [API REST Log Analytics в Azure](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Дополнительная информация

* Дополнительные сведения об оповещениях в Log Analytics и их создании см. в статье [Оповещения в Log Analytics](../log-analytics/log-analytics-alerts.md).

* Сведения о том, как запускать модули Runbook с помощью веб-перехватчика, см. в статье [Запуск Runbook службы автоматизации Azure с помощью объекта webhook](automation-webhooks.md).
