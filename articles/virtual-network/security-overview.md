---
title: "Общие сведения о безопасности сетей Azure | Документация Майкрософт"
description: "Сведения о функциях системы безопасности для управления потоком сетевого трафика между ресурсами Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial
ms.openlocfilehash: fbf0556cc47bc08a71fcf050b43c2dbbe5d27184
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="network-security"></a>Безопасность сети

Вы можете ограничить трафик к ресурсам в виртуальной сети при помощи группы безопасности сети. Группа безопасности сети содержит список правил безопасности, которые разрешают или запрещают входящий и исходящий трафик в зависимости от IP-адреса источника или назначения, порта и протокола. 

## <a name="network-security-groups"></a>Группы безопасности сети

С каждым сетевым интерфейсом можно связать одну группу безопасности сети (либо же она может отсутствовать). Каждый сетевой интерфейс находится в подсети [виртуальной сети](virtual-networks-overview.md). В подсети также может быть одна группа безопасности сети либо же она может отсутствовать. 

Если в подсети установлены правила безопасности, они применяются ко всем ее ресурсам. Помимо сетевых интерфейсов, в подсети могут быть развернуты экземпляры других служб Azure, таких как HDInsight, масштабируемые наборы виртуальных машин и среды службы приложений.

Группа безопасности сети, связанная с сетевым интерфейсом и подсетью, в которой он находится, применяется следующим образом:

- **Входящий трафик.** Сначала выполняется оценка в группе безопасности сети, связанной с подсетью, в которой находится сетевой интерфейс. Затем любой трафик, разрешенный группой безопасности сети, которая связана с подсетью, оценивается группой безопасности сети, которая связана с сетевым интерфейсом. Например, вам может потребоваться доступ входящего трафика из Интернета к виртуальной машине через порт 80. Если связать группу безопасности сети с сетевым интерфейсом и подсетью, в которой он находится, группа безопасности сети должна разрешать доступ через порт 80. Если доступ через порт 80 разрешен только группой безопасности сети, связанной с подсетью или сетевым интерфейсом, который находится в подсети, обмен данными не будет выполняться из-за правил безопасности по умолчанию. См. дополнительные сведения о [правилах безопасности по умолчанию](#default-security-rules). Если вы применили группу безопасности сети только к подсети или сетевому интерфейсу и она содержит правило, в котором разрешен входящий трафик, к примеру, через порт 80, обмен данными будет выполняться. 
- **Исходящий трафик.** Сначала выполняется оценка в группе безопасности сети, связанной с сетевым интерфейсом. Затем любой трафик, разрешенный группой безопасности сети, которая связана с сетевым интерфейсом, оценивается группой безопасности сети, которая связана с подсетью.

Не всегда известно, применяются ли группы безопасности сети и к сетевому интерфейсу, и к подсети. Все правила, применяемые к сетевому интерфейсу, можно просмотреть в списке [действующих правил безопасности](virtual-network-manage-nsg-arm-portal.md) сетевого интерфейса. Кроме того, вы можете воспользоваться функцией [Проверка IP-потока](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) в службе "Наблюдатель за сетями Azure", чтобы определить, разрешен ли обмен данными с сетевым интерфейсом. Средство позволяет узнать, разрешен ли обмен данными и какие правила безопасности сети разрешают или запрещают трафик.
 
> [!NOTE]
> Группы безопасности сети связаны с подсетями или виртуальными машинами и облачными службами, развернутыми при помощи классической модели, а не с сетевыми интерфейсами, для которых используется модель развертывания Resource Manager. Дополнительные сведения о моделях развертывания Azure см. в статье [Развертывание с помощью Azure Resource Manager и классическое развертывание: сведения о моделях развертывания и состоянии ресурсов](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Одна и та же группа безопасности сети может применяться к любому числу отдельных сетевых интерфейсов и подсетей.

## <a name="security-rules"></a>Правила безопасности

Группа безопасности сети может не содержать правила или содержать любое их число в пределах [ограничений](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) подписки Azure. В каждом правиле указываются следующие свойства:

|Свойство  |Пояснение  |
|---------|---------|
|ИМЯ|Уникальное имя в пределах группы безопасности сети.|
|Приоритет | Значение в диапазоне от 100 до 4096. Правила обрабатываются в порядке приоритета. Числа обрабатываются по возрастанию, так как меньшие числа имеют более высокий приоритет. Если показатель трафика соответствует установленному в правиле, обработка останавливается. В результате все правила с более низким приоритетом (большие числа) и тем же атрибутом,что и правила с высоким приоритетом, не обрабатываются.|
|Источник или назначение| Можно задать значение "Любой" или отдельный IP-адрес, блок CIDR (например, 10.0.0.0/24), тег службы или группу безопасности приложений. См. дополнительные сведения о [тегах служб](#service-tags) и [группах безопасности приложений](#application-security-groups). Если выбрать диапазон, тег службы или группу безопасности приложений, можно создавать меньше правил безопасности. Возможность указать несколько отдельных IP-адресов и диапазонов (нельзя указать несколько тегов служб или групп приложений) в правиле относится к расширенным правилам безопасности. Узнайте больше о [дополнительных правилах безопасности](#augmented-security-rules). Расширенные правила безопасности можно создавать только в группах безопасности сети, развернутых с помощью модели Resource Manager. Нельзя одновременно задать несколько IP-адресов и их диапазонов в группах безопасности сети, созданных с помощью классической модели развертывания.|
|Протокол     | TCP, UDP или значение "Любой" (включает TCP, UDP и ICMP). Нельзя указать только ICMP, поэтому если требуется ICMP, используйте значение "Любой". |
|Направление| Определяет тип трафика (входящий или исходящий), к которому применяется правило.|
|Диапазон портов     |Можно задать отдельный порт или их диапазон. Например, вы можете указать 80 или 10000–10005. Если указать диапазон, можно создавать меньше правил безопасности. Расширенные правила безопасности можно создавать только в группах безопасности сети, развернутых с помощью модели Resource Manager. Нельзя задать несколько портов или их диапазонов в одном и том же правиле безопасности в группах безопасности сети, созданных с помощью классической модели развертывания.   |
|Действие     | Разрешить или запретить доступ        |

Правила безопасности подразумевают отслеживание состояния. Если вы задаете правило безопасности для исходящего трафика по любому адресу, например, через порт 80, устанавливать правило безопасности входящего трафика для ответа на исходящий трафик не обязательно. Если связь инициируется извне, достаточно задать правило безопасности для входящего трафика. Обратное также верно. Если через порт разрешено поступление входящего трафика, задавать правило безопасности исходящего трафика для ответа на трафик через порт не требуется. Дополнительные сведения об ограничениях при создании правил безопасности см. в разделе [Ограничения сети — Azure Resource Manage](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Расширенные правила безопасности

Расширенные правила упрощают определение безопасности для виртуальных сетей, позволяя определять масштабные и комплексные политики безопасности сети с меньшим числом правил. Можно объединить несколько портов, несколько явных IP-адресов, теги служб и группы безопасности приложений в одном простом правиле безопасности. Используйте расширенные правила в полях источника, назначения и порта для правила. Когда вы создаете правило, можно указать несколько явных IP-адресов, диапазонов CIDR и портов. Чтобы упростить поддержку для определения правила безопасности, объедините расширенные правила безопасности с тегами служб или группами безопасности приложений. 

Дополнительные сведения об ограничениях при создании расширенных правил безопасности см. в разделе [Ограничения сети — Azure Resource Manage](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="default-security-rules"></a>Правила безопасности по умолчанию

Если группа безопасности сети не связана с подсетью или сетевым интерфейсом, для них будет разрешен весь входящий и исходящий трафик. Как только создается группа безопасности сети, Azure создает в ней следующие правила по умолчанию:

### <a name="inbound"></a>Входящий трафик

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Приоритет|Источник|Исходные порты|Место назначения|Конечные порты|Протокол|Access|
|---|---|---|---|---|---|---|
|65000|Виртуальная сеть|0-65535|Виртуальная сеть|0-65535|Все|РАЗРЕШИТЬ|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Приоритет|Источник|Исходные порты|Место назначения|Конечные порты|Протокол|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Все|РАЗРЕШИТЬ|

#### <a name="denyallinbound"></a>DenyAllInbound

|Приоритет|Источник|Исходные порты|Место назначения|Конечные порты|Протокол|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Все|Запрет|

### <a name="outbound"></a>Исходящие

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Приоритет|Источник|Исходные порты| Место назначения | Конечные порты | Протокол | Access |
|---|---|---|---|---|---|---|
| 65000 | Виртуальная сеть | 0-65535 | Виртуальная сеть | 0-65535 | Все | РАЗРЕШИТЬ |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Приоритет|Источник|Исходные порты| Место назначения | Конечные порты | Протокол | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Интернет | 0-65535 | Все | РАЗРЕШИТЬ |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Приоритет|Источник|Исходные порты| Место назначения | Конечные порты | Протокол | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Все | Запрет |

В столбцах **Источник** и **Место назначения** значения *VirtualNetwork*, *AzureLoadBalancer* и *Internet* являются [тегами служб](#tags), а не IP-адресами. В столбце протокола значение **Все** включает TCP, UDP и ICMP. При создании правила вы можете указать TCP, UDP или "Все". Задать только ICMP нельзя. Поэтому, если для вашего правила требуется ICMP, выберите для протокола значение *Все*. *0.0.0.0/0* в столбцах **Источник** и **Назначение** представляет все адреса.
 
Правила по умолчанию нельзя удалить, но их можно переопределить, создав правила с более высоким приоритетом.

## <a name="service-tags"></a>Теги служб

 Тег службы представляет группу префиксов IP-адресов, чтобы упростить создание правила безопасности. Нельзя создать собственный тег службы или задать IP-адреса, которые будут входить в тег. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов. Теги служб можно использовать вместо определенных IP-адресов при создании правил безопасности. В определении правила безопасности можно использовать указанные ниже теги служб. Их имена незначительно отличаются при использовании разных [моделей развертывания Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (**для развертываний Resource Manager) или VIRTUAL_NETWORK** (для классических развертываний). Этот тег включает адресное пространство виртуальной сети (все диапазоны CIDR, определенные для виртуальной сети), все адресное пространство подключенных локальных сетей и [пиринговые](virtual-network-peering-overview.md) виртуальные сети или виртуальные сети, подключенные к [шлюзу виртуальной сети](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (для развертываний с помощью Resource Manager) или **AZURE_LOADBALANCER** (для классических развертываний). Этот тег обозначает балансировщик нагрузки инфраструктуры Azure. Он преобразуется в [IP-адрес центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653), где инициируются проверки работоспособности Azure. Если подсистема балансировки нагрузки Azure не используется, это правило можно переопределить.
* **Internet** (для развертываний с помощью Resource Manager) или **INTERNET** (для классических виртуальных машин). Этот тег обозначает пространство IP-адресов, которые находятся за пределами виртуальной сети и к которым можно получить доступ из общедоступного сегмента Интернета. К этим адресам относится [общедоступное пространство IP-адресов, принадлежащее Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureTrafficManager** (только для развертываний с помощью Resource Manager). Этот тег определяет диапазон IP-адресов для IP-адресов зонда диспетчера трафика Azure. Дополнительные сведения об IP-адресах зонда диспетчера трафика, см. в разделе [Azure Traffic Manager FAQ](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-faqs) (Часто задаваемые вопросы о диспетчере трафика).
* **Storage** (только для развертываний с помощью Resource Manager). Этот тег определяет пространство IP-адресов для службы хранилища Azure. Если указать значение *Storage*, трафик будет разрешен или запрещен в хранилище. Чтобы разрешить доступ к хранилищу только в определенном [регионе](https://azure.microsoft.com/regions), можно указать этот регион. Например, чтобы разрешить доступ только к службе хранилища Azure в регионе "Восточная часть США", вы можете указать *Storage.EastUS* как тег службы. Тег представляет службу, но не определенные экземпляры службы. Например, тег представляет службу хранилища Azure, но не определенную учетную запись хранения Azure.
* **SQL** (только для развертываний с помощью Resource Manager). Этот тег определяет префиксы адресов для служб базы данных SQL Azure и хранилища данных SQL Azure. Значение *Sql* отвечает за разрешение или запрет трафика в SQL. Чтобы разрешить доступ к SQL только в определенном [регионе](https://azure.microsoft.com/regions), можно указать этот регион. Например, чтобы разрешить доступ только к базе данных SQL Azure в регионе "Восточная часть США", вы можете указать *Sql.EastUS* как тег службы. Тег представляет службу, но не определенные экземпляры службы. Например, тег представляет службу "База данных SQL Microsoft Azure", но не определенную базу данных или сервер SQL Azure.

> [!NOTE]
> Если вы реализуете для виртуальной сети [конечную точку службы](virtual-network-service-endpoints-overview.md), такой как служба хранилища Azure или "База данных SQL Microsoft Azure", Azure добавляет для нее маршрут в подсеть виртуальной сети. Префиксы адресов для маршрута — это те же префиксы или диапазоны CIDR, которые заданы в теге соответствующей службы.

## <a name="application-security-groups"></a>Группы безопасности приложений

Группы безопасности приложений позволяют настроить сетевую безопасность как естественное расширение в структуре приложения. Это позволяет группировать виртуальные машины и определять политики безопасности сети на основе таких групп. Эта функция также позволяет повторно использовать политику безопасности в нужном масштабе без обслуживания явных IP-адресов вручную. Платформа сама обрабатывает явные IP-адреса и множество наборов правил, позволяя вам сосредоточиться на бизнес-логике.

Группу безопасности приложений можно указать в правиле безопасности как источник и назначение. Определив политику безопасности, вы можете создавать виртуальные машины с сетевыми интерфейсами, которые можно назначать группе безопасности приложений. Для каждого сетевого интерфейса на виртуальной машине политика применяется на основе членства в группе безопасности приложений. В примере ниже показано, как можно использовать группу безопасности приложений для всех веб-серверов в вашей подписке:

1. Создайте группу безопасности приложений с именем *WebServers*.
2. Создайте группу безопасности сети с именем *MyNSG*.
3. Создайте в группе безопасности сети правило безопасности для входящего трафика, укажите тег службы *Internet* как адрес источника и группу безопасности приложений *WebServers* как адрес назначения, а затем разрешите доступ через порты 80 и 443.
4. Разверните виртуальную машину с запущенным приложением веб-сервера. Укажите, что сетевой интерфейс виртуальной машины входит в состав группы безопасности приложений *WebServers*. Для виртуальной машины будет разрешен доступ через порты 80 и 443. Доступ через эти порты также будет разрешен для любых созданных в дальнейшем веб-серверов в составе группы безопасности приложений *WebServers*. 

Если вы создаете другие правила и указываете в качестве назначения другие группы безопасности приложений, эти правила не применяются к веб-серверам в предыдущем примере. Правила, определяющие группу безопасности приложений, применяются только к сетевым интерфейсам, которые входят в ее состав. Группы безопасности приложений в сочетании с расширенными правилами безопасности и тегами служб позволяют создать минимальное число групп безопасности сети, чтобы управлять сетевой безопасностью в подписке.
 
Дополнительные сведения об ограничениях при создании групп безопасности приложений и их определении в правилах безопасности см. в разделе [Ограничения сети — Azure Resource Manager](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Группы безопасности приложений доступны в режиме предварительной версии. Для функций предварительной версии не предусмотрен такой же уровень доступности и надежности, как для общедоступного выпуска. Прежде чем применять группы безопасности приложения, зарегистрируйте их для использования, выполнив шаги 1–5 из разделов для Azure или PowerShell руководства по [созданию группы безопасности сети с группами безопасности приложений](create-network-security-group-preview.md). Группы безопасности приложений имеют следующие ограничения:

-   Все сетевые интерфейсы внутри группы безопасности приложения должны находиться в одной виртуальной сети. Невозможно добавить сетевые интерфейсы из разных виртуальных сетей в одну группу безопасности приложения. Виртуальная сеть, в которой находится первый сетевой интерфейс, назначенный группе безопасности приложения, определяет виртуальную сеть, в которой впоследствии должны находиться все назначенные сетевые интерфейсы.
- При указании групп безопасности приложений в качестве источника и назначения в правиле безопасности сетевые интерфейсы в обеих группах безопасности приложения должны существовать в одной виртуальной сети. Например, если ASG1 содержит сетевые интерфейсы из VNet1, а ASG2 — сетевые интерфейсы из VNet2, вам не удастся назначить ASG1 в качестве источника и ASG2 в качестве назначения в правиле, так как все сетевые интерфейсы должны существовать в VNet1.

## <a name="azure-platform-considerations"></a>Сведения о платформе Azure

- **Виртуальный IP-адрес главного узла.** Базовые службы инфраструктуры, например DHCP, DNS и служба мониторинга работоспособности, предоставляются через виртуальные IP-адреса 168.63.129.16 и 169.254.169.254. Эти общедоступные IP-адреса принадлежит корпорации Майкрософт. Они являются единственными виртуализированными IP-адресами, которые используются для этих целей во всех регионах. Адреса сопоставляются с физическим IP-адресом сервера (главного узла), на котором размещена виртуальная машина. Главный узел выполняет функции ретранслятора DHCP, рекурсивного сопоставителя DNS-имен и источника проб работоспособности, инициируемых балансировщиком нагрузки и виртуальными машинами. Взаимодействие с этими IP-адресами не рассматривается как атака. Если заблокировать трафик для этих IP-адресов, виртуальная машина может работать неправильно.
- **Лицензирование (служба управления ключами)**. Для используемых на виртуальных машинах образов Windows требуется лицензия. Чтобы получить лицензию, на серверы узлов службы управления ключами отправляется соответствующий запрос. Для отправки запроса используется исходящий порт 1688. Для развертываний, в которых используется конфигурация [маршрута по умолчанию 0.0.0.0/0](virtual-networks-udr-overview.md#default-route), это правило платформы будет отключено.
- **Виртуальные машины в пулах с балансировкой нагрузки.** Применяемые исходный порт и диапазон адресов относятся к исходному компьютеру, а не подсистеме балансировки нагрузки. Конечный порт и диапазон адресов относятся к целевому компьютеру, а не подсистеме балансировки нагрузки.
- **Экземпляры служб Azure.** В подсетях виртуальной сети развертываются экземпляры нескольких служб Azure, таких как HDInsight, среды службы приложений и масштабируемые наборы виртуальных машин. Полный список служб, которые можно развернуть в виртуальной сети, см. в статье [Интеграция виртуальной сети для служб Azure](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Обязательно ознакомьтесь с требования к портам для каждой службы, прежде чем применять группу безопасности сети к подсети, в которой развернут ресурс. Если запретить порты, которые требуются для службы, она будет работать неправильно.
- **Отправка исходящих сообщений электронной почты.** Корпорация Майкрософт рекомендует использовать аутентифицированные службы ретрансляции SMTP (обычно подключаются через TCP-порт 587, но часто через другие порты) для отправки электронной почты с виртуальных машин Azure. Службы ретрансляции SMTP оптимизируют репутацию отправителя, чтобы минимизировать риск отклонения сообщений сторонними поставщиками почтовых служб. Такие службы ретрансляции SMTP включают среди прочих Exchange Online Protection и SendGrid. Использование служб ретрансляции SMTP никак не ограничено в Azure независимо от типа подписки. 

  Если вы создали подписку Azure до 15 ноября 2017 г., кроме возможности использовать службы ретрансляции SMTP, вы также можете отправлять электронную почту непосредственно через TCP-порт 25. Если вы создали подписку Azure после 15 ноября 2017 г., вам может быть недоступна отправка электронной почты непосредственно через порт 25. Поведение исходящего трафика через порт 25, зависит от типа вашей подписки, как показано ниже.

     - **Соглашение Enterprise** — разрешен исходящий трафик через порт 25. Вы можете отправлять исходящие сообщения электронной почты непосредственно с виртуальных машин на платформе Azure внешним поставщикам почтовых служб без ограничений. 
     - **Оплата по мере использования.** Передача исходящих данных через порт 25 заблокирована для всех ресурсов. Если необходимо отправить электронное письмо с виртуальной машины непосредственно внешним поставщикам почтовых служб (без использования аутентифицированной ретрансляции SMTP), можно сделать запрос на снятие ограничения. Запросы проверяет и одобряет только корпорация Майкрософт. Они утверждаются только после проверок защиты от мошенничества. Чтобы создать запрос, откройте обращение в службу поддержки с типом проблемы *Техническая*, *Подключение к виртуальной сети*, *Не удается отправить сообщение электронной почты (SMTP/порт 25)*. В обращении в службу поддержки объясните, зачем для вашей подписки требуется отправка электронной почты непосредственно поставщикам почтовых служб вместо отправки с помощью аутентифицированной службы ретрансляции SMTP. Если для вашей подписки будет создано исключение из правила, только виртуальные машины, созданные после даты создания исключения, смогут передавать исходящие данные через порт 25.
     - **Поставщик облачных служб (CSP), MSDN, Azure Pass, Azure с открытой лицензией, Education, BizSpark и бесплатная пробная версия**. Передача исходящих данных через порт 25 заблокирована для всех ресурсов. Запросы на снятие ограничения отправить невозможно. Если нужно отправить сообщение электронной почты с виртуальной машины, используйте службу ретрансляции SMTP.

  Если Azure позволяет отправлять сообщения электронной почты через порт 25, корпорация Майкрософт не гарантирует, что поставщики электронной почты примут такие сообщения, отправленные с вашей виртуальной машины. Если конкретный поставщик отклоняет почту, отправленную с вашей виртуальной машины, обратитесь к такому поставщику, чтобы решить проблемы, связанные с доставкой сообщений или фильтрацией нежелательной почты. Или используйте аутентифицированную службу ретрансляции SMTP. 


## <a name="next-steps"></a>Дополнительная информация

* Выполните инструкции из руководства по [созданию группы безопасности сети](virtual-networks-create-nsg-arm-pportal.md).
* Выполните инструкции из руководства по [созданию группы безопасности сети с группами безопасности приложений](create-network-security-group-preview.md).
