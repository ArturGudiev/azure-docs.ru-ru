---
title: "Вертикальное масштабирование виртуальных машин Windows c помощью службы автоматизации Azure | Документация Майкрософт"
description: "Вертикальное масштабирование виртуальной машины Windows в ответ на оповещения мониторинга c помощью службы автоматизации Azure"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ea5169c1a95f00e78ae3f5f177812466eb7a0deb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Вертикальное масштабирование виртуальных машин Windows с помощью службы автоматизации Azure

Вертикальное масштабирование — это процесс увеличения или уменьшения объема ресурсов виртуальной машины в зависимости от рабочей нагрузки. В Azure это можно сделать, изменив размер виртуальной машины. Вертикальное масштабирование можно использовать в следующих сценариях:

* если виртуальная машина используется редко, вы можете уменьшить ее размер, чтобы сократить ежемесячные затраты;
* если виртуальная машина испытывает пиковые нагрузки, ее размер можно увеличить, чтобы расширить емкость.

Ниже представлены шаги для реализации вертикального масштабирования.

1. Настройка службы автоматизации Azure для доступа к виртуальным машинам.
2. Импорт модулей Runbook службы автоматизации Azure для вертикального масштабирования в подписку
3. Добавление веб-перехватчика в модуль Runbook.
4. Добавление правила оповещения для виртуальной машины

> [!NOTE]
> Размеры, до которых можно масштабировать виртуальную машину, могут быть ограничены из-за размера первой виртуальной машины ввиду поддерживаемых размеров кластера, в котором развернута текущая виртуальная машина. В этой статье мы учли эту особенность и в опубликованных модулях Runbook службы автоматизации использовали для масштабирования только приведенные ниже примеры размеров виртуальных машин. Это означает, что нельзя внезапно увеличить виртуальную машину размером Standard_D1v2 до размера Standard_G5 или уменьшить до Basic_A0.
> 
> | Пары размеров виртуальных машин, для которых можно применять вертикальное масштабирование |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Настройка службы автоматизации Azure для доступа к виртуальным машинам.
Первое, что вам нужно сделать, — создать учетную запись службы автоматизации Azure, где будут размещаться модули Runbook, используемые для масштабирования виртуальной машины. Недавно в службе автоматизации появилась функция "Запуск от имени...", которая упрощает настройку субъекта-службы для автоматического запуска модулей Runbook от имени пользователя. Дополнительные сведения об этом см. в следующей статье:

* [Проверка подлинности модулей Runbook в Azure с помощью учетной записи запуска от имени](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Импорт модулей Runbook службы автоматизации Azure для вертикального масштабирования в подписку
Модули Runbook, необходимые для вертикального масштабирования виртуальной машины, уже опубликованы в коллекции Runbook службы автоматизации Azure. Вам потребуется импортировать их в подписку. Дополнительные сведения об импорте модулей Runbook см. в статье:

* [Runbook и коллекции модулей для службы автоматизации Azure](../../automation/automation-runbook-gallery.md)

На приведенном ниже рисунке показаны модули Runbook, которые необходимо импортировать.

![Импорт модулей Runbook](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Добавление веб-перехватчика в модуль Runbook.
После импорта модулей Runbook в них необходимо добавить веб-перехватчик. Этого может потребовать система оповещений виртуальной машины. Дополнительные сведения о создании веб-перехватчика для вашего модуля Runbook см. в статье:

* [Объекты Webhook в службе автоматизации Azure](../../automation/automation-webhooks.md)

Прежде чем закрывать диалоговое окно, убедитесь, что вы скопировали веб-перехватчик. Он понадобится в следующем разделе.

## <a name="add-an-alert-to-your-virtual-machine"></a>Добавление правила оповещения для виртуальной машины
1. Перейдите в раздел параметров виртуальной машины.
2. Щелкните "Правила оповещения".
3. Щелкните "Добавить оповещение".
4. Выберите метрику, на основе значения которой будут срабатывать оповещения.
5. Выберите условие, при выполнении которого будет срабатывать оповещение.
6. Установите пороговое значение для условия, упомянутого выше.
7. Выберите период для проверки условий и порогового значения, упомянутых выше.
8. Вставьте скопированный веб-перехватчик, описанный в предыдущем разделе.

![Добавить правило оповещения для виртуальной машины 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Добавить правило оповещения для виртуальной машины 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

