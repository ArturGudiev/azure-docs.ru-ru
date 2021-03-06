---
title: "Использование Azure DNS с другими службами Azure | Документация Майкрософт"
description: "Узнайте, как использовать Azure DNS для разрешения имен в других службах Azure."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: kumud
ms.openlocfilehash: 6d052bc82c35aa3f2fdf5b5820e3901bd5c4080d
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2017
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Как Azure DNS работает с другими службами Azure

Azure DNS является размещенной службой управления и разрешения имен (DNS). Это позволяет создавать общедоступные DNS-имена для других приложений и служб, развертываемых в Azure. Создать имя для службы Azure в личном домене не сложнее, чем добавить запись правильного типа для этой службы.

* Для динамически выделенных IP-адресов необходимо создать запись CNAME DNS, которая сопоставляет DNS-имя, созданное Azure, с вашей службой. Стандарты DNS не позволяют использовать запись CNAME для вершины зоны.
* Для статически выделенных IP-адресов можно создать запись A DNS с использованием любого имени, включая имя *незащищенного домена* на вершине зоны.

В следующей таблице приведены поддерживаемые типы записей, которые могут использоваться для различных служб Azure. Как видно из этой таблицы, Azure DNS поддерживает записи DNS только для сетевых ресурсов с выходом в Интернет. Azure DNS невозможно использовать для разрешения имен внутренних, частных адресов.

| Служба Azure | Сетевой интерфейс | ОПИСАНИЕ |
| --- | --- | --- |
| Шлюз приложений |[Общедоступный IP-адрес внешнего интерфейса](dns-custom-domain.md#public-ip-address) |Можно создать запись A или CNAME в DNS. |
| Подсистема балансировки нагрузки |[Общедоступный IP-адрес внешнего интерфейса](dns-custom-domain.md#public-ip-address)  |Можно создать запись A или CNAME в DNS. Балансировщик нагрузки может иметь общедоступный IP-адрес IPv6, назначаемый динамически. Поэтому необходимо создать запись CNAME для IPv6-адреса. |
| Диспетчер трафика |Общедоступное имя |Можно создать только запись CNAME, которая сопоставляется с именем trafficmanager.net, назначенным профилю диспетчера трафика. Дополнительные сведения см. в статье [Как работает диспетчер трафика](../traffic-manager/traffic-manager-overview.md#traffic-manager-example). |
| Облачная служба |[Общедоступный IP-адрес](dns-custom-domain.md#public-ip-address) |Для статически выделенных IP-адресов можно создать запись A DNS. Для динамически выделенных IP-адресов необходимо создать запись CNAME, которая сопоставляется с именем *cloudapp.net*.|
| Служба приложений | [Внешний IP-адрес](dns-custom-domain.md#app-service-web-apps) |Для внешних IP-адресов можно создать запись A DNS. В противном случае необходимо создать запись CNAME, которая сопоставляется с именем azurewebsites.net. Дополнительные сведения см. в статье [Сопоставление имени личного домена с приложением Azure](../app-service/app-service-web-tutorial-custom-domain.md). |
| Виртуальные машины Resource Manager |[Общедоступный IP-адрес](dns-custom-domain.md#public-ip-address) |У виртуальных машин Resource Manager могут быть общедоступные IP-адреса. Кроме того, виртуальная машина с общедоступным IP-адресом может обслуживаться балансировщиком нагрузки. Для общедоступного адреса можно создать запись A или CNAME в DNS. Это пользовательское имя может использоваться для обхода виртуального IP-адреса в балансировщике нагрузки. |
| классические виртуальные машины; |[Общедоступный IP-адрес](dns-custom-domain.md#public-ip-address) |Для классических виртуальных машин, созданных с помощью PowerShell или интерфейса командной строки, можно настроить динамический или статический (зарезервированный) виртуальный адрес. Для них можно создать запись CNAME или A в DNS, соответственно. |
