---
title: "Создание центра Интернета вещей на портале Azure | Документация Майкрософт"
description: "Сведения о том, как с помощью портала Azure создавать и удалять Центры Интернета вещей Azure, а также управлять ими. Содержит сведения о ценовых категориях, масштабировании, безопасности, а также о настройке обмена сообщениями."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: ac1a52355ffa5354bebe3b98fdb75783bcd57697
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Создание Центра Интернета вещей с помощью портала Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Содержание статьи

* Как найти Центр Интернета вещей на портале Azure.
* Как создавать и администрировать Центры Интернета вещей.

## <a name="where-to-find-the-iot-hub-service"></a>Где найти службу Центра Интернета вещей

Службу Центра Интернета вещей на портале можно найти в следующих расположениях:

* Выберите **+Создать**, а затем — **Интернет вещей**.
* В Microsoft Azure Marketplace выберите **Интернет вещей**.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

Центр Интернета вещей можно создать с помощью следующих методов.

* Действие **+Создать** открывает представленную ниже колонку. Шаги по созданию Центра Интернета вещей будут такими же, как и в Marketplace.
* В Marketplace выберите **Создать**, чтобы открыть представленную ниже колонку.

В следующих разделах поэтапно описано создание Центра Интернета вещей.

### <a name="choose-the-name-of-the-iot-hub"></a>Выбор имени Центра Интернета вещей

Чтобы создать Центр Интернета вещей, следует указать его имя. Это имя должно быть уникальным среди всех Центров Интернета вещей.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Выбор ценового уровня

Доступны четыре уровня: **Бесплатный**, **Стандартный 1**, **Стандартный 2** и **Стандартный S3**. Уровень "Бесплатный" позволяет подключить к Центру Интернета вещей только 500 устройств и отправлять до 8000 сообщений в день.

**Стандартный S1**: используйте выпуск S1 для решений Интернета вещей с большим числом устройств, каждое из которых генерирует небольшой объем данных. Каждая единица выпуска S1 позволяет передавать не более 400 тыс. сообщений в день со всех устройств.

**Стандартный S2**: выпуск S2 предназначен для решений Интернета вещей, в которых устройства генерируют большие объемы данных. Каждая единица выпуска S2 позволяет передавать не более 6 млн сообщений в день со всех устройств.

**Стандартный S3**: выпуск S3 предназначен для решений Интернета вещей с огромными объемами данных. Каждая единица выпуска S3 позволяет передавать не более 300 млн сообщений в день со всех устройств.

![][4]

> [!NOTE]
> Центр Интернета вещей позволяет подключить только один центр уровня "Бесплатный" для каждой подписки Azure.

### <a name="iot-hub-units"></a>Единицы Центров Интернета вещей

Допустимое число сообщений за единицу в сутки зависит от ценовой категории концентратора. Например, если требуется, чтобы Центр Интернета вещей поддерживал 700 000 входящих сообщений, то следует выбрать две единицы уровня S1.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Разделы «с устройства в облако» и группы ресурсов

Вы можете изменить количество разделов для Центра Интернета вещей. По умолчанию устанавливается четыре раздела, но вы можете изменить это число, используя раскрывающийся список.

Отдельно создавать пустую группу ресурсов необязательно. При создании ресурса вы можете выбрать существующую группу ресурсов или создать новую.

![][5]

### <a name="choose-subscription"></a>Выбор подписки

Центр Интернета вещей Azure автоматически отображает все подписки, с которыми связана учетная запись пользователя. Здесь вы можете выбрать подписку Azure, с которой будет связан Центр Интернета вещей.

### <a name="choose-the-location"></a>Выберите расположение

Параметр расположения содержит список регионов, в которых доступен Центр Интернета вещей.

### <a name="create-the-iot-hub"></a>Создание Центра Интернета вещей

Когда вы выполните все описанные шаги, Центр Интернета вещей будет готов к созданию. Нажмите кнопку **Создать**, чтобы запустить фоновый процесс создания и развертывания Центра Интернета вещей с настроенными параметрами.

Процесс создания Центра Интернета вещей может занять несколько минут, так как развертывание для запуска на серверах в нужном расположении требует некоторого времени.

## <a name="change-the-settings-of-the-iot-hub"></a>Изменение параметров Центра Интернета вещей

Вы можете изменить параметры Центра Интернета вещей после его создания, используя колонку этого центра.

![][8]

**Политики общего доступа:** эти политики определяют разрешения для подключения устройств и служб к Центру Интернета вещей. Чтобы открыть эти политики, в разделе **Общие** щелкните пункт **Политики общего доступа**. В этой колонке вы сможете изменить существующие политики или добавить новую.

### <a name="create-a-policy"></a>Создание политики

* Щелкните **Добавить**, чтобы открыть колонку. Здесь можно ввести новое имя политики и указать разрешения, которые нужно связать с ней. Эта колонка показана на следующем рисунке.

    Для общих политик можно указать несколько разрешений. Политики **Чтение реестра** и **Запись в реестр** предоставляют права на чтение и запись в реестре удостоверений. Если вы выберете разрешение на запись, разрешение на чтение добавляется автоматически.

    Политика **Подключение службы** предоставляет разрешение на доступ к конечным точкам службы, например для **передачи данных от устройства в облако**. Политика **Подключение устройства** предоставляет разрешения на отправку и получение сообщений через конечные точки Центра Интернета вещей на стороне устройства.

* Чтобы добавить новую политику к списку уже существующих, нажмите кнопку **Создать** .

![][10]

## <a name="endpoints"></a>Конечные точки

Щелкните **Конечные точки**, чтобы отобразить список конечных точек для редактируемого Центра Интернета вещей. Существует два типа конечных точек: конечные точки, встроенные в Центр Интернета вещей, и конечные точки, добавляемые в Центр Интернета вещей после его создания.

![][11]

### <a name="built-in-endpoints"></a>Встроенные конечные точки

Встроенные конечные точки делятся на два типа: **Cloud to device feedback** (Ответ, отправляемый из облака на устройство) и **События**.

* **Cloud to device feedback** (Ответ, отправляемый из облака на устройство) имеет два вложенных параметра: **Cloud to Device TTL** (Срок жизни) и **Время хранения** (в часах) для сообщений. При создании Центра Интернета вещей оба этих параметра имеют значение по умолчанию "один час". Чтобы изменить эти параметры, используйте ползунки или введите значения вручную.
* **События** имеют несколько вложенных параметров, и некоторые из них доступны только для чтения. Эти параметры приведены в следующем списке:

  * **Секции**: значение по умолчанию задается при создании Центра Интернета вещей. Вы можете изменить количество секций, используя этот параметр.

  * **Имя и конечная точка, совместимые с концентратором событий.** При создании Центра Интернета вещей автоматически создается концентратор событий, к которому при некоторых обстоятельствах может потребоваться доступ. Значения имени и конечной точки, совместимых с концентраторами событий, невозможно изменить, но их можно скопировать, щелкнув **Копировать**.

  * **Время хранения**: по умолчанию устанавливается значение "один день", но его можно изменить с помощью раскрывающегося списка. Это значение задается в днях и относится к событиям, передаваемым с устройства в облако.

  * **Группы потребителей** позволяют сгруппировать модули чтения, которые будут независимо друг от друга получать сообщения от Центра Интернета вещей. Каждый Центр Интернета вещей создается с одной группой потребителей по умолчанию. С помощью этого параметра можно добавлять и удалять группы потребителей для Центров Интернета вещей.

  > [!NOTE]
  > Группа потребителей по умолчанию не может быть удалена или изменена.

### <a name="custom-endpoints"></a>Пользовательские конечные точки

Пользовательские конечные точки можно добавить в Центр Интернета вещей с помощью портала. В верхней части колонки **Конечные точки** щелкните **Добавить**, чтобы открыть колонку **Добавить конечную точку**. Введите необходимые сведения и нажмите кнопку **ОК**. После этого пользовательская конечная точка появится в основной колонке **Конечные точки**.

![][13]

Дополнительные сведения о пользовательских конечных точках см. в статье [Руководство. Конечные точки Центра Интернета вещей][lnk-devguide-endpoints].

## <a name="routes"></a>Маршруты

Щелкните **Маршруты**, чтобы управлять тем, как Центр Интернета вещей отправляет сообщения из устройства в облако.

![][14]

Вы можете добавить маршруты для Центра Интернета вещей. Для этого в верхней части колонки **Маршруты*** щелкните **Добавить** , введите необходимые сведения и нажмите кнопку **ОК**. После этого ваш маршрут появится в основной колонке **Маршруты**. Маршрут можно изменить, щелкнув его в списке маршрутов. Чтобы включить маршрут, щелкните его в списке маршрутов и установите переключатель **включения и отключения** в положение **Выкл** Нажмите кнопку **ОК** в нижней части колонки, чтобы сохранить изменения.

![][15]

## <a name="pricing-and-scale"></a>Цены и масштабирование

Вы можете изменить цену существующего Центра Интернета вещей с помощью параметров **Цены**, но есть определенные исключения.

* В текущей версии нельзя перенести Центр Интернета вещей с бесплатного уровня на один из платных, и наоборот.
* Для каждой подписки Azure можно использовать только один Центр Интернета вещей уровня "Бесплатный".

![][12]

Вы можете перейти с высокого уровня на более низкий, только если число сообщений за текущий день еще не превысило квоту, установленную для более низкого уровня. Например, если количество сообщений за день превышает 400 000, уровень Центра Интернета вещей может измениться. Однако если изменить уровень на S1, Центр Интернета вещей регулируется в зависимости от количества сообщений, полученных на протяжении этого дня.

## <a name="delete-the-iot-hub"></a>Удаление Центра Интернета вещей

Вы можете найти Центр Интернета вещей с помощью кнопки **Обзор** и выбрать его для удаления. Чтобы удалить Центр Интернета вещей, нажмите кнопку **Удалить** под соответствующим именем.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об управлении Центром Интернета вещей в Azure см. по следующим ссылкам:

* [Массовое управление удостоверениями устройств Центра Интернета вещей][lnk-bulk]
* [Метрики Центра Интернета вещей][lnk-metrics]
* [Мониторинг операций][lnk-monitor]

Для дальнейшего изучения возможностей Центра Интернета вещей см. следующие статьи:

* [Руководство разработчика для Центра Интернета вещей][lnk-devguide]
* [Развертывание ИИ на пограничных устройствах с использованием Azure IoT Edge][lnk-iotedge]
* [Все аспекты безопасности решения Центра Интернета вещей][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
