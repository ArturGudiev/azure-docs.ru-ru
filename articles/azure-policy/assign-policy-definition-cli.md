---
title: "Создание назначения политики с помощью Azure CLI для идентификации ресурсов, не соответствующих требованиям, в среде Azure | Документация Майкрософт"
description: "Использование PowerShell для создания назначения политики Azure для идентификации несоответствующих ресурсов."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 76725f3ebeaf5af4f2ab8aadb303d862fa111ecb
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Создание назначения политики для идентификации ресурсов, не соответствующих требованиям, в среде Azure с помощью Azure CLI | Документация Майкрософт

Чтобы понять, соответствуют ли ресурсы требованиям в Azure, прежде всего нужно определить их состояние. В этом кратком руководстве описывается поэтапный процесс создания назначения политики для выявления виртуальных машин, которые не используют управляемые диски.

Этот процесс позволит вам определить, какие виртуальные машины не используют управляемые диски. Они *не соответствуют* назначению политики.

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Для этого руководства вам потребуется Azure CLI 2.0.4 или более поздней версии, чтобы установить и использовать интерфейс командной строки локально. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-policy-assignment"></a>Создание назначения политики

В этом кратком руководстве мы создаем назначение политики и назначаем определение "Audit Virtual Machines without Managed Disks". Это определение политики идентифицирует ресурсы, которые не соответствуют условиям, заданным в определении политики.

Чтобы создать назначение политики, следуйте инструкциям ниже.

1. Зарегистрируйте поставщик ресурсов Policy Insights, чтобы связать подписку с поставщиком ресурсов. Чтобы зарегистрировать поставщик ресурсов, необходимо иметь разрешение на действие регистрации для поставщика ресурсов. Эта операция включается в роли участника и владельца.

    Зарегистрируйте поставщик ресурсов с помощью следующей команды:

    ```azurecli
    az provider register --namespace Microsoft.PolicyInsights
    ```

    Эта команда возвращает сообщение о начале выполнения регистрации.

    Невозможно отменить регистрацию поставщика ресурсов, так как в подписке содержатся типы ресурсов, связанные с этим поставщиком ресурсов. Дополнительные сведения о регистрации и просмотре поставщиков ресурсов см. в статье [Поставщики и типы ресурсов](../azure-resource-manager/resource-manager-supported-services.md).

2. Просмотрите все определения политик и найдите определение политики *аудита виртуальных машин без управляемых дисков*.

    ```azurecli
az policy definition list
```

    Политика Azure поставляется с готовыми встроенными определениями политик, которые можно использовать. Вы увидите такие встроенные определения политик, как:

    - принудительно применять тег и его значение;
    - применять тег и его значение;
    - Требовать наличия SQL Server версии 12.0.

3. Далее укажите следующие сведения и выполните следующую команду, чтобы назначить определение политики:

    - Отображаемое имя (**Name**) назначения политики. В этом случае используем *Audit Virtual Machines without Managed Disks*.
    - Определение политики (**Policy**), на основе которой вы создаете назначение. В данном случае это определение политики — *Audit Virtual Machines without Managed Disks*.
    - Область (**Scope**) определяет, к каким ресурсам или группе ресурсов принудительно применяется назначение политики. Политика может назначаться разным ресурсам: от подписки до групп ресурсов.

    Использование ранее зарегистрированной подписки (или группы ресурсов). В этом примере мы используем идентификатор подписки **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** и имя группы ресурсов **FabrikamOMS**. Не забудьте изменить их на идентификатор подписки и имя группы ресурсов, с которыми вы работаете.

    Команда должна выглядеть примерно так:

    ```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Назначение политики — это политика, которая назначена в рамках определенной области. Эта область также может варьироваться от группы управления до группы ресурсов.

## <a name="identify-non-compliant-resources"></a>Идентификация несоответствующих ресурсов

Чтобы просмотреть ресурсы, которые не соответствуют новому назначению, выполните следующие действия.

1. Вернитесь на страницу политики Azure.
2. Выберите **Соответствие** в области слева и выполните поиск созданного **назначения политики**.

   ![Соответствие политике](media/assign-policy-definition/policy-compliance.png)

   Существующие ресурсы, которые не соответствуют новому назначению, отображаются на вкладке **Несоответствующие ресурсы**. На изображении выше показаны примеры несоответствующих ресурсов.

## <a name="clean-up-resources"></a>Очистка ресурсов

Остальные руководства из этой серии являются продолжением этого документа. Если вы намерены переходить к ним, не удаляйте ресурсы, которые создали при работе с этим руководством. В противном случае удалите созданное назначение, выполнив следующую команду:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы назначили определение политики для идентификации ресурсов, не соответствующих требованиям, в среде Azure.

Дополнительные сведения о назначении политик, для обеспечения соответствия создаваемых в **будущем** ресурсов см. в следующем руководстве:

> [!div class="nextstepaction"]
> [Создание политик и управление ими](./create-manage-policy.md)
