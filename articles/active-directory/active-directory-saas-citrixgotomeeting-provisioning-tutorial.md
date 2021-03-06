---
title: "Руководство по настройке GoToMeeting для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 48b8072b5ebe61f5a9dccd9d8ea31e5a6945f265
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Руководство по настройке GoToMeeting для автоматической подготовки пользователей

Цель этого руководства — показать, как настроить автоматическую подготовку и отмену подготовки учетных записей пользователей Azure AD в GoToMeeting.

## <a name="prerequisites"></a>предварительным требованиям

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

*   клиент Azure Active Directory;
*   подписка GoToMeeting с поддержкой единого входа;
*   учетная запись пользователя в GoToMeeting с разрешениями администратора команды.

## <a name="assigning-users-to-gotomeeting"></a>Назначение пользователей в GoToMeeting

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи будут синхронизированы только те записи и группы, которые были "назначены" приложению в Azure AD.

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи или группы в Azure AD представляют пользователей, которым требуется доступ к приложению GoToMeeting. После этого можно назначить этих пользователей для приложения GoToMeeting, выполнив следующие действия:

[Назначение корпоративному приложению пользователя или группы](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Важные рекомендации по назначению пользователей для приложения GoToMeeting

*   Мы рекомендуем назначить одного пользователя Azure AD в GoToMeeting для тестирования конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

*   При назначении пользователя для GoToMeeting необходимо выбрать допустимую роль пользователя. Роль "Доступ по умолчанию" не работает для подготовки.

## <a name="enable-automated-user-provisioning"></a>Включение автоматической подготовки пользователей

В этом разделе описывается подключение к API подготовки учетной записи пользователя Azure AD в GoToMeeting и настройка подготовки службы для создания, обновления и отмены назначения учетных записей пользователей в GoToMeeting на основе назначения пользователей и групп в Azure AD.

> [!TIP]
> Для GoToMeeting также можно включить единый вход на основе SAML. Для этого следуйте инструкциям, приведенным на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-account-provisioning"></a>Настройка автоматической подготовки учетных записей пользователей:

1. На [портале Azure](https://portal.azure.com) перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

2. Если вы уже настроили единый вход для GoToMeeting, найдите свой экземпляр GoToMeeting с помощью поиска. В противном случае щелкните **Добавить** и найдите **GoToMeeting** в коллекции приложений. Выберите GoToMeeting в результатах поиска и добавьте это приложение в свой список приложений.

3. Выберите свой экземпляр GoToMeeting и перейдите на вкладку **Подготовка**.

4. Для параметра **Режим подготовки** выберите значение **Автоматический**. 

    ![Подготовка](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. В разделе "Учетные данные администратора" выполните следующие действия:
   
    a. В текстовом поле **Имя пользователя администратора GoToMeeting** введите имя пользователя администратора.

    Б. В текстовом поле **Пароль администратора GoToMeeting** введите пароль администратора.

6. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению GoToMeeting. Если подключение отсутствует, убедитесь, что у учетной записи GoToMeeting есть права администратора команды, и повторите шаг **"Учетные данные администратора"**.

7. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок.

8. Нажмите кнопку **Сохранить**.

9. В разделе "Сопоставления" выберите действие **Синхронизировать пользователей Azure Active Directory в GoToMeeting**.

10. В разделе **Сопоставления атрибутов** просмотрите пользовательские атрибуты, которые синхронизированы из Azure AD в GoToMeeting. Атрибуты, выбранные как свойства **сопоставления**, используются для сопоставления учетных записей пользователей в GoToMeeting для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

11. Чтобы включить службу подготовки Azure AD для GoToMeeting, в разделе "Параметры" установите переключатель **Состояние подготовки** в положение **Включено**.

12. Нажмите кнопку **Сохранить**.

После этого будет запущена начальная синхронизация всех пользователей и групп, которых вы назначили в GoToMeeting в разделе "Пользователи и группы". Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Последующие операции синхронизации выполняются примерно каждые 40 минут, при условии, что служба запущена. В разделе **Сведения о синхронизации** можно отслеживать ход синхронизации и с помощью ссылок просматривать отчеты о подготовке, в которых описаны все действия, выполняемые службой подготовки для приложения GoToMeeting.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Настройка единого входа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrix-gotomeeting-tutorial)


