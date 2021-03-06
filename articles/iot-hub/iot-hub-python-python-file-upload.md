---
title: "Отправка файлов с устройств в Центр Интернета вещей с помощью Python | Документация Майкрософт"
description: "Сведения об отправке файлов с устройства в облако с помощью пакета SDK для устройств Azure IoT для Python. Отправленные файлы хранятся в контейнере больших двоичных объектов службы хранилища Azure."
services: iot-hub
documentationcenter: python
author: kgremban
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: kgremban
ms.openlocfilehash: 5939f87684e92e1f95d39ea5bd52b424ca683acc
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Передача файлов с устройства в облако с помощью Центра Интернета вещей

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

В этом руководстве описано, как использовать [возможности передачи файлов Центра Интернета вещей](iot-hub-devguide-file-upload.md), чтобы передать файл в [хранилище BLOB-объектов Azure](../storage/index.yml). В этом руководстве описаны следующие процедуры.

- Безопасное предоставление контейнера хранилища для передачи файла.
- Использование клиента Python для передачи файла через Центр Интернета вещей.

В руководствах [Подключение устройства к Центру Интернета вещей с помощью .NET](iot-hub-node-node-getstarted.md) и [Отправка сообщений из облака в виртуальное устройство с помощью Центра Интернета вещей (.NET)](iot-hub-node-node-c2d.md) описаны базовые функции Центра Интернета вещей для обмена сообщениями между устройством и облаком. Тем не менее в некоторых случаях не просто сопоставить данные, отправляемые устройством, с относительно небольшими сообщениями, отправляемыми из устройства в облако, которые принимает Центр Интернета вещей. При передаче файлов с устройства вы можете рассчитывать на безопасность и надежность Центра Интернета вещей.

> [!NOTE]
> Пакет SDK для Центра Интернета вещей для Python в настоящее время поддерживает передачу только текстовых файлов, таких как **TXT**.

По завершении работы с этим руководством вы запустите консольное приложение Python:

* **FileUpload.py**, которое отправляет файл в хранилище с помощью пакета SDK для устройств для Python.

> [!NOTE]
> Существуют пакеты SDK для устройств Azure IoT, обеспечивающие поддержку многих платформ устройств и языков (включая C, .NET, JavaScript, Python и Java) в Центре Интернета вещей. Пошаговые инструкции по подключению устройства к Центру Интернета вещей Azure см. в [Центре разработчика для Центра Интернета вещей Azure].

Для работы с этим учебником требуется:

* [Python 2.x или 3.x][lnk-python-download]. Обязательно используйте 32-разрядную или 64-разрядную версию установки согласно требованиям программы настройки. При появлении запроса во время установки обязательно добавьте Python в переменную среды соответствующей платформы. Если вы используете Python 2.x, может потребоваться [установка или обновление *pip* — системы управления пакетами Python][lnk-install-pip].
* При использовании ОС Windows потребуется [распространяемый пакет Visual C++][lnk-visual-c-redist], чтобы разрешить использовать встроенные библиотеки DLL из Python.
* Активная учетная запись Azure. Если ее нет, можно создать [бесплатную учетную запись](http://azure.microsoft.com/pricing/free-trial/) всего за несколько минут.


[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Передача файла из приложения устройства

В этом разделе вы создадите приложение для устройства, чтобы отправлять файлы в Центр Интернета вещей.

1. Чтобы установить пакет **azure-iothub-device-client**, в командной строке выполните следующую команду.

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. В текстовом редакторе создайте в рабочей папке файл **FileUpload.py**.

1. Добавьте следующие инструкции `import` и переменные в начало файла **FileUpload.py**. Замените `deviceConnectionString` строкой подключения устройства Центра Интернета вещей.

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.HTTP

    FILENAME = 'sample.txt'
    ```

1. Создайте обратный вызов функции **upload_blob**:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. Добавьте следующий код для подключения клиента и отправки файла. Добавьте также подпрограмму `main`.

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )
        
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            client.upload_blob_async(FILENAME, FILENAME, os.path.getsize(FILENAME), blob_upload_conf_callback, 0)
        
            print ( "" )
            print ( "File upload initiated..." )
        
            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )
        
    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

1. Сохраните и закройте файл **UploadFile.py**.

1. Скопируйте образец текстового файла в рабочую папку и измените его имя на `sample.txt`.

    > [!NOTE]
    > Пакет SDK для Центра Интернета вещей для Python в настоящее время поддерживает передачу только текстовых файлов, таких как **TXT**.


## <a name="run-the-application"></a>Выполнение приложения

Теперь все готово к запуску приложения.

1. В командной строке в рабочей папке выполните следующую команду.

    ```cmd/sh
    python FileUpload.py
    ```

1. На следующем снимке экрана показаны выходные данные приложения **FileUpload**.

    ![Выходные данные приложения simulated-device](./media/iot-hub-python-python-file-upload/1.png)

1. На портале можно просмотреть отправленный файл в контейнере хранилища, который вы настроили.

    ![Отправленный файл](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Дополнительная информация

В этом руководство показано, как использовать возможности передачи файлов Центра Интернета вещей, чтобы упростить передачу файлов из устройств. Изучение функций и сценариев Центра Интернета вещей можно продолжить в следующих руководствах:

* [Создание Центра Интернета вещей с помощью PowerShell][lnk-create-hub]
* [Знакомство с пакетом SDK для устройств Центра Интернета вещей Azure для C][lnk-c-sdk]
* [IoT Hub SDKs][lnk-sdks] (Пакеты SDK для Центра Интернета вещей)

<!-- Links -->
[Центре разработчика для Центра Интернета вещей Azure]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/