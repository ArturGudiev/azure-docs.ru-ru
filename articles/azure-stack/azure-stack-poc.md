---
title: "Что такое Azure Stack? | Документация Майкрософт"
description: "Azure Stack позволяет запускать службы Azure в вашем центре обработки данных."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 68d1e1752f934e61bbb60c0c934a80b564896a36
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/24/2018
---
# <a name="what-is-azure-stack"></a>Что такое Azure Stack?

Microsoft Azure Stack ― это гибридная облачная платформа, позволяющая доставлять службы Azure из центра обработки данных организации.  Платформа Azure Stack предназначена для поддержки новых сценариев для современных приложений в ключевых сценариях, таких как пограничные и отключенные среды, или для соответствия определенным требованиям безопасности и законодательства.  Azure Stack предлагается в двух вариантах развертывания для соответствия вашим потребностям.

## <a name="azure-stack-integrated-systems"></a>Интегрированные системы Azure Stack
Интегрированные системы Azure Stack предоставляются благодаря партнерским отношениям Майкрософт и [партнеров по оборудованию](https://azure.microsoft.com/overview/azure-stack/integrated-systems/). Это позволяет создать решение с инновациями облачного уровня, уравновешенными простотой управления.  Так как Azure Stack предлагается в форме интегрированной программно-аппаратной системы, вам предоставляется необходимый уровень гибкости и контроля, позволяющий при этом адаптировать инновации из облачной среды.  Интегрированные системы Azure Stack, размер которых варьируется от 4 до 12 узлов, совместно поддерживаются партнером по оборудованию и корпорацией Майкрософт.  Использование интегрированных систем Azure Stack позволяет реализовать новые сценарии для производственных рабочих нагрузок.    

## <a name="azure-stack-development-kit"></a>Пакет средств разработки Azure Stack
Пакет средств разработки Azure Stack — это развертывание Azure Stack с одним узлом, удобное для оценки и изучения Azure Stack.  Также Пакет средств разработки Azure Stack можно использовать как среду разработки, в которой можно разрабатывать решения с помощью API-интерфейсов и инструментов, совместимых с Azure.  Пакет средств разработки Azure Stack не предназначен для использования в качестве производственной среды.

Пакет средств разработки Azure Stack имеет следующие ограничения.
* Пакет средств разработки Azure Stack связан с одним поставщиком удостоверений Azure Active Directory или служб федерации Active Directory. В этом каталоге можно создать несколько пользователей и назначить им подписки.
* Так как все компоненты развернуты на одном компьютере, для ресурсов клиента доступны ограниченные физические ресурсы. Эта конфигурация не предназначена для оценки масштабируемости или производительности.
* Также ограничено применение в сетевых сценариях, что обусловлено обязательным одним излом или сетевой картой.  

## <a name="next-steps"></a>Дополнительная информация
[Основные возможности и концепции](azure-stack-key-features.md)

[Azure Stack: An extension of Azure](https://azure.microsoft.com/en-us/resources/azure-stack-an-extension-of-azure/) (Azure Stack: расширение Azure (PDF))

