---
title: "Как назначить доступ на основе MSI к ресурсу Azure с помощью PowerShell"
description: "Пошаговые инструкции по назначению MSI для одного ресурса и предоставлению доступа другому ресурсу с помощью PowerShell."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c9f146e829e463fbdaf1bcff8747ffc5dd055ecb
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2018
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Назначение доступа на основе управляемого удостоверения службы (MSI) для ресурса с помощью PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

После настройки MSI для ресурса Azure можно предоставить доступ на основе MSI другому ресурсу, как и любому субъекту безопасности. В этом примере показано, как предоставить виртуальной машине Azure доступ на основе MSI к учетной записи хранения Azure с помощью PowerShell.

## <a name="prerequisites"></a>предварительным требованиям

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Установите [Azure PowerShell версии 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1), если еще не сделали этого.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Использование RBAC для назначения доступа на основе MSI другому ресурсу

После включения MSI для ресурса Azure, [например виртуальной машины Azure](msi-qs-configure-powershell-windows-vm.md):

1. Войдите в Azure с помощью командлета `Login-AzureRmAccount`. Используйте учетную запись, связанную с подпиской Azure, с помощью которой вы настроили MSI.

   ```powershell
   Login-AzureRmAccount
   ```
2. В этом примере мы предоставляем виртуальной машине Azure доступ к учетной записи хранения. Сначала с помощью командлета [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) мы получаем субъект-службу для виртуальной машины myVM, который был создан при включении MSI. Затем с помощью командлета [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) мы предоставляем этой виртуальной машине доступ для чтения к учетной записи хранения myStorageAcct.

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Устранение неполадок

Если MSI для ресурса не отображается в списке доступных удостоверений, убедитесь, что вы правильно включили MSI. В нашем случае мы можем вернуться к виртуальной машине Azure на [портале Azure](https://portal.azure.com) и:

- просмотреть страницу "Конфигурация" и убедиться, что указан параметр "MSI enabled" = "Yes";
- просмотреть страницу "Расширения" и убедиться, что расширение MSI успешно развернуто.

Если какое-либо из условий не выполнено, может потребоваться повторно развернуть MSI для ресурса или устранить неполадки развертывания.

## <a name="related-content"></a>Связанная информация

- Общие сведения об MSI см. в разделе [Управляемое удостоверение службы (MSI) для Azure Active Directory](msi-overview.md).
- О том, как включить MSI для виртуальной машины Azure, можно узнать в разделе [Настройка управляемого удостоверения службы (MSI) на виртуальной машине Azure с помощью PowerShell](msi-qs-configure-powershell-windows-vm.md).

Оставляйте свои замечания и пожелания в разделе ниже. Они помогают нам улучшать содержимое веб-сайта.

