---
title: "Группы контейнеров в службе \"Экземпляры контейнеров Azure\""
description: "Основные сведения о работе групп контейнеров в службе \"Экземпляры контейнеров Azure\""
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a42c01917926a4297c97cf9c5dfd1333dbef6793
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Группы контейнеров в службе "Экземпляры контейнеров Azure"

Ресурс верхнего уровня в службе "Экземпляры контейнеров Azure" — это *группа контейнеров*. В этой статье представлены сведения о группах контейнеров и описаны сценарии, которые можно реализовать с их помощью.

## <a name="how-a-container-group-works"></a>Принцип работы группы контейнеров

Группа контейнеров — это набор контейнеров, которые можно планировать на одном хост-компьютере. Контейнеры в группе совместно используют жизненный цикл, локальную сеть и тома хранилища. Они похожи на концепцию *pod* в [Kubernetes][kubernetes-pod] и [DC/OS][dcos-pod].

На схеме ниже показан пример группы контейнеров, включающей несколько контейнеров.

![Диаграмма групп контейнеров][container-groups-example]

Группа контейнеров в этом примере:

* Планируется на одном хост-компьютере.
* Предоставляет один общедоступный IP-адрес с одним предоставленным портом.
* Состоит из двух частей. Один контейнер ожидает передачи данных на порте 80, а другой — на порте 5000.
* Включает два общих файловых ресурса Azure в качестве подключенных томов. При этом к каждому контейнеру локально подключен один из общих ресурсов.

> [!NOTE]
> Многоконтенерные группы сейчас ограничены контейнерами Linux. Мы работаем над тем, чтобы обеспечить все функции для контейнеров Windows, но для текущей платформы есть отличия в [квотах и доступности регионов для службы "Экземпляры контейнеров Azure"](container-instances-quotas.md).

### <a name="networking"></a>Сеть

Группы контейнеров совместно используют IP-адрес и пространство имен порта для этого IP-адреса. Чтобы внешние клиенты могли получить доступ к контейнеру в группе, необходимо предоставить порт по IP-адресу и из контейнера. Так как контейнеры в группе совместно используют пространство имен порта, сопоставление портов не поддерживается. Контейнеры в пределах группы могут взаимодействовать через локальный узел на предоставленных портах, даже если эти порты не предоставлены извне по IP-адресу группы.

### <a name="storage"></a>Хранилище

Вы можете указать внешние тома для подключения в пределах группы контейнеров. Эти тома можно сопоставить с конкретными путями в пределах отдельных контейнеров в группе.

## <a name="common-scenarios"></a>Распространенные сценарии

Группы с несколькими контейнерами полезны, если требуется разделить одну функциональную задачу на небольшое количество образов контейнера, которые могут доставляться различными группами и иметь отдельные требования к ресурсам.

Пример использования может включать следующее:

* Контейнер приложения и журнала. Контейнер журнала собирает журналы и выходные данные метрик для основного приложения и записывает их в хранилище для долговременного хранения.
* Контейнер приложения и мониторинга. Контейнер мониторинга периодически выполняет запрос к приложению, чтобы убедиться, что оно правильно работает и отвечает, и выдает предупреждение, если это не так.
* Контейнер, обслуживающий веб-приложение, и контейнер, извлекающий последнее содержимое из системы управления версиями.

## <a name="next-steps"></a>Дополнительная информация

Узнайте, как [развертывать группу с несколькими контейнерами](container-instances-multi-container-group.md) с использованием шаблона Azure Resource Manager.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
