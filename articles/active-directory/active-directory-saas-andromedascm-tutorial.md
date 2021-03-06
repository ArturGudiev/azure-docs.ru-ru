---
title: "Руководство по интеграции Azure Active Directory с Andromeda SCM | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и Andromeda SCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7a142c86-ca0c-4915-b1d8-124c08c3e3d8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: jeedes
ms.openlocfilehash: 72b66eec34995c334c6d65a1d03637fe21b9dc80
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda-scm"></a>Руководство по интеграции Azure Active Directory с Andromeda SCM

В этом руководстве описано, как интегрировать Andromeda SCM с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Andromeda SCM обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Andromeda SCM.
- Вы можете включить автоматический вход пользователей в Andromeda SCM (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>предварительным требованиям

Чтобы настроить интеграцию Azure AD с Andromeda SCM, вам потребуется:

- подписка Azure AD;
- подписка Andromeda SCM с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Andromeda SCM из коллекции.
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-andromeda-scm-from-the-gallery"></a>Добавление Andromeda SCM из коллекции
Чтобы настроить интеграцию Andromeda SCM с Azure AD, необходимо добавить Andromeda SCM из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Andromeda SCM из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **Andromeda SCM**. На панели результатов выберите **Andromeda SCM** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Andromeda SCM в списке результатов](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение Andromeda SCM с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в Andromeda SCM соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Andromeda SCM.

Чтобы настроить и проверить единый вход Azure AD в Andromeda SCM, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Andromeda SCM](#create-an-andromeda-scm-test-user)** нужно для того, чтобы в Andromeda SCM существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Andromeda SCM.

**Чтобы настроить единый вход Azure AD в Andromeda SCM, сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **Andromeda SCM** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_samlbase.png)

3. Чтобы настроить приложение в режиме, **инициированном поставщиком удостоверений**, в разделе **Домены и URL-адреса приложения Andromeda SCM** сделайте следующее.

    ![Сведения о домене и URL-адресах единого входа приложения Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<tenantURL>`

    Б. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<tenantURL>`.

4. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах единого входа приложения Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_url1.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<tenantURL>/SAMLLogon.aspx`
     
    > [!NOTE] 
    > Приведенное выше значение используется только для примера. Замените их на фактические значения идентификатора, URL-адреса ответа и URL-адреса входа, которые описываются далее в этом руководстве.

5. Приложение Andromeda SCM ожидает утверждения SAML в определенном формате. Настройте следующие утверждения для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На следующем снимке экрана приведен пример.
    
    ![Настройка атрибута единого входа](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_attribute.png)

    > [!Important]
    > Очистите определения пространств имен при настройке.
    
6. В разделе **Атрибуты пользователя** диалогового окна **Единый вход** настройте атрибут токена SAML, как показано на рисунке, и выполните следующие действия.
    
    | Имя атрибута | Значение атрибута |
    | ------------------- | -------------------- |    
    | role        | DEMO |
    | Тип        | DEFAULT |
    | company       | COMP02    |

    > [!NOTE]
    > Значения, указанные выше, приведены в качестве примера. Эти значения представлены для демонстрации. Используйте роли своей организации.

    a. Щелкните **Добавить атрибут**, чтобы открыть диалоговое окно **Добавление атрибута**.

    ![Добавление атрибута для настройки единого входа](./media/active-directory-saas-andromedascm-tutorial/tutorial_attribute_04.png)

    ![Добавление атрибута для настройки единого входа](./media/active-directory-saas-andromedascm-tutorial/tutorial_attribute_05.png)

    Б. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. В списке **Значение** выберите значение атрибута, отображаемое для этой строки.

    d. Оставьте пустым поле **Пространство имен**.
    
    д. Нажмите кнопку **ОК**.

7. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Ссылка для скачивания сертификата](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_certificate.png) 

8. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/active-directory-saas-andromedascm-tutorial/tutorial_general_400.png)
    
9. В разделе **Настройка Andromeda SCM** щелкните **Настроить Andromeda SCM**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_configure.png)

10. Войдите на сайт компании Andromeda SCM от имени администратора.

11. В верхней части строки меню щелкните **Admin** (Администратор) и выберите **Administration** (Администрирование).

    ![Раздел Admin (Администратор) приложения Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_admin.png)

12. Слева в разделе **Interfaces** (Интерфейсы) щелкните **SAML Configuration** (Настройка SAML).

    ![Раздел SAML приложения Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_saml.png)

13. На странице раздела **SAML Configuration** (Настройка SAML) сделайте следующее:

    ![Настройка Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_config.png)

    a. Установите флажок **Enable SSO with SAML** (Включить единый вход с помощью SAML).

    Б. В разделе **Andromeda Information** (Сведения о приложении Andromeda) скопируйте **идентификатор удостоверения поставщика услуг** и вставьте его в текстовое поле **Идентификатор** раздела **Домены и URL-адреса приложения Andromeda SCM**.

    c. Скопируйте **URL-адрес потребителя** и вставьте его в текстовое поле **URL-адрес ответа** раздела **Домены и URL-адреса приложения Andromeda SCM**.

    d. Скопируйте **URL-адрес входа** и вставьте его в текстовое поле **URL-адрес входа** раздела **Домены и URL-адреса приложения Andromeda SCM**.

    д. В разделе **SAML Identity Provider** (Поставщик удостоверений SAML) введите имя поставщика удостоверений.

    f. В текстовое поле **Single Sign On End Point** (Конечная точка единого входа) вставьте значение **URL-адреса службы единого входа SAML**, скопированное на портале Azure.

    ж. Откройте в Блокноте **сертификат в кодировке Base64**, загруженный с портала Azure, и вставьте его в текстовое поле **Сертификат X 509**.
    
    h. Сопоставьте следующие атрибуты с соответствующим значением для выполнения единого входа через Azure AD. Для входа требуется атрибут **User ID** (Идентификатор пользователя). Для подготовки необходимо указать атрибуты: **Email** (Электронный адрес), **Company** (Компания), **UserType** (Тип пользователя) и **Role** (Роль). В этом разделе определяются сопоставления атрибутов (имя и значения), которые соответствуют тем, что указаны на портале Azure.

    ![Сопоставление атрибутов в Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    i. Выберите команду **Сохранить**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-an-andromeda-scm-test-user"></a>Создание тестового пользователя Andromeda SCM.

Цель этого раздела — создать в приложении Andromeda SCM пользователя с именем Britta Simon. Приложение Andromeda SCM поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. При попытке получить доступ к приложению Andromeda SCM создается учетная запись пользователя (если она еще не создана).

>[!Note]
>Чтобы создать пользователя вручную, обратитесь в [службу поддержки Andromeda SCM](https://www.ngcsoftware.com/support/).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Andromeda SCM, чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Andromeda SCM, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **Andromeda SCM**.

    ![Ссылка на Andromeda SCM в списке приложений](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Andromeda SCM на панели доступа, вы автоматически войдете в приложение Andromeda SCM.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_203.png

