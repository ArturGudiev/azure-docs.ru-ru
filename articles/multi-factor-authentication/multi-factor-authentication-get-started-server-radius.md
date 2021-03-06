---
title: "Аутентификация RADIUS и сервер Azure MFA | Документация Майкрософт"
description: "Это страница Многофакторной идентификации Azure, которая будет полезна при развертывании проверки подлинности RADIUS и сервера Многофакторной идентификации Azure."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 07b242699e016550c82f491c1678c6dbc9f7234d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>Интеграция аутентификации RADIUS с сервером Многофакторной идентификации Azure

RADIUS — это стандартный протокол принятия и обработки запросов проверки подлинности. Сервер Многофакторной идентификации Azure может выполнять роль сервер RADIUS. Установите его между клиентом RADIUS (например, VPN-устройством) и целевым объектом проверки подлинности, чтобы добавить двухфакторную проверку подлинности. Целевым объектом проверки подлинности может быть каталог Active Directory, каталог LDAP или другой сервер RADIUS. Для нормального функционирования Многофакторной идентификации Azure (MFA) необходимо настроить сервер Azure MFA так, чтобы он мог взаимодействовать как с клиентскими серверами, таки и с целевым объектом аутентификации. Сервер Azure MFA принимает запросы от клиента RADIUS, проверяет учетные данные на целевом объекте проверки подлинности, добавляет Многофакторную идентификацию Azure и отправляет ответ клиенту RADIUS. Запрос на проверку подлинности будет успешно выполнен только в случае успешного прохождения и основной проверки подлинности, и Многофакторной идентификации Azure.

> [!NOTE]
> Выполняя роль RADIUS, сервер MFA поддерживает только такие протоколы RADIUS: PAP (протокол проверки пароля) и MSCHAPv2 (протокол проверки пароля Майкрософт).  Другие протоколы, такие как EAP (расширяемый протокол аутентификации), можно использовать, если сервер MFA выполняет роль прокси-сервера RADIUS для другого сервера RADIUS, который поддерживает этот протокол.
>
> При такой конфигурации односторонние SMS и OATH-токены не будут работать, так как сервер MFA не сможет инициировать успешный ответ на запрос RADIUS с использованием альтернативных протоколов.

![Проверка подлинности RADIUS](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>Добавление клиента RADIUS
Для настройки проверки подлинности RADIUS установите сервер Многофакторной идентификации Azure на сервере Windows. Если имеется среда Active Directory, сервер должен быть присоединен к домену в сети. Для настройки сервера Многофакторной идентификации Azure используйте следующую процедуру.

1. На сервере Многофакторной идентификации Azure щелкните значок аутентификации RADIUS в левом меню.
2. Установите флажок **Включить проверку подлинности RADIUS**.
3. На вкладке "Клиенты" измените порты для аутентификации и учетных данных, если службе Azure MFA RADIUS требуется ожидать передачи данных через нестандартные порты.
4. Щелкните **Добавить**.
5. Введите IP-адрес устройства или сервера, который будет проходить аутентификацию на сервере Многофакторной идентификации Azure, имя приложения (необязательно) и общий секрет.

  Имя приложения отображается в отчетах, а также может отображаться в SMS-сообщениях или сообщениях о проверке подлинности в мобильном приложении.

  Общий секрет должен быть одинаковым на сервере Многофакторной идентификации Azure и на устройстве или сервере.

6. Установите флажок **Требуется сопоставление пользователей многофакторной проверки подлинности**, если все пользователи, импортированные на сервер, подлежат многофакторной проверке подлинности. Если значительное число пользователей еще не импортировано на сервер или исключено из двухфакторной проверки подлинности, не устанавливайте флажок.
7. Установите флажок **Включить резервный OATH-токен**, чтобы использовать секретные коды OATH из мобильных приложений проверки в качестве резервной меры.
8. Последовательно выберите **ОК**.

Для добавления необходимого числа клиентов RADIUS повторите шаги с 4 по 8.

## <a name="configure-your-radius-client"></a>Настройка клиента RADIUS

1. Перейдите на вкладку **Целевой объект**.
2. Если сервер Azure MFA установлен на сервере, присоединенном к домену в среде Active Directory, выберите домен Windows.
3. Если пользователи должны проходить аутентификацию для каталога LDAP, выберите **привязку LDAP**.

  Щелкните значок интеграции каталогов и измените конфигурацию LDAP на вкладке "Параметры", чтобы можно было привязать сервер к каталогу. Инструкции по настройке LDAP можно найти в [руководстве по настройке прокси-сервера LDAP](multi-factor-authentication-get-started-server-ldap.md).

4. Если пользователи должны проходить проверку подлинности в отношении другого сервера RADIUS, выберите сервер (серверы) RADIUS.
5. Щелкните **Добавить**, чтобы настроить сервер, к которому сервер Azure MFA будет передавать запросы RADIUS.
6. В диалоговом окне добавления сервера RADIUS введите IP-адрес сервера RADIUS и общий секрет.

  Общий секрет должен быть одинаковым на сервере Многофакторной идентификации Azure и на сервере RADIUS. Измените порт проверки подлинности и порт учета, если сервер RADIUS используют другие порты.

7. Последовательно выберите **ОК**.
8. Добавьте сервер Azure MFA в качестве клиента RADIUS на другом сервере RADIUS, чтобы он мог обрабатывать запросы на доступ, отправляемые на него с сервера Azure MFA. Необходимо использовать тот же общий секрет, который настроен на сервере Многофакторной идентификации Azure.

Повторите эти шаги, чтобы добавить другие серверы RADIUS. Настройте порядок, в котором сервер Azure MFA должен их вызывать, с помощью кнопок перемещения **вверх** и **вниз**.

Вы успешно настроили сервер Многофакторной идентификации Microsoft Azure. Теперь сервер прослушивает в настроенных портах запросы на доступ RADIUS, поступающие из настроенных клиентов.   

## <a name="radius-client-configuration"></a>Настройка клиента RADIUS
Для настройки клиента RADIUS используйте следующие рекомендации.

* Настройте устройство или сервер для проверки подлинности с помощью RADIUS по IP-адресу сервера Многофакторной идентификации Azure, который будет действовать как сервер RADIUS.
* Используйте тот же общий секрет, который был настроен ранее.
* Установите время ожидания RADIUS в диапазоне 30–60 секунд, чтобы его было достаточно для проверки учетных данных пользователя, двухфакторной проверки подлинности, получения ответа и отправки ответа на запрос на доступ к RADIUS.

## <a name="next-steps"></a>Дополнительная информация

Узнайте, как [интегрировать проверку подлинности RADIUS](multi-factor-authentication-nps-extension.md) со службой Многофакторной идентификации Azure в облаке. 