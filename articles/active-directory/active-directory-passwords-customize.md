---
title: "Azure Active Directory: настройка самостоятельного сброса пароля"
description: "Параметры настройки самостоятельного сброса пароля в Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: 526286c7f6b62d165af43487ca63fe9055623d0c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Настройка функции самостоятельного сброса пароля в Azure AD

ИТ-специалисты, которые планируют развернуть функцию самостоятельного сброса пароля (SSPR) в Azure Active Directory (Azure AD), могут настроить ее интерфейс в соответствии с потребностями пользователей.

## <a name="customize-the-contact-your-administrator-link"></a>Настройка ссылки "Обратитесь к администратору"

Даже если функция SSPR не включена, пользователи на портале сброса паролей могут воспользоваться ссылкой "Обратитесь к администратору". При нажатии на ссылку произойдет одно из двух:
   * администратору будет отправлено сообщение электронной почты с просьбой помочь изменить пароль пользователя; 
   * пользователям будет отправлен URL-адрес для получения помощи. 

В качестве этого контакта рекомендуем указать адрес электронной почты или веб-сайт, который пользователи уже применяли для отправки вопросов в службу поддержки.

![Контакт][Contact]

Контактный адрес электронной почты отправляется следующим получателям в таком порядке:

1. Уведомление получают пользователи с ролью **Администратор паролей**, если она назначена.
2. Если администраторы паролей не назначены, уведомление получают пользователи с ролью **Администратор пользователей**.
3. Если ни одна из вышеупомянутых ролей не назначена, уведомление получат **глобальные администраторы**.

Во всех случаях уведомление будет отправлено не более чем 100 получателям.

Дополнительные сведения о различных ролях администраторов и их назначении см. в статье [Назначение ролей администратора в Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

### <a name="disable-contact-your-administrator-emails"></a>Отключение отправки электронных сообщений при выборе ссылки "Обратитесь к администратору"

Если организации не требуется уведомлять администраторов о запросах на сброс паролей, можно включить следующую конфигурацию:

* Включение самостоятельного сброса пароля для всех пользователей Этот параметр находится в разделе **Сброс пароля** > **Свойства**.
  
  Если вы не хотите, чтобы пользователи могли сбрасывать пароли, можно ограничить доступ, назначив пустую группу. *Этот вариант использовать не рекомендуется.*
* Настройте ссылку на службу технической поддержки для предоставления URL-адреса или адреса mailto, который пользователи могут использовать для получения помощи. Этот параметр находится в разделе **Сброс пароля** > **Настройка** > **Адрес электронной почты или URL-адрес нестандартной службы технической поддержки**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>Настройка страницы входа в службы федерации Active Directory для SSPR

Администраторы служб федерации Active Directory (AD FS) могут добавить ссылку на страницу входа согласно инструкциям по [добавлению описания страницы входа](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Чтобы добавить ссылку на страницу входа в AD FS, используйте следующую команду на сервере AD FS. С помощью этой страницы пользователи могут начать самостоятельный сброс пароля.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Настройка страницы входа и оформление панели доступа

Страницу входа можно настроить. Вы можете добавить логотип, который будет отображаться вместе с изображением, соответствующим фирменной символике компании.

Выбранные вами изображения отображаются в следующих случаях:

* После того, как пользователь вводит свое имя пользователя.
* Когда пользователь обращается к настраиваемому URL-адресу:
    * при добавлении параметра *whr* — к адресу страницы сброса паролей, например https://login.microsoftonline.com/?whr=contoso.com;
    * при добавлении параметра *username* — к адресу страницы сброса паролей, например https://login.microsoftonline.com/?username=admin@contoso.com.

Сведения о настройке фирменной символики компании см. в статье о [добавлении фирменной символики компании на страницу входа в Azure AD](customize-branding.md).

### <a name="directory-name"></a>Имя каталога

Вы можете изменить атрибут имени каталога в разделе **Azure Active Directory** > **Свойства**. Вы можете указать понятное имя организации, которое будет отображаться на портале и при автоматическом взаимодействии. Эта возможность будет реализована в автоматических сообщениях электронной почты в следующих форматах:

* понятное имя в сообщении электронной почты, например "Майкрософт от имени демонстрационной учетной записи CONTOSO";
* строка темы в сообщении электронной почты, например "Код подтверждения адреса электронной почты демонстрационной учетной записи CONTOSO".

## <a name="next-steps"></a>Дополнительная информация

* [Как развернуть самостоятельный сброс пароля?](active-directory-passwords-best-practices.md)
* [Сброс или изменение пароля](active-directory-passwords-update-your-own-password.md)
* [Регистрация для самостоятельного сброса пароля](active-directory-passwords-reset-register.md)
* [Требования к лицензированию самостоятельного сброса пароля в Azure AD](active-directory-passwords-licensing.md)
* [Какие данные используются для SSPR и какие сведения нужно указывать для пользователей](active-directory-passwords-data.md)
* [Доступные пользователям методы проверки подлинности](active-directory-passwords-how-it-works.md#authentication-methods)
* [Параметры политики для SSPR](active-directory-passwords-policy.md)
* [Что такое обратная запись паролей и каково ее назначение](active-directory-passwords-writeback.md)
* [Как сообщать о действиях в SSPR](active-directory-passwords-reporting.md)
* [Обзор всех параметров SSPR и их значение](active-directory-passwords-how-it-works.md)
* [Как устранить неполадки самостоятельного сброса пароля](active-directory-passwords-troubleshoot.md)
* [Вопросы, не вошедшие в другие статьи](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Пример обращения по электронной почте к администратору для получения помощи при сбросе пароля"
