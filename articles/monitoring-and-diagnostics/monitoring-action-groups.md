---
title: "Создание групп действий и управление ими на портале Azure | Документация Майкрософт"
description: "Узнайте, как создавать группы действий и управлять ими на портале Azure."
author: dkamstra
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: dukek
ms.openlocfilehash: 772b9c2b9532bd2cc37ad89db92545297eecd903
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Создание групп действий и управление ими на портале Azure
## <a name="overview"></a>Обзор ##
В этой статье показано, как создавать группы действий и управлять ими на портале Azure.

Группы действий дают возможность настроить список действий. Эти группы можно использовать в каждом определенном оповещении, чтобы при появлении такого оповещения предпринимались одинаковые действия.

Группа действий может содержать до 10 действий каждого типа. Каждое действие состоит из следующих свойств:

* **Имя:** уникальный идентификатор в группе действий.  
* **Тип действия**. Вы можете отправить текстовое сообщение, сообщение электронной почты, вызвать веб-перехватчик, отправить данные в инструмент ITSM, вызвать приложение Azure или запустить модуль runbook службы автоматизации.
* **Подробные сведения.** Сведения о соответствующем телефонном номере, адресе электронной почты, универсальном коде ресурса (URI) веб-перехватчика или сведения о подключении ITSM.

Дополнительные сведения о настройке групп действий с помощью шаблонов Azure Resource Manager см. в статье [Создание группы действий с помощью шаблона Resource Manager](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Создание группы действий с помощью портала Azure ##
1. На [портале](https://portal.azure.com) выберите **Монитор**. В колонке **Монитор** объединены все параметры мониторинга и данные в одном представлении.

    ![Служба "Монитор"](./media/monitoring-action-groups/home-monitor.png)
2. В разделе **Параметры** выберите **Группы действий**.

    ![Вкладка Action groups (Группы действий)](./media/monitoring-action-groups/action-groups-blade.png)
3. Выберите **Add action group** (Добавить группу действий) и заполните поля.

    ![Команда Add action group (Добавить группу действий)](./media/monitoring-action-groups/add-action-group.png)
4. Введите имя в поля **Action group name** (Имя группы действий) и **Short name** (Короткое имя). Короткое имя используется вместо полного имени группы действий при отправке уведомлений с помощью этой группы.

      ![Диалоговое окно Add action group (Добавить группу действий)](./media/monitoring-action-groups/action-group-define.png)

5. Поле **Подписка** автоматически заполняется вашей текущей подпиской. В этой подписке сохраняются группы действий.

6. Выберите **группу ресурсов**, в которой хранится группа действий.

7. Определите список действий, указав для каждого из них следующие данные:

    a. **Имя.** Введите уникальный идентификатор для этого действия.

    Б. **Тип действия.** Вы можете выбрать электронную почту, SMS, приложение Azure, веб-перехватчик, ITSM или модуль Runbook службы автоматизации.

    c. **Подробные сведения**. В зависимости от выбранного типа действия укажите номер телефона, адрес электронной почты, универсальный код ресурса (URI) веб-перехватчика, приложение Azure, сведения о подключении ITSM или модуль runbook службы автоматизации. Для действия ITSM дополнительно укажите **рабочий элемент** и заполните другие поля, требуемые инструментом ITSM.

   > [!NOTE]
   > Действие ITSM требует подключения ITSM. Дополнительные сведения о создании подключения ITSM см. в статье [Централизованное управление рабочими элементами ITSM с помощью соединителя управления ИТ-службами (предварительная версия)](../log-analytics/log-analytics-itsmc-overview.md). 

8. Нажмите кнопку **ОК**, чтобы создать группу действий.

## <a name="manage-your-action-groups"></a>Управление группами действий ##
Созданная группа действий будет отображаться в разделе **Action groups** (Группы действий) в колонке **Монитор**. Выберите группу действий, которой необходимо управлять:

* добавлять, изменять или удалять действия;
* удалить группу действий.

## <a name="next-steps"></a>Дополнительная информация ##
* Дополнительные сведения о поведении SMS-оповещений в группе действий см. в [этой статье](monitoring-sms-alert-behavior.md).  
* Узнайте о [схеме веб-перехватчика для оповещений журнала действий](monitoring-activity-log-alerts-webhook.md).  
* Дополнительные сведения о соединителе ITSM см. в статье [Централизованное управление рабочими элементами ITSM с помощью соединителя управления ИТ-службами (предварительная версия)](../log-analytics/log-analytics-itsmc-overview.md).
* Дополнительные сведения о лимитах для оповещений см. в статье [Ограничение частоты отправки для SMS, сообщений электронной почты и вызовов Webhook](monitoring-alerts-rate-limiting.md).
* Изучите [обзор оповещений журнала действий](monitoring-overview-alerts.md) и узнайте, как получать оповещения.  
* Узнайте, как [настроить оповещения при поступлении уведомлений о работоспособности службы](monitoring-activity-log-alerts-on-service-notifications.md).
