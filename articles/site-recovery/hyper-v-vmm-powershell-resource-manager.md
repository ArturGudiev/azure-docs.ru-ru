---
title: "Репликация виртуальных машин Hyper-V из облаков VMM на вторичный сайт с помощью PowerShell (Azure Resource Manager) | Документация Майкрософт"
description: "Здесь описывается репликация виртуальных машин Hyper-V из облаков VMM на вторичный сайт VMM с помощью PowerShell (Resource Manager)"
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Репликация виртуальных машин Hyper-V на вторичный сайт с помощью PowerShell (Resource Manager)

В этой статье показано, как автоматизировать шаги для репликации виртуальных машин Hyper-V из облаков System Center Virtual Machine Manager (VMM) в облако VMM на вторичном локальном сайте с помощью [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>предварительным требованиям

- Ознакомьтесь с [архитектурой и компонентами сценария](hyper-v-vmm-architecture.md).
- Ознакомьтесь с [требованиями поддержки ](site-recovery-support-matrix-to-sec-site.md) для всех компонентов.
- Убедитесь, что серверы VMM и узлы Hyper-V соответствуют [требованиям поддержки](site-recovery-support-matrix-to-sec-site.md).
- Убедитесь, что виртуальные машины, которые следует реплицировать, соответствуют [требованиям поддержки реплицируемых машин](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).


## <a name="prepare-for-network-mapping"></a>Подготовка к сопоставлению сетей

[Сопоставление сетей](hyper-v-vmm-network-mapping.md) происходит между локальными сетями виртуальных машин VMM в исходном и целевом облаках. Это предоставляет следующие возможности:

- Подключение виртуальных машин к соответствующим целевым сетям виртуальных машин после отработки отказа. 
- Оптимальное размещение виртуальных машин реплик на целевых серверах узлов Hyper-V. 
- Если не настроить сетевое сопоставление, виртуальные машины реплик не будут подключены к каким-либо сетям виртуальных машин после отработки отказа.

Подготовьте VMM следующим образом.

1. Убедитесь, что на исходном и целевом серверах VMM имеются [логические сети VMM](https://docs.microsoft.com/system-center/vmm/network-logical).
    - Логическая сеть на исходном сервере должна быть связана с исходным облаком, в котором находятся узлы Hyper-V.
    - Логическая сеть на целевом сервере должна быть связана с целевым облаком.
1. Убедитесь, что на исходном и целевом серверах VMM имеются [виртуальные сети VMM](https://docs.microsoft.com/system-center/vmm/network-virtual). Сети виртуальных машин должны быть связаны с логической сетью в каждом расположении.
2. Подключите виртуальные машины на исходных узлах Hyper-V к исходной сети виртуальных машин. 

## <a name="prepare-for-powershell"></a>Подготовка для работы с PowerShell

Перед началом работы убедитесь, что среда Azure PowerShell готова к работе.

- Если вы уже используете PowerShell, необходимо выполнить обновление до версии 0.8.10 или более поздней версии.  Дополнительные сведения о настройке PowerShell см. [здесь](/powershell/azureps-cmdlets-docs).
- После установки и настройки PowerShell проверьте [командлеты службы](/powershell/azure/overview).
- Дополнительные сведения об использовании значений параметров, входных и выходных данных в Azure PowerShell см. в руководстве [по началу работы](/powershell/azure/get-started-azureps).

## <a name="set-up-a-subscription"></a>Настройка подписки
1. В Azure PowerShell войдите в свою учетную запись Azure.

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Получите список подписок вместе с идентификаторами. Запишите идентификатор подписки, в которой необходимо создать хранилище служб восстановления.   

        Get-AzureRmSubscription
3. Настройте подписку для хранилища:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Создание хранилища служб восстановления
1. Если у вас еще нет группы ресурсов Azure Resource Manager, создайте ее.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Создайте новое хранилище служб восстановления и сохраните объект хранилища в переменной, которая будет использоваться далее. 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Объект хранилища можно получить после создания с помощью командлета Get-AzureRMRecoveryServicesVault.

## <a name="set-the-vault-context"></a>Задание контекста хранилища
1. Получите имеющееся хранилище.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Укажите контекст хранилища.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>Установка поставщика Azure Site Recovery
1. На компьютере с VMM создайте каталог, выполнив следующую команду:

       New-Item c:\ASR -type directory
2. Извлеките файлы с помощью загруженного файла установки поставщика: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Начните установку поставщика и дождитесь ее завершения.

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

4. Зарегистрируйте сервер в хранилище.

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Создание и связывание политики репликации
1. Создайте политику репликации (в данном случае для Hyper-V 2012 R2) следующим образом.

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > Облако VMM может содержать узлы Hyper-V под управлением разных версий Windows Server, но политика репликации зависит от версии ОС. Если вы используете узлы на разных версиях операционных систем, создайте отдельные политики репликации для каждой системы. Например, если у вас есть пять узлов, работающих с Windows Server 2012, и три — с Windows Server 2012 R2, то создайте две политики репликации — по одной для каждого типа ОС.

2. Получите первичный контейнер защиты (основного облака VMM) и контейнер защиты для восстановления (облако VMM восстановления).

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Извлеките созданную политику репликации, используя понятное имя.

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Запустите связывание контейнера защиты (облака VMM) с политикой репликации.

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Дождитесь завершения задания связывания политики. Проверить, завершено ли это задание, можно с помощью следующего фрагмента кода PowerShell.

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   После завершения обработки задания выполните следующую команду:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Для проверки выполнения операции выполните действия, описанные в разделе [Мониторинг действий](#monitor-activity).

##  <a name="configure-network-mapping"></a>Настройка сетевого сопоставления
1. Используйте эту команду, чтобы получить серверы для текущего хранилища. Эта команда сохраняет серверы Azure Site Recovery в массиве $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Выполните следующую команду, чтобы получить сети для исходного и целевого серверов VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Исходный сервер VMM может быть первым или вторым в массиве серверов. Проверьте имена серверов VMM и получите сети соответствующим образом.


3. Этот командлет создает сопоставление между основной сетью и сетью восстановления. Он указывает основную сеть в качестве первого элемента $PrimaryNetworks, а сеть восстановления — в качестве первого элемента $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Включение защиты для виртуальных машин
После успешной настройки серверов, облаков и сетей можно включить защиту для виртуальных машин в облаке.

1. Чтобы включить защиту, выполните следующую команду для получения контейнера защиты.

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Получите сущность защиты (виртуальной машины) следующим образом.

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Включите репликацию для виртуальной машины.

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Запуск тестовой отработки отказа

Чтобы протестировать развертывание, можно запустить тестовую отработку отказа для отдельной виртуальной машины или создать план восстановления, состоящий из нескольких виртуальных машин, и запустить тестовую отработку отказа плана. Тестовая обработка отказа имитирует механизм отработки отказа и восстановления в изолированной сети.

1. Получите виртуальную машину, куда будет выполнена отработка отказа виртуальных машин.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Выполните тестовую отработку отказа следующим образом.
    - Для одной виртуальной машины:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
    - Для плана восстановления:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Для проверки выполнения операции выполните действия, описанные в разделе [Мониторинг действий](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Запуск плановых и внеплановых отработок отказа

1. Выполните плановую отработку отказа следующим образом.
    -  Для одной виртуальной машины:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
    - Для плана восстановления:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan
2. Выполните внеплановую отработку отказа.
    - Для одной виртуальной машины:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

    - Для плана восстановления:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Мониторинг действий
Используйте следующие команды для мониторинга действий отработки отказа. Обратите внимание, что между обработкой заданий необходимы паузы для ожидания завершения обработки.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Дополнительная информация

[Узнайте больше](/powershell/module/azurerm.recoveryservices.backup/#recovery) об использовании командлетов PowerShell инструмента Azure Resource Manager для службы Site Recovery.
