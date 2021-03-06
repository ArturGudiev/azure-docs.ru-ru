---
title: "Создание, изменение или удаление общедоступного IP-адреса Azure | Документация Майкрософт"
description: "Сведения о создании, изменении и удалении общедоступного IP-адреса."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 8efc0bff4764a7265a5f1bcdd995979af0b22234
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2018
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Создание, изменение и удаление общедоступного IP-адреса

Общие сведения об общедоступном IP-адресе, а также о его создании, изменении и удалении. Общедоступный IP-адрес — это ресурс с собственными настраиваемыми параметрами. Назначение общедоступного IP-адреса другим ресурсам Azure связано со следующими преимуществами.
- Возможность входящего подключения к таким интернет-ресурсам, как виртуальные машины Azure, масштабируемые наборы виртуальных машин Azure, VPN-шлюз Azure, шлюзы приложений и Azure Load Balancer с доступом к Интернету. Ресурсы Azure не могут принимать входящий трафик из Интернета без назначенного общедоступного IP-адреса. Хотя к некоторым ресурсам Azure можно получить доступ через общедоступные IP-адреса, другим ресурсам нужно назначить общедоступный IP-адрес для этого.
- Исходящие подключения к Интернету используют предсказуемый IP-адрес. Например, виртуальная машина может отправлять данные в Интернет, не имея назначенного общедоступного IP-адреса. Ее адресом будет сетевой адрес, преобразуемый Azure в непредсказуемый общедоступный адрес. Назначение ресурсу общедоступного IP-адреса позволяет узнать, какой IP-адрес используется для исходящих соединений. Хоть адрес и предсказуемый, он может измениться в зависимости от выбранного метода назначения. Дополнительные сведения см. в разделе [Создание общедоступного IP-адреса](#create-a-public-ip-address). См. дополнительную информацию об [исходящих подключениях](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ресурсов Azure.

## <a name="before-you-begin"></a>Перед началом работы

Перед выполнением действий, описанных в любом разделе этой статьи, выполните следующие задачи.

- Если у вас нет учетной записи Azure, зарегистрируйтесь для получения [бесплатной пробной учетной записи](https://azure.microsoft.com/free).
- При использовании портала Azure перейдите по адресу https://portal.azure.com и войдите с использованием своей учетной записи Azure.
- При использовании команд PowerShell для работы с этой статьей выполняйте их в [Azure Cloud Shell](https://shell.azure.com/powershell) или в PowerShell на своем компьютере. Azure Cloud Shell — это бесплатная интерактивная оболочка, с помощью которой можно выполнять действия, описанные в этой статье. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью. Для работы с этим руководством требуется модуль Azure PowerShell версии не ниже 5.2.0. Выполните командлет `Get-Module -ListAvailable AzureRM`, чтобы узнать установленную версию. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Login-AzureRmAccount`, чтобы создать подключение к Azure.
- При использовании команд интерфейса командной строки Azure (CLI) для работы с этой статьей выполняйте их в [Azure Cloud Shell](https://shell.azure.com/bash) или в интерфейсе командной строки на своем компьютере. Для этого руководства требуется Azure CLI версии не ниже 2.0.26. Выполните командлет `az --version`, чтобы узнать установленную версию. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). Если Azure CLI запущен локально, необходимо также выполнить командлет `az login`, чтобы создать подключение к Azure.

За использование общедоступных IP-адресов взимается номинальная плата. Сведения о расценках см. на странице с [ценами на IP-адреса](https://azure.microsoft.com/pricing/details/ip-addresses). 

## <a name="create-a-public-ip-address"></a>Создание общедоступного IP-адреса

1. В верхней части портала Azure в поле с текстом *Поиск ресурсов* введите *общедоступный IP-адрес*. Когда в результатах поиска появится пункт **Общедоступные IP-адреса**, щелкните его.
2. В открывшейся колонке **Общедоступный IP-адрес** щелкните **+Добавить**.
3. В открывшейся колонке **Создание общедоступного IP-адреса** введите или выберите значения следующих параметров и щелкните **Создать**.

    |Параметр|Обязательный?|Сведения|
    |---|---|---|
    |SKU|Yes|До появления номеров SKU все общедоступные IP-адреса имеют номер SKU категории **Базовый**.  Номер SKU нельзя изменить после создания общедоступного IP-адреса. Изолированные виртуальные машины, виртуальные машины в группе доступности или масштабируемые наборы виртуальных машин могут использовать номера SKU категории "Базовый" или "Стандартный".  Совмещение номеров SKU между виртуальными машинами в группе доступности или масштабируемом наборе не допускается. Номер SKU **Базовый**. При создании общедоступного IP-адреса в регионе, который поддерживает зоны доступности, параметр **Availability zone** (Зона доступности) принимает значение *Нет* по умолчанию. Вы можете выбрать зону доступности, чтобы закрепить определенную зону за общедоступным IP-адресом. Номер SKU **Стандартный**. Общедоступный IP-адрес стандартного номера SKU может быть связан с виртуальной машиной или интерфейсной частью подсистемы балансировки нагрузки. При создании общедоступного IP-адреса в регионе, который поддерживает зоны доступности, параметр **Availability zone** (Зона доступности) принимает значение *Избыточное в пределах зоны* по умолчанию. Дополнительные сведения о зонах доступности см. в параметре **Зона доступности**. Стандартный номер SKU необходим при связывании адреса с подсистемой балансировки нагрузки категории "Стандартный". Дополнительные сведения о подсистемах балансировки нагрузки см. в статье [Azure Load Balancer Standard overview (Preview)](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Обзор Azure Load Balancer категории "Стандартный" (предварительная версия)). Стандартные SKU доступны в режиме предварительной версии. Прежде чем создавать общедоступный IP-адрес стандартного SKU, необходимо завершить шаги в [разделе о регистрации для получения предварительной версии стандартных SKU](#register-for-the-standard-sku-preview) и создать общедоступный IP-адрес в поддерживаемом расположении (регионе). Список поддерживаемых расположений см. в разделе [Доступность регионов](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) и на странице [Обновления виртуальной сети Azure](https://azure.microsoft.com/updates/?product=virtual-network) (дополнительные сведения о поддержке регионов). При назначении сетевому интерфейсу виртуальной машины общедоступного IP-адреса на основе номера SKU категории "Стандартный" необходимо явно разрешить предполагаемый трафик с помощью [группы безопасности сети](security-overview.md#network-security-groups). Обмен данными с ресурсом будет невозможен, пока вы не создадите и не свяжете группу безопасности сети и явно не разрешите нужный трафик.|
    |ИМЯ|Yes|Имя должно быть уникальным в пределах выбранной группы ресурсов.|
    |Версия IP-адреса|Yes| Выберите IPv4 или IPv6. Общедоступные IPv4-адреса можно назначить нескольким ресурсам Azure, а общедоступный IPv6-адрес — только балансировщику нагрузки с доступом к Интернету. Балансировщик нагрузки может выполнять балансировку нагрузки трафика IPv6 на виртуальных машинах Azure. Дополнительные сведения о балансировке нагрузки IPv6-трафика к виртуальным машинам см. в статье [Общие сведения о поддержке IPv6 для Azure Load Balancer](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). При выборе **Standard SKU** (Номер SKU "Стандартный") у вас нет возможности выбрать *IPv6*. При использовании **Standard SKU** (Номер SKU "Стандартный") можно создать только IPv4-адрес.|
    |Назначение IP-адресов|Yes|**Динамический.** Динамические адреса присваиваются только после связывания общедоступных IP-адресов с сетевым интерфейсом, подключенным к виртуальной машине, а виртуальная машина запускается впервые. Динамические адреса могут измениться, если виртуальная машина, к которой присоединен сетевой интерфейс, остановлена (освобождена). Если виртуальная машина перезагружается или остановлена (не освобождается), адрес не изменится. **Статическое** — статические адреса назначаются при создании общедоступного IP-адреса. Статические адреса не изменяются, даже если виртуальная машина переводится в остановленное состояние (освобождается). Адрес будет освобожден только при удалении сетевого интерфейса. Метод назначения можно изменить после создания сетевого интерфейса. При выборе *IPv6* в качестве **версии IP** метод назначения — *динамический*. При выборе *Стандартный* для **номера SKU** метод назначения — *статический*.|
    |Время ожидания простоя (в минутах)|Нет |Время (в минутах), в течение которого подключение TCP или HTTP остается открытым без привязки к клиентам при отправке запросов для проверки активности. При выборе IPv6 в качестве **версии IP** это значение невозможно изменить. |
    |Метка имени DNS|Нет |Должна быть уникальной в пределах расположения Azure, в котором создается имя (для всех подписок и клиентов). Azure автоматически регистрирует имя и IP-адрес в своей DNS (вы можете подключаться к ресурсу, используя это имя). Azure добавляет подсеть по умолчанию, например *location.cloudapp.azure.com* (где location — это выбранное вами расположение) к имени, которое вы предоставили, для создания полного имени DNS. Если вы решили создать обе версии адресов, им назначается одинаковое имя DNS. Служба Azure DNS по умолчанию содержит обе записи имен IPv4 (A) и IPv6 (AAAA) и отвечает на поиск имени DNS, используя обе записи. Клиент выбирает, с каким адресом (IPv4 или IPv6) взаимодействовать. Вместо или в дополнение к использованию метки DNS-имени с суффиксом по умолчанию можно использовать службу Azure DNS для настройки DNS-имени с пользовательским суффиксом, которое преобразуется в общедоступный IP-адрес. Дополнительные сведения см. в разделе [Общедоступный IP-адрес](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
    |"Create an IPv6 (or IPv4) address" (Создание IPv6- или IPv4-адреса)|Нет | То, какой из адресов отображается (IPv6 и IPv4) зависит от выбора **версии IP**. Например, при выборе **IPv4** в качестве **версии IP** здесь отображается **IPv6**. При выборе *Стандартный* для **номера SKU** отсутствует возможность создания IPv6-адреса.
    |Имя (отображается, только если установлен флажок **Create an IPv6 (or IPv4) address** (Создание IPv6- или IPv4-адреса)).|Да, если установлен флажок **Create an IPv6 (or IPv4)** (Создание IPv6 или IPv4).|Имя должно отличаться от того, что вы ввели для первого **имени** в этом списке. Если вы решили создать IPv4- и IPv6-адрес, портал создаст два отдельных ресурса общедоступных IP-адресов, каждому из которых назначена своя версия.|
    |Назначение IP-адресов (отображается, только если установлен флажок **Create an IPv6 (or IPv4) address** (Создание IPv6- или IPv4-адреса))|Да, если установлен флажок **Create an IPv6 (or IPv4)** (Создание IPv6 или IPv4).|Если отображается флажок **Create an IPv4 address** (Создание IPv4-адреса), можно выбрать метод назначения. Если отображается флажок **Create an IPv6 address** (Создание IPv6-адреса), невозможно выбрать метод назначения, так как он должен быть **динамическим**.|
    |Подписка|Yes|Ресурс в пределах одной и той же [подписки](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), с которым будет связан общедоступный IP-адрес.|
    |Группа ресурсов|Yes|Ресурс в пределах одной или разных [групп ресурсов](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), с которым будет связан общедоступный IP-адрес.|
    |Расположение|Yes|Ресурс в пределах одного и того же [расположения](https://azure.microsoft.com/regions), с которым будет связан общедоступный IP-адрес.|
    |Зона доступности| Нет  | Этот параметр отображается только в том случае, если выбрано поддерживаемое расположение. Список поддерживаемых расположений см. в статье [Общие сведения о зонах доступности в Azure (предварительная версия)](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Сейчас зоны доступности находятся в предварительной версии. Перед выбором зоны или избыточного в пределах зоны параметра необходимо выполнить шаги, описанные в разделе [Начало работы с предварительной версией зон доступности](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#get-started-with-the-availability-zones-preview). При выборе номера SKU **Базовый**, значение *Нет* будет выбрано автоматически. Если нужно гарантировать наличие определенной зоны, можно выбрать ее. Варианты не являются избыточными в пределах зоны. При выборе номера SKU **Стандартный** автоматически выбирается избыточность в пределах зоны, благодаря чему путь к данным становится устойчивым к сбоям зоны. Если нужно обеспечить определенную зону, неустойчивую к сбоям зоны, можно выбрать ее.
  

**Команды**

Хотя на портале предоставляется возможность создания двух ресурсов общедоступных IP-адресов (IPv4 и IPv6), следующие команды PowerShell и интерфейса командной строки создают один ресурс с адресом для одной или другой версии IP. Если требуется два ресурса общедоступных IP-адресов (по одному на каждую версию IP), необходимо выполнить команду дважды, указав разные имена и версии для ресурсов общедоступных IP-адресов. 

|Средство|Get-Help|
|---|---|
|Интерфейс командной строки|[az network public-ip create](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_public_ip_create)|
|PowerShell|[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Просмотр, изменение параметров или удаление общедоступного IP-адреса

1. В верхней части портала Azure в поле с текстом *Поиск ресурсов* введите *общедоступный IP-адрес*. Когда в результатах поиска появится пункт **Общедоступные IP-адреса**, щелкните его.
2. В открывшейся колонке **Общедоступный IP-адрес** щелкните имя общедоступного IP-адреса для просмотра, изменения параметров или удаления.
3. В открывшейся колонке "Общедоступный IP-адрес" выполните одно из следующих действий (в зависимости от того, нужно ли просмотреть, удалить или изменить этот общедоступный IP-адрес).
    - **Просмотр**. В разделе **Обзор** колонки отображаются основные параметры общедоступного IP-адреса, например сетевой интерфейс, с которым он связан (если адрес связан с сетевым интерфейсом). На портале не отображается версия адреса (IPv4 или IPv6). Чтобы просмотреть сведения о версии, выполните команду PowerShell или интерфейса командной строки для просмотра общедоступного IP-адреса. Если версия IP-адреса IPv6, назначенный адрес не отображается на портале, в PowerShell или интерфейсе командной строки. 
    - **Удаление**. Чтобы удалить общедоступный IP-адрес, в колонке в разделе **Обзор** щелкните **Удалить**. Если адрес связан с IP-конфигурацией, удалить его не получится. В таком случае щелкните **Отменить связь**, чтобы разорвать связь с IP-конфигурацией.
    - **Изменение**. Щелкните **Конфигурация**. Измените параметры, используя сведения, описанные в шаге 4 в разделе [Создание общедоступного IP-адреса](#create-a-public-ip-address) этой статьи. Чтобы изменить назначение IPv4-адреса со статического на динамическое, необходимо сначала разорвать связь между общедоступным IPv4-адресом и IP-конфигурацией. Затем можно изменить способ назначения на динамическое и щелкнуть **Связать**, чтобы связать IP-адрес с той же или другой IP-конфигурацией либо оставить его несвязанным. Чтобы удалить связь с общедоступным IP-адресом, в разделе **Обзор** щелкните **Отменить связь**.

>[!WARNING]
>Изменив способ назначения со статического на динамический, вы потеряете IP-адрес, который был назначен общедоступному IP-адресу. В Azure общедоступные DNS-серверы поддерживают сопоставление между статическими или динамическими адресами и метками (определенными) имени DNS. При этом динамический IP-адрес можно изменить при запуске виртуальной машины, которая была остановлена (освобождена). Чтобы предотвратить изменение адреса, назначьте статический IP-адрес.

**Команды**

|Средство|Get-Help|
|---|---|
|Интерфейс командной строки|Команда [az network public-ip-list](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_public_ip_list) выводит список общедоступных IP-адресов, [az network public-ip-show](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_public_ip_show) — отображает параметры, [az network public-ip update](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_public_ip_update) — обновляет IP-адрес, а [az network public-ip delete](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_public_ip_delete) — удаляет IP-адрес.|
|PowerShell|Командлет [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) позволяет извлечь объект общедоступного IP-адреса и просмотреть его параметры, [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) позволяет обновить параметры IP-адреса, а [Remove-AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) — удалить IP-адрес.|

## <a name="register-for-the-standard-sku-preview"></a>Регистрация для получения предварительной версии стандартных SKU

> [!NOTE]
> Возможности в предварительном выпуске могут не отличаться таким же уровнем доступности и надежности, как функции, предоставляемые в режиме общедоступной версии. Возможности предварительной версии не поддерживаются, могут иметь ограничения и могут быть доступны не во всех расположениях Azure. 

Прежде чем создавать общедоступный IP-адрес стандартного SKU, необходимо зарегистрироваться для участия в предварительной версии. Чтобы зарегистрироваться для использования предварительной версии, выполните следующие шаги:

1. В PowerShell введите следующую команду, чтобы зарегистрироваться для использования предварительной версии:
   
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
2. Убедитесь, что регистрация прошла успешно, выполнив следующую команду:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

## <a name="next-steps"></a>Дополнительная информация
Назначение общедоступных IP-адресов при создании следующих ресурсов Azure:

- виртуальных машин [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json);
- [Azure Load Balancer с доступом к Интернету](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json);
- [Шлюз приложений Azure](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [подключения типа "сеть — сеть" с помощью VPN-шлюза Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json);
- [масштабируемого набора виртуальных машин Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
