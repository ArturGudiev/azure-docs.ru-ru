---
title: "Управление доступом к приложениям с помощью Azure AD | Документация Майкрософт"
description: "Описывает, как Azure Active Directory позволяет организациям задавать приложения, к которым имеет доступ каждый пользователь."
services: active-directory
documentationcenter: 
author: femila
manager: mtillman
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 363478cb1bb2637a8c4f9cf36505817b163df781
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="managing-access-to-apps"></a>Управление доступом к приложениям
Непрерывное управление доступом, оценка использования и ведение отчетов продолжают вызывать трудности и после интеграции приложения в систему удостоверений вашей организации. Во многих случаях ИТ-администраторы или сотрудники службы поддержки должны принимать постоянное и активное участие в управлении доступом к приложениям. В некоторых случаях назначение выполняется общим ИТ-отделом или ИТ-отделом подразделения. Во многих случаях решение о назначении должно быть направлено на рассмотрение ответственному лицу, прежде чем ИТ-специалисты осуществят такое назначение.  Другие организации вкладывают средства в интеграцию с существующей автоматической системой управления удостоверениями и доступом, такой как контроль доступа на основе ролей (RBAC) или контроль доступа на основе атрибутов (ABAC). Как интеграция, так и разработка правил требуют специальных знаний и существенных затрат. А реализация мониторинга и ведения отчетов при любом из этих подходов к управлению является отдельной сложной и дорогостоящей задачей.

## <a name="how-does-azure-active-directory-help"></a>Чем в такой ситуации может помочь Azure Active Directory?
 Azure AD поддерживает расширенное управление доступом к настроенным приложениям, позволяя организациям легко устанавливать подходящие политики доступа — от автоматического назначения на основе атрибутов (сценарии ABAC или RBAC) до делегирования и управления администраторами. С помощью Azure AD можно легко реализовать сложные политики, объединяя несколько моделей управления для одного приложения, а также повторно использовать правила управления для разных приложений с одной аудиторией.

* [Добавление новых или существующих приложений](active-directory-enterprise-apps-manage-sso.md)

 Назначение приложения Azure AD сосредоточено на двух основных режимах назначения:

* **Отдельное назначение**. ИТ-администратор с разрешениями глобального администратора каталога может выбрать отдельные учетные записи пользователей и предоставить им доступ к приложению.
* **Назначение на основе группы (только в платной версии Azure AD)**. ИТ-администратор с разрешениями глобального администратора каталога может назначить группу приложению. Наличие доступа у отдельных пользователей определяется тем, являются ли они членами группы на момент попытки получения доступа к приложению. Иными словами, администратор может создать правило назначения, в котором указано, что "все текущие члены назначенной группы имеют доступ к приложению". При применении этого варианта назначения администраторы могут использовать любые возможности управления группами Azure AD, включая [основанные на атрибутах динамические группы](active-directory-groups-create-azure-portal.md), группы внешних систем (например локальная версия Active Directory или Workday), а также группы, управляемые администратором или самостоятельно. Одну группу можно легко назначить нескольким приложениям, в результате чего приложения со сходством назначения могут совместно использовать правила назначения, что снижает общий уровень сложности управления. Обратите внимание на то, что на данный момент членство во вложенных группах не поддерживается для назначения приложений на основе групп.

Используя эти два режима назначения, администраторы могут реализовать любой подход к управлению назначением.

В С Azure AD ведение отчетов по использованию и назначению полностью интегрировано, что позволяет администраторам легко формировать отчеты о состоянии назначения, ошибках назначения и даже об использовании.

## <a name="complex-application-assignment-with-azure-ad"></a>Сложное назначение приложений с помощью Azure AD
Рассмотрим такое приложение, как Salesforce. Во многих организациях приложение Salesforce главным образом используется специалистами по маркетингу и продажам. Часто сотрудники маркетингового отдела имеют привилегированный доступ к Salesforce, а сотрудники отдела продаж — ограниченный доступ. Во многих случаях ограниченный доступ к приложению также имеет обширная группа информационных работников. Исключения из этих правил усложняют ситуацию. Часто обязанности по предоставлению пользователям доступа или изменению их ролей, когда требуется отклониться от универсальных правил, лежат на руководителях отделов маркетинга и продаж.

С помощью Azure AD такие приложения, как Salesforce, могут быть предварительно настроены для единого входа и автоматической подготовки к работе. После настройки приложения администратор может однократно создать и назначить соответствующие группы. В этом примере администратор может выполнить указанные ниже назначения.

* [Динамические группы](active-directory-groups-create-azure-portal.md) можно определить таким образом, чтобы они автоматически представляли всех членов групп маркетинга и продаж с помощью таких атрибутов, как отдел или роль.
  
  * Всем членам групп маркетинга в Salesforce будет назначена роль marketing.
  * Всем членам групп продаж в Salesforce будет назначена роль sales. Для дальнейшего уточнения можно использовать несколько групп, представляющих региональные группы продаж, которым назначены различные роли Salesforce.
* Чтобы включить механизм исключений, для каждой роли можно создать группу самообслуживания. Например, можно создать группу "Salesforce marketing exception" в качестве группы самообслуживания. Этой группе можно назначить роль marketing в Salesforce, а команду руководителей по маркетингу можно назначить в качестве владельцев. Члены команды руководителей по маркетингу могут добавлять или удалять пользователей, задавать политику присоединения и даже утверждать или отклонять запросы на присоединение от отдельных пользователей. Это согласуется с обычным рабочим процессом информационных работников, так как не требует специального обучения владельцев или членов.

В этом случае все назначенные пользователи будут автоматически подготовлены для Salesforce; поскольку они добавляются в разные группы, их назначения ролей в Salesforce будут обновлены. Пользователи смогут найти и использовать Salesforce через панель доступа к приложениям корпорации Майкрософт, веб-клиенты Office или даже со страницы своей организации в Salesforce. Администраторы смогут легко просмотреть состояние использования и назначения с помощью отчетов Azure AD.

Администраторы могут применять [условный доступ Azure AD](active-directory-conditional-access-azure-portal.md) , чтобы задавать политики доступа для конкретных ролей. Эти политики могут указывать, разрешен ли доступ вне корпоративной среды, а также включать в себя многофакторную проверку подлинности или требования к устройствам для обеспечения доступа в различных ситуациях.

## <a name="how-can-i-get-started"></a>Как начать работу?
Прежде всего, если вы еще не используете Azure AD и являетесь ИТ-администратором:

* [Попробуйте](https://azure.microsoft.com/trial/get-started-active-directory/)  — вы можете зарегистрироваться сегодня, чтобы использовать бесплатную пробную версию в течение 30 дней и развернуть первое облачное решение в течение 5 минут, применив эту ссылку.

Ниже перечислены компоненты Azure AD, обеспечивающие совместное использование учетных записей.

* [Назначение группы](active-directory-accessmanagement-self-service-group-management.md)
* Добавление приложений в Azure AD
* Приступая к работе с назначением
* Часто задаваемые вопросы по назначению приложений
* [Панели мониторинга и отчеты об использовании приложения](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Где можно получить дополнительную информацию?
* [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)
* [Защита приложений с помощью условного доступа](active-directory-conditional-access-azure-portal.md)
* [Самостоятельное управление группами/SSAA](active-directory-accessmanagement-self-service-group-management.md)

