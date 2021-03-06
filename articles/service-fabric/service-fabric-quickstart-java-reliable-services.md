---
title: "Создание приложения Java в Azure Service Fabric | Документация Майкрософт"
description: "В рамках этого краткого руководства вы создадите приложение Java для Azure, используя пример приложения надежных служб Service Fabric."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 0b284194abbbdd38524c0ae74ab7e05977d6883f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/24/2018
---
# <a name="quickstart-deploy-a-java-service-fabric-reliable-services-application-to-azure"></a>Краткое руководство. Развертывание приложения надежных служб Java Service Fabric в Azure
Azure Service Fabric — это платформа распределенных систем для развертывания микрослужб и контейнеров и управления ими. 

В этом кратком руководстве показано, как развернуть первое приложение Java в Service Fabric с использованием интегрированной среды разработки Eclipse на компьютере разработчика Linux. После завершения этого руководства вы получите приложение для голосования с клиентской частью в виде веб-приложения Java, которое сохраняет результаты голосования во внутренней службе с отслеживанием состояния в кластере.

![Снимок экрана приложения](./media/service-fabric-quickstart-java/votingapp.png)

Из этого краткого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Использование Eclipse в качестве средства для приложений Java Service Fabric.
> * Развертывание приложения в локальном кластере. 
> * Развертывание приложения в кластере Azure
> * Масштабирование приложения на несколько узлов

## <a name="prerequisites"></a>предварительным требованиям
Для работы с этим кратким руководством сделайте следующее:
1. [Установите пакет SDK и интерфейс командной строки Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods).
2. [установите Git](https://git-scm.com/);
3. [Установите Eclipse](https://www.eclipse.org/downloads/).
4. [Настройте среду Java](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), следуя дополнительным шагам по установке подключаемого модуля Eclipse. 

## <a name="download-the-sample"></a>Скачивание примера приложения
В окне терминала выполните следующую команду, чтобы клонировать репозиторий с примером приложения на локальный компьютер.
```
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Локальный запуск приложения
1. Запустите локальный кластер, выполнив следующую команду:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Запуск локального кластера занимает некоторое время. Чтобы убедиться, что кластер является рабочим, откройте Service Fabric Explorer по адресу **http://localhost:19080**. Наличие пяти работоспособных узлов означает, что локальный кластер запущен и работает. 
    
    ![Работоспособное состояние локального кластера](./media/service-fabric-quickstart-java/localclusterup.png)

2. Откройте Eclipse.
3. Щелкните File (Файл) -> Open Projects from File System... (Открыть проекты из файловой системы...). 
4. Щелкните каталог и выберите каталог `Voting` из папки `service-fabric-java-quickstart`, клонированной из GitHub. Нажмите кнопку Готово. 

    ![Диалоговое окно Eclipse "Import" (Импорт)](./media/service-fabric-quickstart-java/eclipseimport.png)
    
5. Теперь у вас имеется проект `Voting` в обозревателе пакетов для Eclipse. 
6. Щелкните правой кнопкой мыши проект и выберите **Publish Application...** (Опубликовать приложение...) в раскрывающемся списке **Service Fabric**. Выберите **PublishProfiles/Local.json** в качестве целевого профиля и нажмите кнопку Publish (Опубликовать). 

    ![Диалоговое окно публикации локально](./media/service-fabric-quickstart-java/localjson.png)
    
7. Откройте избранный веб-браузер и получите доступ к приложению по адресу **http://localhost: 8080**. 

    ![Локальная клиентская часть приложения](./media/service-fabric-quickstart-java/runninglocally.png)
    
Теперь можно добавить варианты для выбора в голосовании и начать прием голосов. Приложение запускается и хранит все данные в кластере Service Fabric без необходимости использования отдельной базы данных.

## <a name="deploy-the-application-to-azure"></a>Развертывание приложения в Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Настройка кластера Azure Service Fabric
Чтобы развернуть приложение в кластере Azure, создайте собственный кластер.

Кластеры сообщества — это бесплатные временные кластеры Service Fabric, размещенные в Azure. Их запускает команда Service Fabric. Любой пользователь может развертывать приложения в этих кластерах и изучать платформу. Чтобы получить доступ к кластеру сообщества, следуйте инструкциям в [этом разделе](http://aka.ms/tryservicefabric). 

Чтобы выполнять операции управления на безопасном общедоступном кластере, можно использовать Service Fabric Explorer, CLI или Powershell. Чтобы использовать Service Fabric Explorer, необходимо скачать с веб-сайта общедоступного кластера PFX-файл и импортировать сертификат в хранилище сертификатов (Windows или Mac) или сам браузер (Ubuntu). Пароль для самозаверяющих сертификатов из общедоступного кластера стороны отсутствует. 

Для выполнения операций управления с помощью Powershell или CLI, требуется PFX-файл (Powershell) или PEM-файл (CLI). Чтобы преобразовать PFX-файл в PEM-файл, используйте следующую команду:  

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

См. дополнительные сведения о [создании кластера Service Fabric в Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> Служба Spring Boot прослушивает входящий трафик через порт 8080. Убедитесь, что порт открыт в кластере. При использовании кластера сообщества этот порт открыт.
>

### <a name="add-certificate-information-to-your-application"></a>Добавление сведений о сертификате в приложение

Отпечаток сертификата необходимо добавить в приложение, чтобы обеспечить использование моделей программирования Service Fabric. 

1. Вам потребуется отпечаток сертификата в файле ```Voting/VotingApplication/ApplicationManiest.xml``` при работе с безопасным кластером. Выполните следующую команду, чтобы извлечь отпечаток сертификата.

    ```bash
    openssl x509 -in [CERTIFICATE_FILE] -fingerprint -noout
    ```

2. В ```Voting/VotingApplication/ApplicationManiest.xml``` добавьте следующий фрагмент кода в тег **ApplicationManifest**. Значение **X509FindValue** должно представлять отпечаток из предыдущего шага (без точки с запятой). 

    ```xml
    <Certificates>
        <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
    </Certificates>   
    ```
    
### <a name="deploy-the-application-using-eclipse"></a>Развертывание приложения с помощью Eclipse
Теперь, когда приложение и кластер готовы, можно развернуть их в кластер напрямую из Eclipse.

1. Откройте файл **Cloud.json** в каталоге **PublishProfiles** и заполните поля `ConnectionIPOrURL` и `ConnectionPort` соответствующим образом. Например: 

    ```bash
    {
         "ClusterConnectionParameters": 
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "[path_to_your_pem_file_on_local_machine]",
            "ClientCert": "[path_to_your_pem_file_on_local_machine]"
         }
    }
    ```

2. Щелкните правой кнопкой мыши проект и выберите **Publish Application...** (Опубликовать приложение...) в раскрывающемся списке **Service Fabric**. Выберите **PublishProfiles/Cloud.json** в качестве целевого профиля и щелкните Publish (Опубликовать). 

    ![Диалоговое окно публикации в облаке](./media/service-fabric-quickstart-java/cloudjson.png)

3. Откройте избранный веб-браузер и получите доступ к приложению по адресу **http://\<IP-адрес_или_URL-адрес_подключения>:8080**. 

    ![Облачная клиентская часть приложения](./media/service-fabric-quickstart-java/runningcloud.png)
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Масштабирование приложений и служб в кластере
Службы могут легко масштабироваться в кластере с учетом изменения нагрузки на службы. Масштабирование службы осуществляется путем изменения числа экземпляров, запущенных в кластере. Существует несколько способов масштабирования служб — вы можете использовать сценарии или команды интерфейса командной строки Service Fabric (sfctl). В этом примере мы используем Service Fabric Explorer.

Service Fabric Explorer выполняется во всех кластерах Service Fabric. Чтобы его открыть, укажите адрес кластера и порт управления кластерами HTTP (19080) в адресной строке, например `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.

Для масштабирования службы веб-интерфейса выполните следующие действия:

1. Откройте Service Fabric Explorer в своем кластере (например, по ссылке `https://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`).
2. Щелкните многоточие рядом с узлом **fabric:/Voting/VotingWeb** в дереве и выберите **Масштабировать службу**.

    ![Масштабирование службы в Service Fabric Explorer](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Теперь вы можете изменить количество экземпляров службы веб-интерфейса.

3. Измените количество на **2** и нажмите кнопку **Масштабировать службу**.
4. Щелкните узел **fabric:/Voting/VotingWeb** в дереве и разверните узел раздела (он отображается в виде идентификатора GUID).

    ![Завершение масштабирования службы в Service Fabric Explorer](./media/service-fabric-quickstart-java/servicescaled.png)

    Теперь вы видите, что у службы есть два экземпляра, а с помощью дерева вы можете определить узлы, на которых запущены эти экземпляры.

С помощью этой простой задачи управления мы удвоили количество ресурсов для обработки пользовательской нагрузки для службы веб-интерфейса. Важно понимать, что для надежной работы службы не требуется запускать несколько экземпляров службы. При сбое в работе службы Service Fabric запускает новый экземпляр службы в кластере.

## <a name="next-steps"></a>Дополнительная информация
Из этого руководства вы узнали, как выполнить следующие действия:

> [!div class="checklist"]
> * Использование Eclipse в качестве средства для приложений Java Service Fabric.
> * Развертывание приложений Java в локальном кластере. 
> * Развертывание приложений Java в кластере Azure.
> * Масштабирование приложения на несколько узлов

* См. дополнительные сведения об [отладке служб в приложении Java с использованием Eclipse](service-fabric-debugging-your-application-java.md).
* См. дополнительные сведения о [настройке непрерывной интеграции и развертывания с использованием Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md).
* Просмотрите другие [примеры Java](https://github.com/Azure-Samples/service-fabric-java-getting-started).