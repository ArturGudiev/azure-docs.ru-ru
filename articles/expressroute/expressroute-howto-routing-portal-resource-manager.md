---
title: "Как настроить маршрутизацию (пиринг) для канала ExpressRoute в Azure (модель Resource Manager) | Документация Майкрософт"
description: "В этой статье описана процедура создания и подготовки частного пиринга, общедоступного пиринга и пиринга Microsoft для канала ExpressRoute, а также показано, как проверить состояние, обновить или удалить пиринги для канала."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2017
ms.author: cherylmc
ms.openlocfilehash: be25e9ffab4fee79b8d9cc6c88c6ffb3e852af0d
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Создание и изменение пиринга для канала ExpressRoute

В этой статье описано, как создать и администрировать конфигурацию маршрутизации ExpressRoute в модели развертывания Azure Resource Manager, используя портал Azure. Вы также сможете проверить состояние, обновить, удалить и отозвать пиринги для канала ExpressRoute. Если вы хотите использовать для работы с каналом другой метод, выберите подходящую статью из списка ниже.


## <a name="configuration-prerequisites"></a>Предварительные требования для настройки

* Прежде чем приступать к настройке, обязательно изучите [предварительные требования](expressroute-prerequisites.md), [требования к маршрутизации](expressroute-routing.md) и [рабочие процессы](expressroute-workflows.md).
* Вам потребуется активный канал ExpressRoute. Приступая к работе, [создайте канал ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) ; он должен быть затем включен на стороне поставщика услуг подключения. Для выполнения командлетов, описанных в следующих разделах, нужно подготовить и включить канал ExpressRoute.
* Если вы планируете использовать общий ключ или хэш MD5, используйте его на обеих сторонах туннеля и ограничьте максимальное число символов до 25.

Эти инструкции распространяются только на каналы от поставщиков, предоставляющих услуги подключения второго уровня. Если ваш поставщик услуг подключения предлагает услуги третьего уровня (обычно это IPVPN, например MPLS), он выполнит настройку маршрутизации и управление ею. 

> [!IMPORTANT]
> Мы пока не предлагаем использовать пиринги, которые настроены поставщиками услуг на портале управления службами. Такая возможность будет предоставлена позже. Перед настройкой пирингов BGP уточните информацию у поставщика услуг.
> 
> 

Для каждого канала ExpressRoute можно настроить один, два или три пиринга (частный пиринг Azure, общедоступный пиринг Azure и пиринг Microsoft). Пиринги можно настраивать в любом порядке, главное, выполнять их конфигурацию по очереди. Дополнительную информацию о доменах маршрутизации и пиринге см. в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).

## <a name="msft"></a>Пиринг Майкрософт

Этот раздел поможет создать, получить, обновить и (или) удалить конфигурацию пиринга Майкрософт для канала ExpressRoute.

> [!IMPORTANT]
> Пиринг Майкрософт для каналов ExpressRoute, которые были настроены до 1 августа 2017 г., позволяет объявлять все префиксы служб, даже если фильтры маршрутов не определены. Пиринг Майкрософт для каналов ExpressRoute, настроенных 1 августа 2017 г. или позднее, не будет объявлять префиксы, пока к каналу не будет присоединен фильтр маршрутов. Дополнительные сведения см. в руководстве по [настройке фильтра маршрута для пиринга Майкрософт](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Создание пиринга Майкрософт

[!INCLUDE [Premium](../../includes/expressroute-mspeering-premium-include.md)]

1. Настройте канал ExpressRoute. Прежде чем продолжить, убедитесь, что канал полностью подготовлен поставщиком услуг подключения. Если поставщик услуг подключения оказывает услуги третьего уровня, он может включить для вас пиринг Майкрософт. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала.

  ![Отображение пиринга Майкрософт](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Настройте пиринг Майкрософт для этого канала. Перед началом работы убедитесь, что у вас есть следующие сведения.

  * Подсеть /30 для основной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4, принадлежащего вам и зарегистрированного в RIR/IRR.
  * Подсеть /30 для дополнительной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4, принадлежащего вам и зарегистрированного в RIR/IRR.
  * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
  * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS.
  * Объявленные префиксы: необходимо предоставить список всех префиксов, которые вы планируете объявить во время сеанса BGP. Допускаются только общедоступные префиксы IP-адресов. Если вы планируете отправить набор префиксов, их можно оформить в виде списка, разделенного запятыми. Эти префиксы должны быть зарегистрированы в RIR/IRR на ваше имя.
  * **Необязательно.** ASN клиента: для объявления префиксов, не зарегистрированных с номером AS для пиринга, можно указать номер AS, с которым они зарегистрированы.
  * Имя реестра маршрутизации: можно указать RIR/IRR, в котором зарегистрированы номер AS и префиксы.
  * **Необязательно.** Хэш MD5, если вы решите его использовать.
3. Вы можете выбрать пиринг, который необходимо настроить, как показано в следующем примере. Выберите строку пиринга Майкрософт.

  ![Выбор строки пиринга Майкрософт](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Настройте пиринг Майкрософт. На следующем изображении показан пример настройки:

  ![Настройка пиринга Майкрософт](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Указав все параметры, сохраните конфигурацию.

  Если для канала будет установлено состояние "Требуется проверка" (как показано на изображении), следует создать запрос в службу поддержки и предоставить сотрудникам поддержки доказательство того, что вы владеете префиксами.

  ![Сохранение настройки пиринга Майкрософт](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Запрос в службу поддержки можно создать непосредственно на портале, как показано в следующем примере:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. После подтверждения настройки отобразится примерно такой результат:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="getmsft"></a>Просмотр сведений о пиринге Майкрософт

Чтобы посмотреть свойства общедоступного пиринга Azure, выберите нужный пиринг.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="updatemsft"></a>Обновление конфигурации пиринга Майкрософт

Щелкнув строку пиринга, вы сможете изменить свойства пиринга.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="deletemsft"></a>Удаление пиринга Майкрософт

Конфигурацию пиринга можно удалить, щелкнув значок "Удалить", как показано на следующем изображении:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="private"></a>Частный пиринг Azure

Этот раздел поможет вам создать, получить, обновить и (или) удалить конфигурацию частного пиринга Azure для канала ExpressRoute.

### <a name="to-create-azure-private-peering"></a>Создание частного пиринга Azure

1. Настройте канал ExpressRoute. Прежде чем продолжить, убедитесь, что канал полностью подготовлен поставщиком услуг подключения. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить для вас частный пиринг Azure. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала.

  ![list](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Настройте для канала частный пиринг Azure. Прежде чем продолжить, убедитесь в наличии следующих элементов:

  * Подсеть /30 для основной ссылки. Эта подсеть не должна входить в адресное пространство, зарезервированное для виртуальных сетей.
  * Подсеть /30 для дополнительной ссылки. Эта подсеть не должна входить в адресное пространство, зарезервированное для виртуальных сетей.
  * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
  * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS. Для этого пиринга можно использовать частный номер AS. Не используйте номер 65515.
  * **Необязательно.** Хэш MD5, если вы решите его использовать.
3. Выберите в таблице пирингов нужный частный пиринг Azure, как показано в следующем примере:

  ![Частный пиринг](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Настройте частный пиринг. На следующем изображении показан пример настройки:

  ![Настройка частного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Указав все параметры, сохраните конфигурацию. После подтверждения настройки отобразится примерно такой результат:

  ![Сохранение частного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="getprivate"></a>Просмотр сведений о частном пиринге Azure

Чтобы посмотреть свойства частного пиринга Azure, выберите нужный пиринг.

![Просмотр частного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="updateprivate"></a>Обновление конфигурации частного пиринга Azure

Щелкнув строку пиринга, вы сможете изменить свойства пиринга.

![Обновление частного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="deleteprivate"></a>Удаление частного пиринга Azure

Конфигурацию пиринга можно удалить, щелкнув значок "Удалить", как показано на следующем изображении:

![Удаление частного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="public"></a>Общедоступный пиринг Azure

Этот раздел поможет создать, получить, обновить и (или) удалить конфигурацию общедоступного пиринга Azure для канала ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Создание общедоступного пиринга Azure

1. Настройте канал ExpressRoute. Прежде чем продолжить, убедитесь, что канал полностью подготовлен поставщиком услуг подключения. Если поставщик услуг подключения предоставляет управляемые службы уровня 3, он может включить для вас общедоступный пиринг Azure. В этом случае инструкции в следующих разделах выполнять не нужно. Если же поставщик услуг подключения не берет на себя управление маршрутизацией, выполните приведенные ниже инструкции для настройки конфигурации после создания канала.

  ![Отображение общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Настройте для канала общедоступный пиринг Azure. Прежде чем продолжить, убедитесь в наличии следующих элементов:

  * Подсеть /30 для основной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4.
  * Подсеть /30 для дополнительной ссылки. Это должен быть допустимый префикс общедоступного адреса IPv4.
  * Действительный идентификатор виртуальной локальной сети для установки пиринга. Идентификатор не должен использоваться ни одним другим пирингом в канале.
  * Номер AS для пиринга. Можно использовать 2-байтовые и 4-байтовые номера AS.
  * **Необязательно.** Хэш MD5, если вы решите его использовать.
3. Выберите в таблице пирингов нужный общедоступный пиринг Azure, как показано на следующем изображении:

  ![Выбор строки общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Настройте общедоступный пиринг. На следующем изображении показан пример настройки:

  ![Настройка общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Указав все параметры, сохраните конфигурацию. После подтверждения настройки отобразится примерно такой результат:

  ![Сохранение настройки общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="getpublic"></a>Просмотр сведений об общедоступном пиринге Azure

Чтобы посмотреть свойства общедоступного пиринга Azure, выберите нужный пиринг.

![Просмотр свойств общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="updatepublic"></a>Обновление конфигурации общедоступного пиринга Azure

Щелкнув строку пиринга, вы сможете изменить свойства пиринга.

![Выбор строки общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="deletepublic"></a>Удаление общедоступного пиринга Azure

Настройку пиринга можно удалить, щелкнув значок "Удалить", как показано в следующем примере:

![Удаление общедоступного пиринга](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="next-steps"></a>Дополнительная информация

Следующий шаг — [связывание виртуальной сети с каналом ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md).
* Дополнительную информацию о рабочих процессах ExpressRoute см. в статье [Процедуры ExpressRoute для подготовки каналов и состояний каналов](expressroute-workflows.md).
* Дополнительную информацию о пиринге канала см. в статье [Каналы ExpressRoute и домены маршрутизации](expressroute-circuit-peerings.md).
* Подробнее о работе с виртуальными сетями см. в статье [Обзор виртуальных сетей](../virtual-network/virtual-networks-overview.md).