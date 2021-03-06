---
title: "Общие сведения о поддержке Azure MQTT в Центре Интернета вещей | Документация Майкрософт"
description: "Руководство разработчика: поддержка устройств, подключающихся к конечной точке для устройств Центра Интернета вещей по протоколу MQTT. Содержит сведения о встроенной поддержке MQTT в пакетах SDK для устройств Azure IoT."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2018
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a22c20a26ee4750c79c23fbba69de72a0084dfe7
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/22/2018
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>Взаимодействие с Центром Интернета вещей с помощью протокола MQTT

Центр Интернета вещей позволяет устройствам взаимодействовать с конечными точками устройств Центра Интернета вещей с помощью:

* протокола [MQTT версии 3.1.1][lnk-mqtt-org] на порту 8883;
* протокола MQTT версии 3.1.1 через WebSocket на порту 443.

Весь обмен данными Центра Интернета вещей с устройствами защищен с помощью протокола TLS/SSL. Таким образом Центр Интернета вещей не поддерживает небезопасные подключения через порт 1883.

## <a name="connecting-to-iot-hub"></a>Подключение к Центру Интернета вещей

На устройстве протокол MQTT можно использовать для подключения к Центру Интернета вещей с помощью:

* библиотек в [пакетах SDK для Центра Интернета вещей Azure][lnk-device-sdks];
* непосредственно с помощью протокола MQTT.

## <a name="using-the-device-sdks"></a>Использование пакетов SDK для устройств

Доступны [пакеты SDK для устройств][lnk-device-sdks], поддерживающие протокол MQTT, для Java, Node.js, C, C# и Python. Для установки подключения к Центру Интернета вещей пакеты SDK для устройств используют стандартную строку подключения к Центру Интернета вещей. Чтобы использовать протокол MQTT, параметр протокола клиента должен быть задан как **MQTT**. По умолчанию пакеты SDK для устройств подключаются к Центру Интернета вещей, если для флага **CleanSession** установлено значение **0**, и используют **качество обслуживания первого уровня** для обмена сообщениями с Центром Интернета вещей.

Когда устройство подключено к Центру Интернета вещей, пакеты SDK для устройства предоставляют методы, позволяющие устройству осуществлять обмен сообщениями с Центром Интернета вещей.

В следующей таблице содержатся ссылки на примеры кода для каждого поддерживаемого языка и указаны параметры, используемые для подключения к Центру Интернета вещей по протоколу MQTT.

| Язык | Параметр протокола |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Переход от AMQP на MQTT в приложении устройства

При использовании [пакетов SDK для устройств][lnk-device-sdks] для перехода с протокола AMQP на MQTT требуется изменить параметр протокола в инициализации клиента, как указано выше.

При этом обязательно проверьте следующее:

* AMQP возвращает ошибки для многих условий, а MQTT завершает подключение. В результате может потребоваться изменить логику обработки исключений.
* MQTT не поддерживает операции *отклонения* при получении [сообщений из облака на устройство][lnk-messaging]. Если нужно, чтобы серверная часть получала ответы от приложения для устройства, стоит использовать [прямые методы][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Непосредственное использование протокола MQTT

Если устройство не может использовать пакеты SDK для устройств, оно может подключаться к общедоступным конечным точкам устройства по протоколу MQTT по порту 8883. В пакете **CONNECT** устройство должно использовать следующие значения.

* В поле **Идентификатор клиента** укажите значение **идентификатор устройства**.

* В поле **Username** (Имя пользователя) укажите значение `{iothubhostname}/{device_id}/api-version=2016-11-14`, где `{iothubhostname}` — это полная запись CName Центра Интернета вещей.

    Например, если имя Центра Интернета вещей — **contoso.azure-devices.net**, а имя устройства — **MyDevice01**, то полное поле **Username** (Имя пользователя) должно содержать:

    `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`

* В поле **Пароль** укажите маркер SAS. Формат маркера SAS аналогичен описанному для протоколов HTTPS и AMQP:

  `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`

  > [!NOTE]
  > При использовании аутентификации с помощью сертификата X.509 пароли маркеров SAS не требуются. Дополнительные сведения см. в статье [Настройка сертификата безопасности X.509 в Центре Интернета вещей Azure][lnk-x509].

  Дополнительные сведения о способах создания маркеров SAS см. в соответствующем разделе статьи [Управление доступом к Центру Интернета вещей][lnk-sas-tokens].

  При тестировании также можно использовать [обозреватель устройств][lnk-device-explorer] для быстрого создания маркера SAS, который можно скопировать и вставить в собственный код:

  1. Перейдите на вкладку **Управление** в **обозревателе устройств**.
  2. Щелкните **Маркер SAS** (вверху справа).
  3. В разделе **SASTokenForm** выберите свое устройство в раскрывающемся списке **DeviceID**. Задайте значение **срока жизни**.
  4. Щелкните **Создать** , чтобы создать маркер.

     Созданный маркер SAS имеет следующую структуру.

     `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

     Его часть используется в поле **Пароль** для подключения с использованием MQTT:

     `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

Для пакетов подключения и отключения MQTT Центр Интернета вещей создает событие в канале **мониторинга операций**. Это событие содержит дополнительные сведения, которые помогут устранить неполадки с подключением.

### <a name="tlsssl-configuration"></a>конфигурация протокола TLS/SSL

Чтобы напрямую использовать протокол MQTT, ваш клиент *должен* подключиться по протоколу TLS/SSL. Попытки пропустить этот шаг будут завершаться ошибками соединения.

Чтобы установить соединение TLS, может потребоваться скачать корневой сертификат DigiCert Baltimore и добавить ссылку на него. Этот сертификат используется в Azure для обеспечения безопасного подключения. Вы можете найти этот сертификат в репозитории [Azure-iot-sdk-c][lnk-sdk-c-certs]. Дополнительные сведения об этих сертификатах можно найти на [веб-сайте Digicert][lnk-digicert-root-certs].

Пример реализации с помощью версии библиотеки [Paho MQTT][lnk-paho] фонда Eclipse для Python может выглядеть следующим образом.

Сначала установите библиотеку Paho из командной строки:

```cmd/sh
pip install paho-mqtt
```

Затем запустите клиент в сценарии Python. Замените заполнители следующим образом.

* `<local path to digicert.cer>` — путь к локальному файлу, содержащему корневой сертификат DigiCert Baltimore. Этот файл можно создать путем копирования сведений о сертификате из [certs.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) в пакете Центра Интернета вещей Azure для C. Укажите строки `-----BEGIN CERTIFICATE-----` и `-----END CERTIFICATE-----`, удалите метки `"` в начале и в конце каждой строки, а также удалите знаки `\r\n` в конце каждой строки.
* `<device id from device registry>` — идентификатор устройства, добавленного в Центр Интернета вещей.
* `<generated SAS token>` — маркер SAS для устройства, созданного как описано ранее в этой статье.
* `<iot hub name>` — имя Центра Интернета вещей.

```python
from paho.mqtt import client as mqtt
import ssl

path_to_root_cert = "<local path to digicert.cer>"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
iot_hub_name = "<iot hub name>"

def on_connect(client, userdata, flags, rc):
  print ("Device connected with result code: " + str(rc))
def on_disconnect(client, userdata, rc):
  print ("Device disconnected with result code: " + str(rc))
def on_publish(client, userdata, mid):
  print ("Device sent message")

client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.on_connect = on_connect
client.on_disconnect = on_disconnect
client.on_publish = on_publish

client.username_pw_set(username=iot_hub_name+".azure-devices.net/" + device_id, password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None)
client.tls_insecure_set(False)

client.connect(iot_hub_name+".azure-devices.net", port=8883)

client.publish("devices/" + device_id + "/messages/events/", "{id=123}", qos=1)
client.loop_forever()
```

### <a name="sending-device-to-cloud-messages"></a>Отправка сообщений из устройства в облако

После успешного подключения устройство может отправлять сообщения в Центр Интернета вещей, используя `devices/{device_id}/messages/events/` или `devices/{device_id}/messages/events/{property_bag}` в качестве значения параметра **Имя раздела**. Элемент `{property_bag}` позволяет устройству отправлять сообщения с дополнительными свойствами в формате URL-адреса. Например: 

```text
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> В этом элементе `{property_bag}` используется та же кодировка, что и для строк запросов в протоколе HTTPS.

Приложение для устройства может также использовать `devices/{device_id}/messages/events/{property_bag}` в качестве значения параметра **Will topic name** (Будущее имя раздела) для определения *будущих сообщений*, которые будут пересылаться в качестве сообщения телеметрии.

* Центр Интернета вещей не поддерживает сообщения со вторым уровнем качества обслуживания. Если приложение для устройства публикует сообщение со **вторым уровнем качества обслуживания**, то Центр Интернета вещей закрывает сетевое подключение.
* Центр Интернета вещей не сохраняет сообщения retain. Если устройство отправляет сообщение с флагом **RETAIN**, имеющим значение 1, то Центр Интернета вещей добавляет в сообщение свойство приложения **x-opt-retain**. В этом случае Центр Интернета вещей не хранит сообщение retain, а передает его во внутреннее приложение.
* Центр Интернета вещей поддерживает только одно активное подключение MQTT на устройство. Любое новое подключение MQTT от имени того же идентификатора устройства приводит к тому, что Центр Интернета вещей разрывает существующее подключение.

Дополнительные сведения см. в статье [Отправка и получение сообщений в Центре Интернета вещей][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Получение сообщений из облака на устройство

Для получения сообщений из Центра Интернета вещей устройство должно подписаться с использованием `devices/{device_id}/messages/devicebound/#` в качестве значения параметра **Topic Filter** (Фильтр разделов). Многоуровневый подстановочный знак `#` в параметре Topic Filter (Фильтр разделов) используется только для того, чтобы разрешить устройству получать дополнительные свойства в имени раздела. В Центре Интернета вещей запрещено использовать подстановочные знаки `#` или `?` для фильтрации подразделов. Так как Центр Интернета вещей не является универсальным брокером службы сообщений на основе шаблона "издатель-подписчик", то он поддерживает только задокументированные имена и фильтры разделов.

Устройство не будет получать сообщения из Центра Интернета вещей, пока не будет успешно подписано на соответствующую конечную точку устройства, представленную фильтром разделов `devices/{device_id}/messages/devicebound/#`. Когда подписка выполнена, устройство получает сообщения, переданные из облака на устройство, только с момента подписки. Если устройство подключается с флагом **CleanSession**, имеющим значение **0**, то подписка будет сохраняться в разных сеансах. В этом случае при следующем подключении с флагом **CleanSession 0** устройство получает ожидающие сообщения, отправленные на него, пока оно было отключено. Если устройство использует флаг **CleanSession** со значением **1**, то оно не будет получать сообщения из Центра Интернета вещей, пока не будет подписано на конечную точку устройства.

Центр Интернета вещей передает сообщения с **именем раздела** `devices/{device_id}/messages/devicebound/` или `devices/{device_id}/messages/devicebound/{property_bag}` при наличии свойств сообщения. `{property_bag}` содержит закодированные в формате URL-адреса пары "ключ-значение" свойств сообщения. В контейнер свойств входят только свойства приложений и задаваемые пользователем системные свойства (такие как **messageId** или **correlationId**). Имена системных свойств имеют префикс **$**, свойства приложений используют исходное имя свойства без префикса.

Если приложение для устройства подписывается на раздел со **вторым уровнем качества обслуживания**, то Центр Интернета вещей присваивает пакету **SUBACK** уровень качества обслуживания не выше первого. После этого Центр Интернета вещей доставляет сообщения на устройство, используя первый уровень качества обслуживания.

### <a name="retrieving-a-device-twins-properties"></a>Получение свойств двойника устройства

Сначала устройство подписывается на `$iothub/twin/res/#`, чтобы получать ответы операций. Затем оно отправляет пустое сообщение в раздел `$iothub/twin/GET/?$rid={request id}` с заполненным значением **request ID** (идентификатор запроса). Затем служба отправляет ответное сообщение, содержащее данные двойника устройства в разделе `$iothub/twin/res/{status}/?$rid={request id}`, используя то же значение **идентификатора запроса**, что и в запросе.

Идентификатором запроса может быть любое допустимое значение свойства сообщения, как указано в статье [Обмен сообщениями между устройством и облаком с помощью Центра Интернета вещей][lnk-messaging], а состояние проверяется как целое число.

Текст ответа содержит раздел properties двойника устройства. В следующем фрагменте кода показан текст записи реестра удостоверений, ограниченный компонентом properties, например:

```json
{
    "properties": {
        "desired": {
            "telemetrySendFrequency": "5m",
            "$version": 12
        },
        "reported": {
            "telemetrySendFrequency": "5m",
            "batteryLevel": 55,
            "$version": 123
        }
    }
}
```

Возможны следующие коды состояний:

|Status | ОПИСАНИЕ |
| ----- | ----------- |
| 200 | Успешно |
| 429 | Слишком много запросов (регулирование), как указано в статье [Reference - quotas and throttling][lnk-quotas] (Справочные материалы. Квоты и регулирование) |
| 5** | ошибки сервера; |

Дополнительные сведения см. в статье [Двойники устройства][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Обновление сообщаемых свойств двойника устройства

Следующая последовательность действий описывает, как устройство обновляет сообщаемые свойства в двойнике устройства в Центре Интернета вещей:

1. Сначала устройство должно подписаться на раздел `$iothub/twin/res/#`, чтобы получать ответы операций из Центра Интернета вещей.

1. Устройство отправляет сообщение, содержащее обновление двойника устройства, в раздел `$iothub/twin/PATCH/properties/reported/?$rid={request id}`. Это сообщение содержит значение **request ID** (идентификатор запроса).

1. Затем служба отправляет ответное сообщение, содержащее новое значение ETag для коллекции сообщаемых свойств в разделе `$iothub/twin/res/{status}/?$rid={request id}`. В этом ответном сообщении используется то же значение **request ID**, что и в запросе.

Текст запроса содержит документ JSON, в котором имеются новые значения для переданных свойств. Каждый элемент документа JSON обновляет или добавляет соответствующий компонент в документе двойника устройства. Если элементу задано значение `null`, то этот компонент удаляется из содержащего его объекта. Например: 

```json
{
    "telemetrySendFrequency": "35m",
    "batteryLevel": 60
}
```

Возможны следующие коды состояний:

|Status | ОПИСАНИЕ |
| ----- | ----------- |
| 200 | Успешно |
| 400 | Недопустимый запрос. Неправильно сформированный JSON. |
| 429 | Слишком много запросов (регулирование), как указано в статье [Reference - quotas and throttling][lnk-quotas] (Справочные материалы. Квоты и регулирование) |
| 5** | ошибки сервера; |

Дополнительные сведения см. в статье [Двойники устройства][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Получение уведомлений об обновлении требуемых свойств

При подключении устройства Центр Интернета вещей отправляет уведомления в раздел `$iothub/twin/PATCH/properties/desired/?$version={new version}`, в котором находится содержимое обновления, выполненного серверной частью решения. Например: 

```json
{
    "telemetrySendFrequency": "5m",
    "route": null
}
```

В обновлениях свойств значение `null` означает, что компонент объекта JSON удаляется.

> [!IMPORTANT]
> Центр Интернета вещей создает уведомления об изменении только в том случае, если устройства подключены. Убедитесь, что выполняется [процедура при повторном подключении устройства][lnk-devguide-twin-reconnection], чтобы требуемые свойства продолжали синхронизироваться между Центром Интернета вещей и приложением для устройства.

Дополнительные сведения см. в статье [Двойники устройства][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Ответ на прямой метод

Сначала устройство должно подписаться на `$iothub/methods/POST/#`. Центр Интернета вещей отправляет запросы метода в раздел `$iothub/methods/POST/{method name}/?$rid={request id}` с допустимым документом JSON или без текста.

В качестве ответа устройство отправляет сообщение без текста или с допустимой строкой JSON в раздел `$iothub/methods/res/{status}/?$rid={request id}`. В этом сообщении значение **request ID** должно совпадать с идентификатором в сообщении запроса, а в качестве **status** должно быть указано целое число.

Дополнительные сведения см. в статье [Прямые методы][lnk-methods].

### <a name="additional-considerations"></a>Дополнительные замечания

Последнее замечание. Если на стороне облака требуется настроить реакцию на событие протокола MQTT, ознакомьтесь со статьей [о дополнительных протоколах для Центра Интернета вещей][lnk-azure-protocol-gateway]. Это программное обеспечение позволяет развернуть шлюз протокола с высокой производительностью, который взаимодействует непосредственно с Центром Интернета вещей. Шлюз протокола Azure IoT позволяет настроить протокол устройства для уже существующих развертываний MQTT или других настраиваемых протоколов. Однако при этом подходе необходимо запустить настраиваемый шлюз протокола и управлять им.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о протоколе MQTT см. в [документации по MQTT][lnk-mqtt-docs].

Дополнительные сведения о планировании развертывания Центра Интернета вещей см. в следующих руководствах:

* [Каталог устройств, сертифицированных по программе Microsoft Azure Certified for IoT][lnk-devices]
* [Поддержка дополнительных протоколов для центра IoT][lnk-protocols]
* [Сравнение центра IoT и концентраторов событий][lnk-compare]
* [Масштабирование центра IoT][lnk-scaling]

Для дальнейшего изучения возможностей Центра Интернета вещей см. следующие статьи:

* [Руководство разработчика для Центра Интернета вещей][lnk-devguide]
* [Развертывание ИИ на пограничных устройствах с использованием Azure IoT Edge][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-x509]: iot-hub-security-x509-get-started.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
[lnk-sdk-c-certs]: https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c
[lnk-digicert-root-certs]: https://www.digicert.com/digicert-root-certificates.htm
[lnk-paho]: https://pypi.python.org/pypi/paho-mqtt
