---
title: "Архитектура присоединенного к домену кластера Azure HDInsight | Документация Майкрософт"
description: "Сведения о планировании архитектуры присоединенного к домену кластера HDInsight."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2017
ms.author: bprakash
ms.openlocfilehash: 5285199d22528ed6b9fa3b7dbc85e382e7b28569
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Планирование архитектуры присоединенных к домену кластеров Hadoop в Azure HDInsight

Стандартный кластер HDInsight рассчитан на одного пользователя. Он подходит для большинства организаций с небольшими отделами по работе с приложениями, создающими объемные рабочие нагрузки данных. С ростом популярности кластера Hadoop многие организации переходят на новый режим работы. В этом режиме ИТ-специалисты управляют кластерами, которые совместно используются несколькими отделами разработки приложений. Таким образом, возможности работы с многопользовательскими кластерами стали одними из самых запрашиваемых в Azure HDInsight.

Вместо создания собственной многопользовательской проверки подлинности и авторизации HDInsight полагается на самого популярного поставщика удостоверений — Active Directory (AD). Расширенные возможности безопасности в AD можно использовать для управления многопользовательской аутентификацией в HDInsight. Интегрировав HDInsight с AD, вы сможете взаимодействовать с кластерами, используя свои учетные данные AD. Виртуальная машина в HDInsight присоединяется к службе AD через домен, и HDInsight сопоставляет пользователя AD с локальным пользователем Hadoop. Благодаря этому аутентифицированный пользователь может легко переключаться между всеми пользовательскими службами, запущенными в HDInsight (Ambari, сервер Hive, Ranger, сервер Thrift Spark и т. д.). Администраторы могут настраивать строгие политики авторизации на основе Apache Ranger, чтобы применять управление доступом на основе ролей для ресурсов в HDInsight.


## <a name="integrate-hdinsight-with-active-directory"></a>Интеграция HDInsight с Active Directory

При интеграции HDInsight с Active Directory узлы кластера HDInsight присоединяются к домену AD. Для включенных в кластер компонентов Hadoop настраиваются параметры безопасности Kerberos. Для каждого компонента Hadoop в Active Directory создается субъект-служба. Также субъект создается для каждого компьютера, присоединенного к домену. Такие субъекты-службы и субъекты компьютеров могут нарушить организацию в Active Directory. Чтобы устранить эту проблему, создайте в Active Directory подразделение и перенесите в него все эти субъекты. 

Кратко перечислим, что вам нужно создать в сетевом окружении:

- контроллер домена Active Directory с настроенным LDAPS;
- подключение из виртуальной сети HDInsight к контроллеру домена Active Directory;
- подразделение в Active Directory;
- учетную запись службы со следующими разрешениями:

    - на создание субъектов-служб в подразделении;
    - на присоединение компьютеров к домену и создание субъектов компьютеров в подразделении.

На приведенном ниже снимке экрана представлено такое подразделение в домене contoso.com. Также вы видите здесь несколько субъектов-служб и субъектов компьютеров.

![Подразделение в кластере HDInsight, присоединенном к домену](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="two-ways-of-bringing-your-own-active-directory-domain-controllers"></a>Два способа предоставить контроллеры домена Active Directory

Есть два способа предоставить контроллеры домена Active Directory для создания кластеров HDInsight, присоединенных к домену: 

- **Доменные службы Azure Active Directory.** Эта служба предоставляет управляемый домен Active Directory, который полностью совместим с Windows Server Active Directory. Корпорация Майкрософт управляет доменом AD, применяет исправления к домену и осуществляет мониторинг домена. Вы можете развернуть свой кластер, не беспокоясь о поддержке контроллеров домена. Пользователи, группы и пароли синхронизируются из Azure Active Directory. Это позволяет пользователям входить в кластер с обычными корпоративными учетными данными. Дополнительные сведения см. в статье [Настройка присоединенных к домену кластеров HDInsight с помощью доменных служб Azure Active Directory](./apache-domain-joined-configure-using-azure-adds.md).

- **Active Directory на виртуальных машинах Azure IaaS.** В этом сценарии вы развертываете и контролируете собственный домен Windows Server Active Directory на виртуальных машинах Azure IaaS. Дополнительные сведения см. в статье [Настройка среды с присоединенной к домену песочницей HDInsight](./apache-domain-joined-configure.md).

## <a name="next-steps"></a>Дополнительная информация
* Сведения о настройке присоединенного к домену кластера HDInsight см. в [этой статье](apache-domain-joined-configure.md).
* Сведения об управлении присоединенными к домену кластерами HDInsight см. в [этой статье](apache-domain-joined-manage.md).
* Сведения о настройке политик Hive и выполнении запросов Hive см. в статье [Настройка политик Hive в присоединенном к домену кластере HDInsight (предварительная версия)](apache-domain-joined-run-hive.md).
* Сведения о выполнении запросов Hive с помощью SSH в присоединенных к домену кластерах HDInsight см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
