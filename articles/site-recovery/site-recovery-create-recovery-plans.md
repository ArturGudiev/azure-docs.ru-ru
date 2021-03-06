---
title: "Создание и настройка плана восстановления для отработки отказа и восстановления в Azure Site Recovery | Документация Майкрософт"
description: "Подробнее о создании и настройке планов восстановления в Azure Site Recovery. В этой статье описывается отработка отказа и восстановление виртуальных машин и физических серверов."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Создание плана восстановления с помощью Site Recovery

В этой статье описывается создание и настройка плана восстановления в [Azure Site Recovery](site-recovery-overview.md).

Созданный план восстановления делает следующее:

* Определяет группы компьютеров, для которых отработка отказа и запуск выполняются одновременно.
* Создают зависимости между машинами за счет их объединения в группы плана восстановления. Например, если отработка отказа и восстановление выполняются для конкретного приложения, все виртуальные машины этого приложения следует объединить в одну группу плана восстановления.
* Выполняют отработку отказа. Вы можете запустить тестовую, плановую или внеплановую отработку отказа в плане восстановления.

## <a name="why-use-a-recovery-plan"></a>Для чего используется план восстановления

План восстановления помогает подготовить систематический процесс восстановления путем создания небольших независимых модулей, которыми можно управлять. Обычно эти модули представляют приложение в среде. План восстановления может помочь вам определить последовательность, в которой запускаются виртуальные машины. Вы также можете использовать план восстановления для автоматизации общих задач во время восстановления.

> [!TIP]
> Единственный способ проверить, подготовлена ли ваша среда к переносу в облако или аварийному восстановлению, — убедиться, что каждое ваше приложение является частью плана восстановления. Кроме того, каждый план восстановления проверяется в Azure путем тестирования восстановления. При такой готовности можно уверенно перенести весь центр обработки данных или выполнить его отработку отказа в Azure.
 
Ниже приведены три ключевых положения плана восстановления:

* Моделирование приложения для отслеживания зависимостей.
* Автоматизация большинства задач восстановления для уменьшения RTO.
* Тестовая отработка отказа для обеспечения готовности к аварии.

### <a name="model-an-application-to-capture-dependencies"></a>Моделирование приложения для отслеживания зависимостей

План восстановления — это группа виртуальных машин, обычно содержащих приложение, для которых одновременно выполняется отработка отказа. Используя конструкции плана восстановления, можно расширить эту группу для записи свойств приложения. 

В этой статье мы используем типичное трехуровневое приложение, которое может использовать внутренний сервер SQL, ПО промежуточного слоя и веб-интерфейс. Вы можете настроить план восстановления, чтобы обеспечить правильный порядок запуска виртуальных машин после отработки отказа. Внутренний сервер SQL должен запускаться первым, далее ПО промежуточного слоя, а затем веб-интерфейс. Этот порядок гарантирует, что к моменту запуска последней виртуальной машины приложение будет работать. 

Например, когда запускается ПО промежуточного слоя, оно пытается подключиться к уровню SQL. План восстановления гарантирует, что уровень SQL уже будет работать. Запуск интерфейсных серверов в последнюю очередь гарантирует, что пользователи не подключатся к URL-адресу приложения, пока все компоненты не будут запущены, а приложение — готово к приему запросов. Для создания этих зависимостей настройте план восстановления, чтобы добавить группы, а затем выберите виртуальную машину. Чтобы переместить виртуальную машину в другую группу, измените ее группу.

![Снимок экрана примера плана восстановления в Site Recovery](./media/site-recovery-create-recovery-plans/rp.png)

После завершения настройки вы можете визуализировать точные шаги восстановления. Ниже приведен порядок действий, выполненных во время отработки отказа по плану восстановления:

1. Сначала выполняется шаг завершения работы, на котором осуществляется попытка отключить локальные виртуальные машины. (За исключением тестовой отработки отказа, при которой основной сайт должен работать).
2. Далее инициируется отработка отказа всех виртуальных машин в плане восстановления в параллельном режиме. На шаге отработки отказа с помощью реплицированных данных подготавливаются диски виртуальных машин.
3. В указанном порядке выполняются группы запуска и запускаются виртуальные машины в каждой группе. Сначала в группе 1, затем в группе 2 и, наконец, в группе 3. При наличии нескольких виртуальных машин в какой-либо из групп (например, для внешнего веб-интерфейса с балансировкой нагрузки) все они загружаются параллельно.

> [!TIP]
> Упорядочивание групп гарантирует, что зависимости между различными уровнями приложения соблюдаются. Распараллеливание, когда оно применимо, улучшает RTO восстановления приложений.

   > [!NOTE]
   > Отработка отказа компьютеров, которые входят в одну группу, будет выполняться в параллельном режиме. Отработка отказа компьютеров, которые входят в разные группы, будет выполняться в порядке указания групп. Только после выполнения отработки отказа и запуска всех компьютеров группы 1 начинается отработка отказа компьютеров группы 2.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Автоматизация большинства задач восстановления для уменьшения RTO

Восстановление больших приложений может быть сложной задачей. Помнить слишком много ручных действий во время аварий сложно, и это может приводить к ошибкам. В некоторых случаях тот, кому нужно активировать отработку отказа, может не знать об особенностях приложения. 

План восстановления позволяет автоматизировать обязательные действия, которые необходимо выполнить на каждом этапе. Это можно сделать с помощью модулей runbook службы автоматизации Azure. Используя модули runbook, можно автоматизировать общие задачи восстановления, такие как в приведенных ниже примерах. Для выполнения задач, которые невозможно автоматизировать, планы восстановления обеспечивают возможность вставить действия, выполняемые вручную.

* **Задачи виртуальной машины Azure после отработки отказа.** Как правило, они необходимы, чтобы подключиться к виртуальной машине. Примеры:
    * создание общедоступного IP-адреса для виртуальной машины после отработки отказа;
    * назначение NSG для сетевого адаптера виртуальной машины, для которой выполнена отработка отказа;
    * добавление подсистемы балансировки нагрузки в группу доступности.
* **Задачи в виртуальной машине после отработки отказа.** Они необходимы для перенастройки приложения таким образом, чтобы оно работало в новой среде. Примеры:
    * изменение строки подключения к базе данных в виртуальной машине;
    * изменение конфигурации или правил веб-сервера.

> [!TIP]
> Имея полный план восстановления для автоматизации задач после восстановления с помощью модулей runbook службы автоматизации, можно обеспечить отработку отказа "одним щелчком" и оптимизировать RTO.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Тестовая отработка отказа для обеспечения готовности к аварии

План восстановления можно использовать для активации отработки отказа или тестовой отработки отказа. Всегда следует выполнить тестовую отработку отказа для приложения, прежде чем выполнять отработку отказа. Тестовая отработка отказа помогает проверить, запустится ли приложение на сайте восстановления. Если при настройке что-то упущено, можно легко активировать очистку и повторить тестовую отработку отказа. Несколько раз выполните тестовую отработку отказа, пока не будете уверены в надежном восстановлении приложения.

![Снимок экрана примера тестового плана восстановления в Site Recovery](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Так как все приложения уникальны, необходимо создать планы восстановления, подходящие для каждого из них. 
>
> Кроме того, в мире динамических центров обработки данных приложения и их зависимости постоянно меняются. Тестируйте отработку отказа приложений один раз в квартал, чтобы проверять актуальность плана восстановления.

## <a name="create-a-recovery-plan"></a>Создайте план восстановления

1. Щелкните **Планы восстановления** > **Создать план восстановления**.
   Укажите имя плана восстановления, а также исходный и целевой серверы. Исходное расположение должно содержать виртуальные машины, на которых настроены отработка отказа и восстановление. Выберите источник и цель в соответствии с виртуальными машинами, которые должны быть частью плана восстановления. 

   |Сценарий                   |Источник               |Цель           |
   |---------------------------|---------------------|-----------------|
   |Из Azure в Azure             |Регион Azure         |Регион Azure     |
   |VMware — Azure            |Сервер конфигурации |Таблицы Azure            |
   |Из диспетчера виртуальных машин в Azure               |Отображаемое имя диспетчера виртуальных машин    |Таблицы Azure            |
   |С сайта Hyper-V в Azure      |Имя сайта Hyper-V    |Таблицы Azure            |
   |Физические компьютеры в Azure |Сервер конфигурации |Таблицы Azure            |
   |Из VMM в VMM                 |Понятное имя VMM    |Отображаемое имя диспетчера виртуальных машин|

   > [!NOTE]
   > План восстановления может содержать виртуальные машины с одинаковыми источником и целью. Виртуальные машины VMware и System Center Virtual Machine Manager не могут входить в один и тот же план восстановления. Однако виртуальные машины VMware и физические компьютеры можно добавить в один план. В этом случае источником для них является сервер конфигурации.

2. В окне **Выбор виртуальных машин** укажите виртуальные машины (или группу репликации), которые хотите добавить в группу по умолчанию (группа 1) в плане восстановления. Для выбора будут доступны только виртуальные машины, которые защищены в источнике (выбранном в плане восстановления) и в цели (выбранной в плане восстановления).

## <a name="customize-and-extend-recovery-plans"></a>Настройка и расширение планов восстановления

Можно настроить и расширить планы восстановления, перейдя в область ресурса плана восстановления Site Recovery. Щелкните вкладку **Настройка**. Можно настроить и расширить планы восстановления, используя следующие параметры:

- **Add new groups** (Добавление новых групп). В группу по умолчанию можно добавить до семи групп плана восстановления. Затем вы можете добавить дополнительные виртуальные машины или группы репликации в эти группы плана восстановления. Новые группы нумеруются в порядке добавления. Каждую виртуальную машину или группу репликации можно включить только в одну группу плана восстановления.
- **Add a manual action** (Добавление ручного действия). Вы можете добавить ручные действия, выполняемые до или после группы плана восстановления. При выполнении план восстановления он останавливается в той точке, куда вы добавили выполняемое вручную действие. При этом появляется диалоговое окно с предложением подтвердить, что это действие выполнено вручную.
- **Добавление сценария**. Вы можете добавить сценарии, выполняемые до или после группы плана восстановления. Каждый новый скрипт добавляет к группе новый набор действий. Например, для группы с именем Group 1 создается набор предварительных действий с именем *Group 1: pre-steps*. Все предварительные действия перечислены в этом наборе. Чтобы добавить сценарий, на основном сайте должен быть развернут сервер VMM. Дополнительные сведения см. в статье [Add a VMM script to a recovery plan](site-recovery-how-to-add-vmmscript.md) (Добавление сценария VMM в план восстановления).
- **Add Azure runbook** (Добавление модулей Runbook Azure). Чтобы расширить план восстановления, вы можете применить модули Runbook Azure. Они позволяют автоматизировать задачи или выполнять пошаговое восстановление. Дополнительные сведения см. в статье [Добавление модулей runbook службы автоматизации Azure в планы восстановления](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Добавление в план сценария, модуля runbook или ручного действия

Когда вы создадите план восстановления и добавите в группу по умолчанию виртуальные машины или группы репликации, в эту группу можно добавить сценарий или ручное действие.

1. Откройте план восстановления.
2. Выберите элемент в списке **Шаг**. Затем выберите **Сценарий** или **Выполняемое вручную действие**.
3. Укажите, следует ли добавить сценарий или действие до или после выбранного элемента. Используя кнопки управления **Move Up** (Переместить выше) и **Move Down** (Переместить ниже), можно изменить положение скрипта.
4. Если вы добавляете скрипт VMM, выберите **Failover to VMM script** (Отработка отказа в скрипт VMM). Для параметра **Путь к скрипту** укажите относительный путь к общей папке. Например, **\RPScripts\RPScript.PS1**.
5. Если вы добавляете модуль Runbook службы автоматизации, укажите учетную запись службы автоматизации, в которой расположен этот модуль Runbook. Затем выберите сценарий модуля Runbook Azure.
6. Выполните отработку отказа плана восстановления, чтобы убедиться в исправной работе сценария.

Параметры сценария или модуля runbook доступны только в приведенных ниже сценариях отработки отказа или восстановления размещения. Ручное действие доступно для отработки отказа и восстановления размещения.


|Сценарий               |Отработка отказа |Восстановление размещения |
|-----------------------|---------|---------|
|Из Azure в Azure         |Модуль Runbook |Модуль Runbook  |
|VMware — Azure        |Модуль Runbook |Нет данных       | 
|VMM в Azure           |Модуль Runbook |Скрипт   |
|С сайта Hyper-V в Azure  |Модуль Runbook |Нет данных       |
|Из VMM в VMM             |Скрипт   |Скрипт   |


## <a name="next-steps"></a>Дополнительная информация

* [Дополнительные сведения](site-recovery-failover.md) о выполнении отработки отказа.  
* Просмотрите это видео, чтобы узнать, как работает план восстановления.
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
