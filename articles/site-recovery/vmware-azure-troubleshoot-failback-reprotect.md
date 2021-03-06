---
title: "Устранение неполадок при восстановлении размещения виртуальных машин Azure в локальную инфраструктуру VMware с помощью Azure Site Recovery | Документация Майкрософт"
description: "В этой статье описываются способы устранения распространенных ошибок восстановления размещения и повторного включения защиты в процессе восстановления размещения в VMware из Azure с помощью Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/27/2017
ms.author: rajanaki
ms.openlocfilehash: 9b1156884a78eb7d68dc9680765b3c1436c0606a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Устранение неполадок при восстановлении размещения из Azure в VMware

В этой статье описываются способы решения проблем, которые могут возникнуть при восстановлении размещения виртуальных машин Azure в локальную инфраструктуру VMware после отработки отказа в Azure с помощью [Azure Site Recovery](site-recovery-overview.md).

По сути восстановление размещения состоит из двух основных этапов. После отработки отказа необходимо повторно включить защиту виртуальных машин Azure в локальной сети, чтобы запустить их репликацию. Второй шаг — запустить отработку отказа из Azure для восстановления размещения на локальный сайт. 

## <a name="troubleshoot-reprotection-errors"></a>Устранение ошибок повторного включения защиты

В этом разделе описаны распространенные ошибки повторного включения защиты и способы их исправления.

### <a name="error-code-95226"></a>Код ошибки 95226

**Не удалось применить защиту повторно, так как виртуальной машине Azure не удалось связаться с локальным сервером конфигурации.**

Эта ошибка возникает в следующих случаях:

1. Виртуальная машина Azure не может связаться с локальным сервером конфигурации. Виртуальную машину не удается обнаружить и зарегистрировать на сервере конфигурации. 
2. Служба InMage Scout Application Service не запускается на виртуальной машине Azure после отработки отказа. Эта служба необходима для обмена данными с локальным сервером конфигурации.

Действия для устранения этой проблемы.

1. Проверьте, разрешает ли сеть виртуальной машины Azure этой виртуальной машине обмениваться данными с локальным сервером конфигурации. Для этого вы можете установить VPN-подключение типа "сеть — сеть" к локальному центру обработки данных или же настроить подключение ExpressRoute с частным пирингом в виртуальной сети виртуальной машины Azure. 
2. Если виртуальная машина может обмениваться данными с локальным сервером конфигурации, войдите на виртуальную машину и проверьте службу InMage Scout Application Service. Если эта служба не запущена, запустите ее вручную и установите для нее тип запуска "Автоматически".

### <a name="error-code-78052"></a>Код ошибки 78052

***Не удалось включить защиту для виртуальной машины.**

Это может произойти, если на главном целевом сервере, на который выполняется восстановление размещения, уже существует виртуальная машина с таким именем.

Чтобы решить эту проблему, выполните следующие действия:
1. Выберите другой главный целевой сервер на другом узле. Таким образом, при повторной защите будет создана виртуальная машина на другом узле, что исключит конфликт имен. 
2. Также можно использовать операцию vMotion для главного целевого сервера, чтобы переместить его на другой узел и избежать конфликта имен. Если имя существующей виртуальной машины выбрано случайно, переименуйте ее, чтобы можно было создать новую виртуальную машину на том же узле ESXi.

### <a name="error-code-78093"></a>Код ошибки 78093

**Виртуальная машина не работает, зависла или недоступна.**

Чтобы повторно включить защиту виртуальной машины, для которой выполнена отработка отказа, должна быть запущена виртуальная машина Azure. Это необходимо для регистрации службы Mobility Service на локальном сервере конфигурации и запуска репликации с помощью взаимодействия с сервером обработки. Если виртуальная машина подключена к неправильной сети или не работает (зависла или отключена), сервер конфигурации не сможет связаться со службой Mobility Service на виртуальной машине для запуска повторной защиты. 

1. Перезапустите виртуальную машину, чтобы она начала взаимодействовать с локальной средой.
2. После запуска виртуальной машины Azure перезапустите задание повторной защиты.

### <a name="error-code-8061"></a>Код ошибки 8061

**Хранилище данных недоступно из узла ESXi.**

Проверьте [предварительные требования к главному целевому серверу](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) и [поддерживаемые хранилища данных](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) для восстановления размещения.


## <a name="troubleshoot-failback-errors"></a>Устранение ошибок восстановления размещения

В этом разделе описаны распространенные ошибки, которые могут возникнуть во время восстановления размещения.

### <a name="error-code-8038"></a>Код ошибки 8038

**Локальную виртуальную машину не удалось подключить из-за ошибки**

Это происходит, если локальная виртуальная машина восстановлена на узле, на котором подготовлен недостаточный объем памяти. Действия для устранения этой проблемы.

1. Подготовьте больше памяти на узле ESXi.
2. Кроме того, можно использовать функцию vMotion, чтобы перенести виртуальную машину на другой узел ESXi с достаточным объемом памяти для ее запуска.
