---
title: "Развертывание рабочих ролей в службе приложений — Azure Stack | Документация Майкрософт"
description: "Подробные инструкции по масштабированию службы приложений Microsoft Azure Stack"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: brenduns
ms.reviewer: anwestg
ms.openlocfilehash: ddd9820715e964218db8b88fb5211b3725c808b9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>Служба приложений в Azure Stack: добавление рабочих ролей и ролей инфраструктуры
*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*  

Этот документ содержит инструкции по масштабированию службы приложений для рабочих ролей и ролей инфраструктуры Azure Stack. Здесь вы найдете указания по созданию дополнительных рабочих ролей, что позволяет поддерживать приложения любого размера.

> [!NOTE]
> Если в вашей среде Azure Stack объем памяти не превышает 96 ГБ, вы можете столкнуться с затруднениями при добавлении дополнительной емкости.

По умолчанию служба приложений в Azure Stack поддерживает бесплатные и общие рабочие уровни. Чтобы добавить другие рабочие уровни, вам потребуется больше рабочих ролей.

Если вы не уверены, какие компоненты были развернуты вместе со службой приложений при установке Azure Stack, дополнительные сведения можно найти в статье [Обзор службы приложений в Azure Stack](azure-stack-app-service-overview.md).

Служба приложений Azure в Azure Stack развертывает все роли в масштабируемых наборах виртуальных машин, что обеспечивает дополнительные возможности масштабирования. Чтобы использовать эти возможности, все операции масштабирования рабочих уровней выполняются с правами администратора службы приложений.

Дополнительные рабочие роли можно добавить непосредственно в роли администратора поставщика ресурсов в службе приложений.

1. Войдите на портал администрирования Azure Stack с правами администратора службы.

2. Перейдите к **службе приложений**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Щелкните **Роли**. Здесь подробно представлены все развертываемые роли службы приложений.

4. Щелкните правой кнопкой мыши строку для типа роли, которую нужно масштабировать, и выберите **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Щелкните **Масштабирование**, выберите число экземпляров для масштабирования и нажмите кнопку **Сохранить**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Служба приложений Azure Stack добавит и настроит виртуальные машины, установит все необходимое программное обеспечение и отметит рабочие роли как готовые к использованию, когда процесс подготовки завершится. Весь процесс может занять около 80 минут.

7. Ход подготовки новых ролей можно отслеживать в списке рабочих ролей в колонке **Роли**.

Когда рабочие роли будут полностью развернуты и готовы к использованию, пользователи смогут размещать в них свои рабочие нагрузки. В следующем примере представлено несколько ценовых категорий, доступных по умолчанию. Если для определенного рабочего уровня нет доступных рабочих процессов, вы не сможете выбрать соответствующую ценовую категорию.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Чтобы развернуть роли управления, внешнего интерфейса или издателя, нужно развернуть соответствующий масштабируемый набор виртуальных машин. В последующих выпусках мы добавим возможность развертывать эти роли при помощи функций управления в службе приложений.

Чтобы масштабировать роли управления, интерфейса или издателя, выполните те же действия, выбирая соответствующий тип роли. Контроллеры не развертываются как наборы масштабирования. Поэтому во время установки для всех рабочих развертываний следует развернуть два контроллера.

### <a name="next-steps"></a>Дополнительная информация

[Настройка источников развертывания](azure-stack-app-service-configure-deployment-sources.md)
