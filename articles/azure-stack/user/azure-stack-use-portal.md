---
title: "Использование портала Azure Stack | Документация Майкрософт"
description: "Узнайте, как открыть и использовать пользовательский портал Azure Stack."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 7c34d7a225be63da95f664525b0366ff89b28838
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-azure-stack-portal"></a>Использование портала Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Как пользователь служб Azure Stack, вы можете с помощью портала Azure Stack подписаться на открытые предложения и использовать службы, которые предоставляются в составе этих предложений. Если вы уже работали с порталом Azure, пользовательский интерфейс будет вам знаком.

## <a name="access-the-portal"></a>Доступ к порталу

Оператор Azure Stack (поставщик службы или администратор в вашей организации) сообщит вам правильный URL-адрес для доступа к порталу. 

- В интегрированной системе URL-адрес зависит от региона оператора и внешнего доменного имени. Он будет иметь формат https://portal. &lt; *регион*&gt;.&lt;*полное_доменное_имя*&gt;.
- При работе с Пакетом средств разработки Azure Stack портал можно найти по адресу https://portal.local.azurestack.external.

![Снимок экрана пользовательского портала Azure Stack](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Настройка панели мониторинга

Панель мониторинга содержит стандартный набор плиток. Вы можете **изменить панель мониторинга** или создать **панель мониторинга**. На панель мониторинга можно легко добавить дополнительные плитки. Например, щелкните **Добавить**, затем правой кнопкой мыши щелкните **Вычисления** и выберите в контекстном меню элемент **Закрепить на панели мониторинга**.

## <a name="create-subscription-and-browse-available-resources"></a>Создание подписки и просмотр доступных ресурсов
 
Если у вас еще нет подписки, сначала ее нужно получить, подписавшись на предложение. После этого вы сможете просматривать доступные ресурсы. Чтобы просматривать и создавать ресурсы, воспользуйтесь любым из следующих методов.

- Щелкните плитку **Marketplace** на панели мониторинга. 
- На плитке **Все ресурсы** щелкните **Создать ресурсы**.
- В левой панели навигации щелкните **Создать**.

## <a name="learn-how-to-use-available-services"></a>Узнайте, как использовать доступные службы

Если вам нужны инструкции по использованию доступных ресурсов, их можно получить несколькими способами.

- Возможно, ваша организация или поставщик услуг поддерживают собственную документацию. Это почти наверняка так, если они предлагают настраиваемые службы или приложения.
- Приложения сторонних разработчиков также комплектуются документацией.
- Для согласованных с Azure служб настоятельно рекомендуем изучить документацию по Azure Stack. Чтобы получить доступ к пользовательской документации Azure Stack, щелкните значок справки и выберите элемент **Справка и поддержка**.
 
    ![Снимок экрана с открытым элементом "Справка и поддержка" в пользовательском интерфейсе](media/azure-stack-use-portal/HelpAndSupport.png)

    Рекомендуем перед началом работы изучить следующие статьи.

    - [Key considerations: Using services or building apps for Azure Stack](azure-stack-considerations.md) (Важные аспекты использования служб и создания приложений в Azure Stack)
    - В разделе документации "Использование служб" вы найдете список всех служб, аналогичных службам Azure. Для каждой службы предлагается статья с "рекомендациями", в которой описаны различия между службой в Azure и ее аналогом в Azure Stack. Например, см. [статью с рекомендациями по виртуальным машинам](azure-stack-vm-considerations.md). Также в разделе "Использование служб" вы найдете дополнительные сведения, которые относятся исключительно к Azure Stack. 
     
      Документацию по Azure можно использовать как источник общих сведений о службе, но всегда нужно учитывать существующие различия. Не забывайте, что ссылки на документацию, размещенные на плитке **Краткие руководства**, ведут к документации по Azure.

## <a name="get-support"></a>Получение поддержки

Если вам нужна дополнительная помощь, обратитесь к поставщику услуг или ответственному лицу в вашей организации. 

Если вы используете Пакет средств разработки Azure Stack, поддержку вы сможете получить только на [форуме по Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack).

## <a name="next-steps"></a>Дополнительная информация

[Key considerations: Using services or building apps for Azure Stack](azure-stack-considerations.md) (Важные аспекты использования служб и создания приложений в Azure Stack)
