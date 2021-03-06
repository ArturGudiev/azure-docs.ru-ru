---
title: "Свойства пользователя службы совместной работы Azure Active Directory B2B | Документация Майкрософт"
description: "Свойства пользователя службы совместной работы Azure Active Directory B2B можно настраивать."
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 7e1eb709124262d55fc4c6a5bfd8c1ccb33fa8bb
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Свойства пользователя службы совместной работы Azure Active Directory B2B

В Azure Active Directory B2B (бизнес — бизнес) пользователем службы совместной работы называется пользователь с гостевой учетной записью (UserType = Guest). Обычно такой пользователь является представителем партнерской организации и по умолчанию имеет ограниченные права в каталоге приглашающей организации.

В зависимости от потребностей приглашающей организации пользователь службы совместной работы Azure AD B2B может пребывать в одном из следующих состояний учетной записи.

- Состояние 1. Пользователь размещен во внешнем экземпляре Azure AD и представлен в приглашающей организации как гостевой пользователь. В этом случае пользователь B2B входит в систему с учетной записью Azure AD, принадлежащей приглашенному клиенту. Если партнерская организация не использует Azure AD, то создается гостевой пользователь в Azure AD. Для этого требуется, чтобы пользователь активировал приглашение, а служба Azure AD проверила его адрес электронной почты. Такой подход также называется JIT-клиентом или вирусным клиентом.

- Состояние 2. Пользователь размещен в учетной записи Майкрософт и представлен в главной организации как гостевой пользователь. В этом случае гостевой пользователь входит в систему с помощью учетной записи Майкрософт. Внешний идентификатор приглашенного пользователя (учетная запись google.com или любая другая, кроме учетных записей Майкрософт) регистрируется как учетная запись Майкрософт при активации предложения.

- Состояние 3. Пользователь размещен в локальном каталоге Active Directory главной организации и синхронизируется с каталогом Azure AD главной организации. В этом выпуске для такого пользователя нужно вручную изменить атрибут UserType в облаке, используя PowerShell.

- Состояние 4. Пользователь размещен в каталоге Azure AD главной организации с атрибутом UserType = Guest и получил учетные данные, которые управляются главной организацией.

  ![Отображение инициалов приглашающего](media/active-directory-b2b-user-properties/redemption-diagram.png)


Теперь давайте рассмотрим, как в Azure AD выглядит пользователь службы совместной работы Azure AD B2B с состоянием 1.

### <a name="before-invitation-redemption"></a>До активации приглашения

![До активации предложения](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>После активации приглашения

![После активации предложения](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Основные свойства пользователя службы совместной работы Azure AD B2B
### <a name="usertype"></a>UserType
Это свойство указывает связь пользователя с главным клиентом. Оно может иметь два значения:
- Member (Участник). Это сотрудник главной организации, получающий зарплату в этой организации. Например, это пользователь, которому нужен доступ к сайтам только для внутреннего использования. Этот пользователь не должен считаться внешним сотрудником.

- Guest обозначает пользователя, который не считается внутренним пользователем компании, например внешний сотрудник, партнер, клиент и т. п. Такой пользователь не должен, например, получать служебных записок от генерального директора или пользоваться социальным пакетом компании.

  > [!NOTE]
  > Свойство UserType не влияет на то, как пользователь выполняет вход, какие роли он получает в каталоге и т. д. Оно только показывает связь пользователя с главной организацией, и организация может применять политики в зависимости от значения этого свойства.

### <a name="source"></a>Источник
Это свойство указывает, как пользователь выполняет вход.

- Приглашенный пользователь. Это пользователь, который был приглашен, но еще не активировал свое приглашение.

- Внешний каталог Active Directory. Этот пользователь зарегистрирован во внешней организации и проходит проверку подлинности с помощью учетной записи Azure AD, принадлежащей другой организации. Этот тип входа соответствует состоянию 1.

- Учетная запись Майкрософт. У этого пользователя есть учетная запись Майкрософт, которую он использует для проверки подлинности. Этот тип входа соответствует состоянию 2.

- Windows Server Active Directory. Этот пользователь выполняет вход из локального каталога Active Directory, принадлежащего этой организации. Этот тип входа соответствует состоянию 3.

- Azure Active Directory. Этот пользователь проходит проверку подлинности с помощью учетной записи Azure AD, принадлежащей этой организации. Этот тип входа соответствует состоянию 4.
  > [!NOTE]
  > Свойства Source и UserType не зависят друг от друга. Значение атрибута Source не подразумевает определенного значения UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Можно ли добавить пользователей Azure AD B2B в качестве участников (Member), а не гостей (Guest)?
Как правило, понятия "пользователь службы Azure AD B2B" и "гостевой пользователь" являются синонимами. Поэтому пользователь службы совместной работы Azure AD B2B по умолчанию добавляется как гостевой пользователь (UserType = Guest). Однако в некоторых случаях партнерская организация может входить наряду с главной организацией в более крупную организацию. В таких случаях главная организация может рассматривать пользователей из партнерской организации как участников, а не гостей. Воспользуйтесь API-интерфейсами диспетчера приглашений Azure AD B2B, чтобы добавить или пригласить пользователя из партнерской организации в главную организацию как участника.

## <a name="filter-for-guest-users-in-the-directory"></a>Фильтрация гостевых пользователей в каталоге

![Фильтрация гостевых пользователей](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Преобразование UserType
Сейчас преобразовать тип пользователей, меняя значение свойства UserType с Member на Guest и обратно, можно с помощью PowerShell. Но при этом предполагается, что свойство UserType достоверно отражает связь пользователя с организацией. Поэтому значение этого свойства следует изменять только в том случае, если изменяется связь пользователя с организацией. Если связь пользователя с организацией действительно изменилась, не потребуется ли выполнить другие действия, например изменить имя участника-пользователя (UPN)? Сохранит ли пользователь доступ к тем же ресурсам? Нужно ли назначить почтовый ящик? По этим причинам мы не рекомендуем изменять UserType с помощью PowerShell без связи с другими действиями. Кроме того, мы советуем не полагаться на использование этого свойства, так как возможность его изменять с помощью PowerShell может быть отключена.

## <a name="remove-guest-user-limitations"></a>Снятие ограничений для гостевых пользователей
В определенных случаях требуется предоставить гостевым пользователям привилегии более высокого уровня. Можно добавить гостевого пользователя в любую роль или даже удалить заданные по умолчанию ограничения для гостевых пользователей в каталоге. Это позволит ему пользоваться такими же привилегиями, как у участников.

Ограничения для гостевого пользователя, заданные по умолчанию, можно отключить. При этом гостевой пользователь получит те же разрешения в каталоге компании, что и участник.

![Снятие ограничений для гостевых пользователей](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Дополнительная информация

Другие статьи о службе совместной работы Azure AD B2B:

* [Что такое служба совместной работы Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Добавление пользователя службы совместной работы Azure Active Directory B2B в роль](active-directory-b2b-add-guest-to-role.md)
* [Делегирование приглашений для службы совместной работы Azure Active Directory B2B](active-directory-b2b-delegate-invitations.md)
* [Auditing and reporting a B2B collaboration user](active-directory-b2b-auditing-and-reporting.md) (Аудит и создание отчетов для пользователей службы совместной работы B2B)
* [Динамические группы и служба совместной работы Azure Active Directory B2B](active-directory-b2b-dynamic-groups.md)
* [Примеры кода и команд PowerShell для службы совместной работы Azure Active Directory B2B](active-directory-b2b-code-samples.md)
* [Настройка приложений SaaS для службы совместной работы B2B](active-directory-b2b-configure-saas-apps.md)
* [Основные сведения о токенах пользователей в службе совместной работы Azure Active Directory B2B](active-directory-b2b-user-token.md)
* [Сопоставление утверждений пользователя службы совместной работы B2B в Azure Active Directory](active-directory-b2b-claims-mapping.md)
* [Доступ внешних пользователей к Office 365](active-directory-b2b-o365-external-user.md)
* [Текущие ограничения службы совместной работы Azure Active Directory B2B](active-directory-b2b-current-limitations.md)
