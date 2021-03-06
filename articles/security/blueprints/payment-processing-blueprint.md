---
title: "Схема безопасности и соответствия требованиям Azure. Среды для обработки платежей, соответствующие стандарту PCI DSS"
description: "Схема безопасности и соответствия требованиям Azure. Среды для обработки платежей, соответствующие стандарту PCI DSS"
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 2f1e00a8-0dd6-477f-9453-75424d06a1df
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2018
ms.author: frasim
ms.openlocfilehash: 3e97862091e6ea334f2437bd8424b79952f41bf4
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2018
---
# <a name="azure-security-and-compliance-blueprint---pci-dss-compliant-payment-processing-environments"></a>Схема безопасности и соответствия требованиям Azure. Среды для обработки платежей, соответствующие стандарту PCI DSS

## <a name="overview"></a>Обзор

Процесс обработки платежей для PCI DSS-совместимых сред предоставляет руководство по развертыванию совместимой с PCI DSS среды PaaS, подходящей для обработки конфиденциальных данных платежной карты. Он демонстрирует общую эталонную архитектуру и предназначен для упрощения внедрения Microsoft Azure. Эта схема иллюстрирует комплексное решение для удовлетворения потребностей организаций, ищущих облачный подход к снижению нагрузки и уменьшению стоимости развертывания.

Эта схема отвечает требованиям строгих стандартов безопасности данных в индустрии платежных карт (PCI DSS 3.2) для сбора, хранения и получения данных платежной карты. Она демонстрирует надлежащую обработку данных кредитных карт (включая номер карты, срок действия и данные проверки) в безопасной, соответствующей требованиям многоуровневой среде, развернутой как комплексное решение PaaS на основе Azure. Дополнительные сведения о требованиях PCI DSS 3.2 и этом решении см. в статье [PCI DSS Requirements - High-Level Overview](pci-dss-requirements-overview.md) (Общий обзор требований PCI DSS).

Эта схема призвана служить основой, с помощью которой клиенты смогут лучше понять конкретные требования, и не должна использоваться "как есть" в рабочей среде. Развертывания приложения в этой среде без изменений недостаточно для полного соответствия требованиям настраиваемого решения, совместимого с PCI DSS. Обратите внимание на следующее:
- Эта схема обеспечивает своего рода базовые показатели, которые помогают клиентам использовать Microsoft Azure в соответствии с PCI DSS.
- Для соответствия требованиям PCI DSS требуется, чтобы квалифицированный аудитор систем безопасности (QSA) сертифицировал решение для рабочей среды.
- Клиенты несут ответственность за проведение соответствующих проверок безопасности и соответствия любого решения, созданного с использованием этой базовой архитектуры, поскольку требования могут варьироваться в зависимости от специфики реализации и географического расположения каждого клиента.  

Краткий обзор того, как работает это решение, содержится в этом [видео](https://aka.ms/pciblueprintvideo). В нем объясняется и демонстрируется развертывание решения.

## <a name="solution-components"></a>Компоненты решения

Базовая архитектура состоит из следующих компонентов:

- **Схема архитектуры**. На схеме показана эталонная архитектура, используемая для решения интернет-магазина Contoso.
- **Шаблоны развертывания**. В этом развертывании [шаблоны Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview#template-deployment) используются для автоматического развертывания компонентов архитектуры в Microsoft Azure путем указания параметров конфигурации во время установки.
- **Автоматизированное развертывание скриптов**. Эти скрипты помогают развернуть комплексное решение. Скрипты состоят из:
    - Скрипт установки модуля и настройки [глобального администратора](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) позволяет установить и проверить правильность настройки необходимых модулей PowerShell и ролей глобального администратора.
    - Сценарий установки PowerShell используется для развертывания комплексного решения, предоставляемого в виде ZIP-файла и BACPAC-файла, которые содержат предварительно созданную демонстрационную версию веб-приложения с [примером содержимого базы данных SQL](https://github.com/Microsoft/azure-sql-security-sample) . Исходный код этого решения доступен для изучения в [репозитории кода схемы][code-repo]. 

## <a name="architectural-diagram"></a>Схема архитектуры

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Пользовательский сценарий

Схема предназначена для приведенного ниже варианта использования.

> Этот сценарий иллюстрирует, как фиктивный веб-сайт переместил обработку своих платежных карт в решение PaaS на основе Azure. Решение выполняет сбор базовой информации пользователя, включая данные об оплате. Решение не обрабатывает платежи с данными держателя карты. После сбора данных клиенты несут ответственность за инициирование и завершение транзакций с помощью обработчика платежей. Дополнительные сведения см. на странице [Azure Blueprint Program](https://aka.ms/pciblueprintprocessingoverview) (Программа Azure Blueprint).

### <a name="use-case"></a>Вариант использования 
Небольшой *интернет-магазин Contoso* готов переместить свою платежную систему в облако. Владельцы магазина выбрали Microsoft Azure для размещения процесса покупки и предоставления клерку возможности получения платежей по кредитным картам от своих клиентов.

Администратор ищет решение, которое можно быстро развернуть для достижения своих целей в отношении облачного решения. Он будет использовать это подтверждение концепции (POC), чтобы обсудить с заинтересованными лицами, как Azure может использоваться для сбора, хранения и получения данных платежной карты при соблюдении строгих требований стандарта безопасности данных платежной индустрии (PCI DSS).

> Вы будете нести ответственность за проведение соответствующих проверок безопасности и соответствия любого решения, созданного на основе архитектуры, используемой этим POC, поскольку требования могут варьироваться в зависимости от специфики реализации и вашего географического расположения. Для соответствия требованиям PCI DSS нужно, чтобы квалифицированный аудитор систем безопасности сертифицировал решение для рабочей среды.

### <a name="elements-of-the-foundational-architecture"></a>Элементы базовой архитектуры

Базовая архитектура разработана со следующими фиктивными элементами:

Сайт домена `contosowebstore.com`

Роли пользователей используются для иллюстрации варианта использования и позволяют ознакомиться с пользовательским интерфейсом.

#### <a name="role-site-and-subscription-admin"></a>Роль: администратор сайта и подписки

|Элемент      |Пример|
|----------|------|
|Имя пользователя: |`adminXX@contosowebstore.com`|
| Имя: |`Global Admin Azure PCI Samples`|
|Тип пользователя:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- Пользователь с учетной записью администратора не может считывать сведения о кредитной карте без маски. Все действия регистрируются.
- Пользователь с учетной записью администратора не может управлять базой данных SQL или входить в нее.
- Пользователь с учетной записью администратора может управлять службой Active Directory и подпиской.

#### <a name="role-sql-administrator"></a>Роль: администратор SQL

|Элемент      |Пример|
|----------|------|
|Имя пользователя: |`sqlAdmin@contosowebstore.com`|
| Имя: |`SQLADAdministrator PCI Samples`|
| Имя: |`SQL AD Administrator`|
|Фамилия: |`PCI Samples`|
|Тип пользователя:| `Administrator`|

- Пользователь с учетной записью sqladmin не может просматривать неотфильтрованные сведения о кредитной карте. Все действия регистрируются.
- Пользователь с учетной записью sqladmin может управлять базой данных SQL.

#### <a name="role-clerk"></a>Роль: клерк

|Элемент      |Пример|
|----------|------|
|Имя пользователя:| `receptionist_EdnaB@contosowebstore.com`|
| Имя: |`Edna Benson`|
| Имя:| `Edna`|
|Фамилия:| `Benson`|
| Тип пользователя: |`Member`|

Эдна Бенсон — секретарь и бизнес-менеджер. Она отвечает за выставление счетов и точность сведений о клиенте. Это пользователь, который выполняет вход для всех действий с демо-сайтом интернет-магазина Contoso. Эдна имеет следующие права: 

- создание и чтение информации о клиентах;
- изменение информации о клиентах;
- перезаписывание или изменение номера кредитной карты, срока действия и кода безопасности карты.

### <a name="contoso-webstore---estimated-pricing"></a>Интернет-магазин Contoso — примерная стоимость

Эта базовая архитектура и пример веб-приложения имеют ежемесячную структуру оплаты и стоимость использования в час, которые необходимо учитывать при определении размера решения. Эти затраты можно рассчитать с помощью [калькулятора расценок Azure](https://azure.microsoft.com/pricing/calculator/). Начиная с сентября 2017 года расчетная месячная стоимость этого решения составляет примерно 2500 долл. США, включая 1000 долл. США в месяц за использование ASE версии 2. Эти затраты будут зависеть от объема использования и могут быть изменены. Клиенту необходимо рассчитать свои ожидаемые ежемесячные затраты на момент развертывания для более точной оценки. 

Это решение использует приведенные ниже службы Azure. Подробные сведения об архитектуре развертывания см. в [этом разделе](#deployment-architecture).

>- Шлюз приложений
>- Azure Active Directory
>- Среда службы приложений версии 2
>- OMS Log Analytics.
>- Хранилище ключей Azure
>- группы сетевой безопасности;
>- База данных Azure SQL
>- Azure Load Balancer
>- Application Insights
>- Центр безопасности Azure
>- Веб-приложение Azure
>- Служба автоматизации Azure
>- Модули Runbook со службой автоматизации Azure
>- Azure DNS
>- Виртуальная сеть Azure
>- Виртуальная машина Azure
>- Группа ресурсов и политики Azure
>- Хранилище больших двоичных объектов Azure
>- Управление доступа на основе ролей Azure Active Directory (RBAC)

## <a name="deployment-architecture"></a>Архитектура развертывания

В следующем разделе подробно описываются компоненты разработки и реализации.

### <a name="network-segmentation-and-security"></a>Сегментация сети и безопасность

![](images/pci-tiers-diagram.png)

#### <a name="application-gateway"></a>Шлюз приложений

Базовая архитектура снижает риск уязвимостей безопасности с помощью шлюза приложений с брандмауэром веб-приложения (WAF) и включенным набором правил OWASP. Доступны следующие дополнительные возможности:

- [сквозное SSL-подключение](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell);
- включение [разгрузки SSL](/azure/application-gateway/application-gateway-ssl-portal);
- отключение [TLS версий 1.0 и 1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell);
- [брандмауэр веб-приложения](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (режим WAF);
- [режим предотвращения](/azure/application-gateway/application-gateway-web-application-firewall-portal) с набором правил OWASP 3.0;
- включение [ведения журналов диагностики](/azure/application-gateway/application-gateway-diagnostics);
- [пользовательские пробы работоспособности](/azure/application-gateway/application-gateway-create-gateway-portal);
- [Центр безопасности Azure](https://azure.microsoft.com/services/security-center) и [Помощник Azure](/azure/advisor/advisor-security-recommendations) обеспечивают дополнительную защиту и уведомления. Центр безопасности Azure также предоставляет систему репутации.

#### <a name="virtual-network"></a>Виртуальная сеть

Базовая архитектура определяет частную виртуальную сеть с пространством адресов 10.0.0.0/16.

#### <a name="network-security-groups"></a>Группы безопасности сети

Каждый из уровней сети имеет выделенную группу безопасности сети (NSG):
- группа безопасности сети периметра для брандмауэра и WAF шлюза приложения;
- группа безопасности сети для среды управления Jumpbox (узел-бастион);
- группа безопасности сети для среды службы приложений.

В каждой группе безопасности сети (NSG) открыты определенные порты и протоколы для безопасной и правильной работы решения. Дополнительные сведения см. в разделе [Группы безопасности сети](#network-security-groups).

В каждой группе безопасности сети (NSG) открыты определенные порты и протоколы для безопасной и правильной работы решения. Кроме того, для каждой NSG настроены следующие конфигурации:
- включенные [журналы диагностики и события](/azure/virtual-network/virtual-network-nsg-manage-log) хранятся в учетной записи хранения; 
- OMS Log Analytics подключена к [диагностике NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

#### <a name="subnets"></a>Подсети
 Убедитесь, что каждая подсеть связана с соответствующей NSG.

#### <a name="custom-domain-ssl-certificates"></a>SSL-сертификаты личного домена
 Трафик HTTPS разрешен с использованием SSL-сертификата личного домена.

### <a name="data-at-rest"></a>Неактивные данные

Архитектура защищает неактивные данные, используя шифрование, проверку базы данных и другие меры.

#### <a name="azure-storage"></a>Хранилище Azure

Для выполнения требований шифрования неактивных данных все [хранилища Azure](https://azure.microsoft.com/services/storage/) используют [шифрование службы хранения](/azure/storage/storage-service-encryption).

#### <a name="azure-sql-database"></a>Базы данных SQL Azure

В экземпляре базы данных Azure SQL используются следующие меры безопасности базы данных:

- [аутентификация и авторизация AD](/azure/sql-database/sql-database-aad-authentication);
- [аудит баз данных SQL](/azure/sql-database/sql-database-auditing-get-started);
- [Transparent Data Encryption (Прозрачное шифрование данных)](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)
- [правила брандмауэра](/azure/sql-database/sql-database-firewall-configure) для обеспечения пулов рабочих ролей ASE и управления IP клиента;
- [обнаружение угроз SQL](/azure/sql-database/sql-database-threat-detection-get-started);
- [столбцы Always Encrypted](/azure/sql-database/sql-database-always-encrypted-azure-key-vault);
- [динамическое маскирование данных базы данных SQL Azure](/azure/sql-database/sql-database-dynamic-data-masking-get-started) с использованием скрипта PowerShell после развертывания.

### <a name="logging-and-auditing"></a>Ведение журналов и аудит

[Operations Management Suite (OMS)](/azure/operations-management-suite/) может предоставить для интернет-магазина Contoso подробные журналы всех действий системы и пользователя, включая данные владельца карты. Здесь можно просмотреть все изменения и проверить их правильность. 

- **Журналы действий**. [Журналы действий](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) содержат информацию об операциях, которые выполнялись с ресурсами в подписке.
- **Журналы диагностики**. [Журналы диагностики](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) — это все журналы, создаваемые ресурсом. Эти журналы включают в себя системные журналы событий Windows, хранилища BLOB-объектов, таблиц и очередей.
- **Журналы брандмауэра**. Шлюз приложений предоставляет все журналы доступа и диагностики. Журналы брандмауэра доступны для ресурсов шлюза приложений с включенным WAF.
- **Архивация журналов**. Все журналы диагностики настроены для записи в централизованную зашифрованную учетную запись Azure для архивирования на определенный период хранения (2 дня). Затем журналы подключаются к Azure Log Analytics для обработки, хранения и отображения данных на панелях мониторинга. [Log Analytics](https://azure.microsoft.com/services/log-analytics) — это служба OMS, которая помогает собирать и анализировать данные, создаваемые ресурсами в облачных и локальных средах.

### <a name="encryption-and-secrets-management"></a>Шифрование и управление секретами

Интернет-магазин Contoso шифрует все данные кредитной карты и использует Azure Key Vault для управления ключами, предотвращая извлечение данных владельца карты.

- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. 
- [Прозрачное шифрование данных SQL](/sql/relational-databases/security/encryption/transparent-data-encryption) используется для шифрования всех данных владельца карты, даты окончания срока действия и кода безопасности карты.
- Данные хранятся на диске с использованием [шифрования дисков Azure](/azure/security/azure-security-disk-encryption) и BitLocker.

### <a name="identity-management"></a>Управление удостоверениями

Следующие технологии обеспечивают возможности управления удостоверениями в среде Azure.
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) — это мультитенантный облачный каталог и служба управления удостоверениями корпорации Майкрософт. Все пользователи решения создаются в Azure Active Directory, включая пользователей, обращающихся к базе данных SQL.
- Аутентификация приложения выполняется с использованием Azure AD. Дополнительные сведения см. в статье [Интеграция приложений с Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications). Кроме того, при шифровании столбцов базы данных также используется Azure AD для аутентификации приложения в базе данных Azure SQL. Дополнительные сведения см. в статье [Always Encrypted: защита конфиденциальных данных в Базе данных SQL и хранение ключей шифрования в хранилище ключей Azure](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Защита идентификации Azure Active Directory](/azure/active-directory/active-directory-identityprotection) выявляет потенциальные уязвимости, влияющие на удостоверения вашей организации, настраивает автоматическое реагирование на обнаруженные подозрительные действия, связанные с удостоверениями вашей организации, исследует подозрительные инциденты и предпринимает соответствующие действия для их устранения.
- [Контроль доступа на основе ролей (RBAC) Azure](/azure/active-directory/role-based-access-control-configure) обеспечивает точное управление доступом для Azure. Доступ к подписке ограничен администратором подписки, а доступ к Azure Key Vault запрещен для всех пользователей.

Подробнее об использовании функций безопасности базы данных SQL Azure см. в примере [демонстрационного приложения Contoso Clinic](https://github.com/Microsoft/azure-sql-security-sample).
   
### <a name="web-and-compute-resources"></a>Веб-ресурсы и вычислительные ресурсы

#### <a name="app-service-environment"></a>Среда службы приложений

[Служба приложений Azure](/azure/app-service/) является управляемой службой для развертывания веб-приложений. Приложение интернет-магазина Contoso развертывается в качестве [веб-приложения службы приложений](/azure/app-service-web/app-service-web-overview).

[Среда службы приложений Azure (ASE версии 2)](/azure/app-service/app-service-environment/intro) — это компонент службы приложений Azure, предоставляющий полностью изолированную и выделенную среду для безопасного выполнения приложений службы приложений в большом масштабе. Это тарифный план уровня "Премиум", используемый этой базовой архитектурой для обеспечения соответствия PCI DSS.

Среды ASE изолированы, позволяют запускать приложения только одного клиента и всегда развернуты в виртуальной сети. Клиенты могут детально управлять входящим и исходящим сетевым трафиком приложений, а приложения могут создавать высокоскоростные безопасные подключения к локальным корпоративным ресурсам через виртуальные сети.

Использование сред ASE для этой архитектуры позволило использовать следующие средства управления и конфигурации:

- размещение внутри защищенной виртуальной сети и использование правил безопасности сети;
- настройка ASE с самозаверяющимся сертификатом ILB для подключений HTTPS;
- [внутренняя балансировка нагрузки](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (режим 3);
- отключение [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings) — протокол TLS является устаревшим с точки зрения PCI DSS;
- изменение [шифров TLS](/azure/app-service-web/app-service-app-service-environment-custom-settings);
- управление [портами N/W для входящего трафика](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic); 
- [WAF — ограничения доступа к данным](/azure/app-service-web/app-service-app-service-environment-web-application-firewall);
- разрешение [трафика базы данных SQL](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview).


#### <a name="jumpbox-bastion-host"></a>Jumpbox (узел-бастион)

Поскольку среда службы приложений защищена и заблокирована, необходим механизм, позволяющий разрешать любые выпуски DevOps или изменения, которые могут потребоваться, например возможность отслеживать веб-приложение с помощью Kudu. Виртуальная машина защищена с помощью балансировщика нагрузки NAT, который позволяет подключать виртуальную машину на порту, отличному от TCP 3389. 

Виртуальная машина создана как jumpbox (узел-бастион) со следующими конфигурациями:

-   [расширение защиты от вредоносных программ](/azure/security/azure-security-antimalware);
-   [расширение OMS](/azure/virtual-machines/virtual-machines-windows-extensions-oms);
-   [Расширение системы диагностики Azure](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [шифрование дисков Azure](/azure/security/azure-security-disk-encryption) с помощью Azure Key Vault (соблюдает требования Azure для государственных организаций, PCI DSS, HIPAA и другие требования);
-   [политика автоматического завершения работы](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) для уменьшения степени использования ресурсов виртуальной машины во время простоя.

### <a name="security-and-malware-protection"></a>Безопасность и защита от вредоносных программ

[Центр безопасности Azure](https://azure.microsoft.com/services/security-center/) предоставляет полное представление о состоянии безопасности всех своих ресурсов в Azure. Вы можете убедиться, что все необходимые настройки заданы правильно, а также найти ресурсы, которым нужно уделить внимание.  

[Azure Advisor](/azure/advisor/advisor-overview) — это персонализированный облачный консультант, который поможет следовать рекомендациям по оптимизации развернутых служб Azure. Он анализирует конфигурацию ресурсов и данные телеметрии их использования и рекомендует решения, которые помогут повысить эффективность затрат, производительность, уровень доступности и безопасности ресурсов Azure.

[Антивредоносная программа (Майкрософт)](/azure/security/azure-security-antimalware) для облачных служб и виртуальных машин Azure — это средство защиты для обнаружения и удаления вирусов, шпионских и других вредоносных программ в режиме реального времени. Кроме того, вы можете настроить оповещения о попытках установки вредоносного или нежелательного программного обеспечения в системе Azure.

### <a name="operations-management"></a>Operations Management

#### <a name="application-insights"></a>Application Insights

[Application Insights](https://azure.microsoft.com/services/application-insights/) позволяет получать практическую информацию с помощью средств управления производительностью приложений и мгновенной аналитики.

#### <a name="log-analytics"></a>Служба Log Analytics

[Log Analytics](https://azure.microsoft.com/services/log-analytics/) — это служба в Operations Management Suite (OMS), которая помогает собирать и анализировать данные, создаваемые ресурсами в облачных и локальных средах.

#### <a name="oms-solutions"></a>Решения OMS

Следует учесть и настроить следующие дополнительные решения OMS:
- [Анализ журнала действий](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
- [Анализ сетевой активности Azure](/azure/log-analytics/log-analytics-azure-networking-analytics?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Azure SQL Analytics](/azure/log-analytics/log-analytics-azure-sql)
- [Отслеживание изменений](/azure/log-analytics/log-analytics-change-tracking?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Анализ хранилища ключей](/azure/log-analytics/log-analytics-azure-key-vault?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Схема услуги](/azure/operations-management-suite/operations-management-suite-service-map)
- [Безопасность и аудит](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Защита от вредоносных программ](/azure/log-analytics/log-analytics-malware?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Управление обновлениями](/azure/operations-management-suite/oms-solution-update-management)

### <a name="security-center-integration"></a>Интеграция центра безопасности

Развертывание по умолчанию предназначено для обеспечения базовых рекомендаций центра безопасности, указывающих работоспособное и безопасное состояние конфигурации. Узнайте, как включать сбор данных в Центре безопасности Azure. Дополнительные сведения см. в статье [Краткое руководство по центру безопасности Azure](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>Развертывание решения

Компоненты для развертывания этого решения доступны в [репозитории кода для проекта PCI][code-repo]. Для развертывания базовой архитектуры требуется несколько шагов, выполняемых с помощью Microsoft PowerShell версии 5. Чтобы подключиться к веб-сайту, вы должны предоставить имя личного домена (например, contoso.com). Его нужно указать с помощь параметра `-customHostName` на шаге 2. Дополнительные сведения см. в статье [Приобретение имени личного домена для веб-приложений Azure](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Имя личного домена не требуется для успешного развертывания и запуска решения, но вы не сможете подключиться к веб-сайту в демонстрационных целях.

Скрипты добавляют пользователей домена в указанный вами клиент Azure AD. Рекомендуется создать клиент Azure AD для использования в качестве тестового образца.

Если возникли проблемы во время развертывания, см. страницу [вопросов и ответов и сведения об устранении неполадок](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

Настоятельно рекомендуется использовать чистую установку PowerShell для развертывания решения. Кроме того, убедитесь, что вы используете последние модули, необходимые для правильного выполнения скриптов установки. В этом примере вы войдете в jumpbox (узел-бастион) и выполните следующие команды. Обратите внимание, что это позволяет выполнять команду личного домена.


1. Установите необходимые модули и правильно настройте роли администратора.
 
    ```powershell
     .\0-Setup-AdministrativeAccountAndPermission.ps1 
        -azureADDomainName contosowebstore.com
        -tenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -subscriptionId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -configureGlobalAdmin 
        -installModules
    ```
    Подробные сведения об использовании см. в статье [с описанием настройки учетной записи администратора и разрешений](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/0-Setup-AdministrativeAccountAndPermission.md).
    
2. Установите средство для управления обновлениями решения. 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
        -resourceGroupName contosowebstore
        -globalAdminUserName adminXX@contosowebstore.com 
        -globalAdminPassword **************
        -azureADDomainName contosowebstore.com 
        -subscriptionID XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
        -suffix PCIcontosowebstore
        -customHostName contosowebstore.com
        -sqlTDAlertEmailAddress edna@contosowebstore.com 
        -enableSSL
        -enableADDomainPasswordPolicy 
    ```
    
    Инструкции по использованию см. в статье [о развертывании и настройке ресурсов Azure](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md).
    
3. Ведение журналов OMS и мониторинг. После развертывания решения можно открыть рабочее пространство [Microsoft Operations Management Suite (OMS)](/azure/operations-management-suite/operations-management-suite-overview), а шаблоны примеров, предоставленные в репозитории решений, можно использовать для иллюстрации того, как можно настроить панель мониторинга. Примеры шаблонов OMS см. в папке [omsDashboards](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Обратите внимание на то, что для правильного развертывания шаблонов должны собираться данные в OMS. Это может занять один час или больше, в зависимости от активности сайта.
 
    При настройке ведения журналов OMS рассмотрите возможность включения этих ресурсов:
 
    - Microsoft.Network/applicationGateways
    - Microsoft.Network/NetworkSecurityGroups
    - Microsoft.Web/serverfarms
    - Microsoft.Sql/servers/databases
    - Microsoft.Compute/virtualMachines
    - Microsoft.Web/sites
    - Microsoft.KeyVault/Vaults
    - Microsoft.Automation/automationAccounts
 

    
## <a name="threat-model"></a>Модель рисков

Схема потока данных (DFD) и пример модели угроз для [модели угроз для схемы](https://aka.ms/pciblueprintthreatmodel) интернет-магазина Contoso.

![](images/pci-threat-model.png)



## <a name="customer-responsibility-matrix"></a>Таблица обязанностей клиента

Клиенты несут ответственность за сохранение копии [итоговой таблицы ответственности](https://aka.ms/pciblueprintcrm32), в которой изложены требования PCI DSS, за которые отвечает заказчик, и те, за которые отвечает Microsoft Azure.

## <a name="pci-compliance-review"></a>Проверка соответствия требованиям PCI 

Решение было проверено компанией Coalfire Systems, Inc. (квалифицированные аудиторы систем безопасности по требованиям PCI DSS). В статье [PCI Compliance Review](https://aka.ms/pciblueprintprocessingoverview) (Проверка соответствия требованиям PCI) описывается проверка аудитором решения и рекомендации для преобразования проекта в развертывание, готовое к рабочей среде.

## <a name="disclaimer-and-acknowledgements"></a>Отказ от ответственности и подтверждения

*Сентябрь 2017 г.*

- Этот документ является исключительно информационным. МАЙКРОСОФТ И AVYAN НЕ ПРЕДОСТАВЛЯЕТ НИКАКИХ ГАРАНТИЙ, ЯВНЫХ, КОСВЕННЫХ ИЛИ ПРЕДУСМОТРЕННЫХ ЗАКОНОМ, В ОТНОШЕНИИ ИНФОРМАЦИИ В ЭТОМ ДОКУМЕНТЕ. Данный документ предоставляется "как есть". Сведения и мнения, представленные в данном документе, включая URL-адреса и ссылки на другие веб-сайты, могут быть изменены без предварительного уведомления. Клиенты, читающие этот документ, берут ответственность за его использование на себя.  
- Настоящий документ не предоставляет клиентам юридических прав на интеллектуальную собственность в отношении продуктов или решений корпорации Майкрософт или Avyan.  
- Клиенты могут скопировать и использовать данный документ для внутренних справочных целей.  

  > [!NOTE]
  > Выполнение некоторых рекомендаций в этом документе может привести к более интенсивному использованию данных, а также сетевых и вычислительных ресурсов в Azure, а значит и к дополнительным затратам на лицензии или подписки Azure.  

- Решение, описанное в этой статье, предназначено для использования в качестве базовой архитектуры и не должно использоваться в рабочей среде "как есть". Для соответствия требованиям PCI клиентам нужно проконсультироваться с квалифицированным аудитором систем безопасности.  
- Все имена клиентов, записи транзакций и любые связанные с ними данные на этой странице являются фиктивными, созданы для базовой архитектуры и предоставлены только для иллюстрации. Любое сходство с реальными ситуациями случайно.  
- Это решение было разработано совместно корпорацией Майкрософт и Avyan Consulting и поставляется с [лицензией MIT](https://opensource.org/licenses/MIT).
- Это решение проверила компания Coalfire, аудитор PCI-DSS в Майкрософт. В статье о [проверке соответствия требованиям PCI](https://aka.ms/pciblueprintcrm32) представлена независимая проверка решения и компонентов, которые следует учитывать. 

### <a name="document-authors"></a>Авторы документа

- *Фрэнк Симорджей (Frank Simorjay) (Майкрософт)*  
- *Гурурай Пандуранги (Gururaj Pandurangi) (Avyan Consulting)*


[code-repo]: https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms "Репозиторий кода"
