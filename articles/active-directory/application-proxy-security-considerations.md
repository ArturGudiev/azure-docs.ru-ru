---
title: "Вопросы безопасности для прокси приложения Azure AD | Документы Майкрософт"
description: "В этой статье рассматриваются вопросы безопасности при использовании прокси приложения Azure AD."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 2370c4717e2cff6b4b8113b09624ef873b309647
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2018
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Вопросы безопасности при удаленном доступе к приложениям через прокси приложения Azure AD

В этой статье описаны компоненты, которые защищают пользователей и приложения при использовании прокси приложения Azure Active Directory.

На следующей схеме показано, как Azure AD обеспечивает безопасный удаленный доступ к локальным приложениям.

 ![Схема безопасного удаленного доступа через прокси приложения Azure AD](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a>Преимущества безопасности

Прокси приложения Azure AD предоставляет следующие преимущества безопасности.

### <a name="authenticated-access"></a>Аутентифицируемый доступ 

Если вы решили использовать предварительную аутентификацию Azure Active Directory, то только проверенные подключения будут иметь доступ к вашей сети.

Прокси приложения Azure AD во всех случаях использует для проверки подлинности службу маркеров безопасности Azure AD.  Предварительная проверка подлинности организована так, что она автоматически блокирует значительное количество анонимных атак. Доступ к внутреннему приложению получают только удостоверения, прошедшие проверку подлинности.

Если выбрать сквозной режим для предварительной аутентификации, это преимущество будет отсутствовать. 

### <a name="conditional-access"></a>Условный доступ

Примените расширенные параметры политики, прежде чем устанавливать подключения к сети.

Благодаря [условному доступу](active-directory-conditional-access-azure-portal-get-started.md) можно определить ограничения в отношении трафика, который может передаваться к внутренним приложениям. Вы можете создать политики, ограничивающие вход на основе расположения, надежности аутентификации и профиля риска пользователя.

С помощью условного доступа можно также настроить политики Многофакторной идентификации, добавив еще один уровень безопасности при проверке подлинности пользователей. 

### <a name="traffic-termination"></a>Завершение трафика

Весь трафик завершается в облаке.

Прокси приложения Azure AD работает как обратный прокси-сервер, поэтому в этой службе завершается весь трафик, направленный к внутренним приложениям. Сеанс возобновляется только для внутренних серверов, то есть эти внутренние серверы не получают трафик HTTP напрямую. Эта конфигурация означает повышенную защиту от целенаправленных атак.

### <a name="all-access-is-outbound"></a>Только исходящий доступ 

Для корпоративной сети не требуется открывать входящие подключения.

Соединители прокси приложения используют только исходящие подключения к службе прокси приложения Azure AD, поэтому нет необходимости открывать порты брандмауэра для входящих подключений. Для традиционных прокси требовалась сеть периметра (другие названия — *DMZ*, *демилитаризованная зона* и *промежуточная подсеть*) и разрешать на границе сети доступ для подключений без проверки подлинности. Это требовало расходов на брандмауэры веб-приложения, которые анализируют трафик и защищают среду. Благодаря прокси приложения можно отказаться от сети периметра, так как теперь все подключения являются исходящими и передаются по защищенному каналу.

Дополнительные сведения о соединителях см. в статье [Сведения о соединителях прокси приложения Azure AD](application-proxy-understand-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Облачная аналитика и машинное обучение 

Получите новейшую систему безопасности.

Являясь частью Azure Active Directory, прокси приложения может использовать [защиту идентификации Azure AD](active-directory-identityprotection.md) с данными из Microsoft Security Response Center и Digital Crimes Unit. Вместе мы в упреждающем режиме выявляем скомпрометированные учетные записи и обеспечиваем защиту от входов, представляющих высокий уровень риска. Мы учитываем множество факторов, чтобы определить, какие попытки входа связаны с высоким риском. К этим факторам относятся помеченные флагами зараженные устройства, анонимизация сетей и нетипичные или маловероятные расположения.

Многие из этих отчетов и событий уже сейчас можно использовать в API для интеграции с системами управления сведениями о безопасности и событиями (SIEM).

### <a name="remote-access-as-a-service"></a>Удаленный доступ как услуга

Вам больше не нужно беспокоиться о поддержке локальных серверов и установке для них исправлений.

Программное обеспечение, на котором не установлены исправления, по-прежнему подвергается большому количеству атак. Прокси приложения Azure AD — это глобальная служба корпорации Майкрософт, которая гарантирует применение новейших исправлений и обновлений безопасности.

Чтобы повысить уровень безопасности приложений, опубликованных в прокси приложения Azure AD, мы запрещаем программам-обходчикам индексировать и архивировать ваши приложения. Каждый раз, когда программа-обходчик пытается получить параметры автоматической обработки для опубликованного приложения, прокси приложения отправляет ей файл robots.txt, который содержит `User-agent: * Disallow: /`.

### <a name="ddos-prevention"></a>Предотвращение атак DDoS

Приложения, опубликованные посредством прокси приложения, защищены от распределенных атак типа "отказ в обслуживании" (DDoS).

Служба прокси приложения отслеживает объем трафика, направляемого в ваши приложения и сеть. Если резко возрастает количество устройств, запрашивающих удаленный доступ к вашим приложениям, корпорация Майкрософт регулирует доступ к вашей сети. 

Корпорация Майкрософт отслеживает шаблоны трафика для отдельных приложений и для вашей подписки в целом. Если одно приложение получает больше запросов, чем обычно, то на короткое время запросы на доступ к этому приложению отклоняются. Если во всей подписке вы получите больше запросов, чем обычно, то запросы на доступ к любым вашим приложениям будут отклоняться. Эта предупредительная мера предотвращает перегрузку серверов приложений запросами на удаленный доступ, чтобы локальные пользователи по-прежнему имели доступ к своим приложениям. 

## <a name="under-the-hood"></a>Как все устроено

Прокси приложения Azure AD состоит из двух частей:

* Облачная служба, выполняемая в Azure, в которой устанавливают подключения внешние клиенты или пользователи.
* [Локальный соединитель](application-proxy-understand-connectors.md). Этот локальный компонент ожидает передачи запросов от службы прокси приложения Azure AD и обрабатывает подключения к внутренним приложениям. 

Между соединителем и службой прокси приложения устанавливается поток данных. Это происходит в следующих случаях:

* При первоначальной настройке соединителя.
* Соединитель извлекает информацию о конфигурации из службы прокси приложения.
* Пользователь получает доступ к опубликованному приложению.

>[!NOTE]
>Весь обмен данными осуществляется по протоколу SSL и всегда инициируется соединителем службы прокси приложения. В службе используются только исходящие соединения.

Почти для всех вызовов соединитель использует сертификат клиента, чтобы пройти проверку подлинности в службе прокси приложения. Единственным исключением является этап начальной настройки, когда устанавливается сертификат клиента.

### <a name="installing-the-connector"></a>Установка соединителя

При начальной настройке соединителя выполняются события из следующей последовательности.

1. В процессе установки соединителя происходит его регистрация в службе. Пользователям предлагается ввести учетные данные администратора Azure AD. Полученный после этой аутентификации маркер затем предъявляется службе прокси приложения Azure AD.
2. Служба прокси приложения оценивает этот маркер. Она проверяет, является ли пользователь администратором организации в клиенте. Если пользователь не является администратором, процесс завершается.
3. Соединитель создает запрос на сертификат клиента и передает его вместе с маркером в прокси приложения службы. В свою очередь, служба проверяет этот маркер и подписывает запрос на сертификат клиента.
4. Соединитель использует этот сертификат клиента для последующего обмена данными со службой прокси приложения.
5. Соединитель обращается к службе, чтобы получить данные о конфигурации системы, предоставляя сертификат клиента. Теперь все готово, чтобы принимать запросы.

### <a name="updating-the-configuration-settings"></a>Обновление параметров конфигурации

Каждый раз, когда служба прокси приложения обновляет параметры конфигурации, выполняются события из следующей последовательности.

1. Соединитель подключается к конечной точке конфигурации в службе прокси приложения, передавая свой сертификат клиента.
2. После проверки сертификата клиента служба прокси приложения возвращает в соединитель данные о конфигурации (например, в какую группу должен входить этот соединитель).
3. Если с даты начала действия текущего сертификата прошло больше 180 дней, соединитель создает новый запрос сертификата. Таким образом, сертификат клиента обновляется каждые 180 дней.

### <a name="accessing-published-applications"></a>Доступ к опубликованным приложениям

При доступе пользователей к опубликованному приложению служба прокси приложения и соединитель прокси приложения взаимодействуют следующим образом.

1. [Служба аутентифицирует пользователя для приложения](#the-service-checks-the-configuration-settings-for-the-app)
2. [Служба помещает запрос в очередь соединителя.](#The-service-places-a-request-in-the-connector-queue)
3. [Соединитель обрабатывает запрос из очереди.](#the-connector-receives-the-request-from-the-queue)
4. [Соединитель ожидает ответ.](#the-connector-waits-for-a-response)
5. [Служба передает пользователю поток данных.](#the-service-streams-data-to-the-user)

Чтобы узнать больше о том, что происходит на каждом из этих этапов, читайте эту статью дальше.


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a>1. Служба аутентифицирует пользователя для приложения

Если вы настроили приложение для использования сквозного режима в качестве предварительной аутентификации, то действия в этом разделе можно пропустить.

Если вы настроили приложение для использования предварительной аутентификации с помощью Azure AD, то пользователи перенаправляются для аутентификации в службу токенов безопасности Azure AD. При этом выполняются следующие операции.

1. Прокси приложения проверяет соответствие всем требованиям политики условного доступа для конкретного приложения. Этот шаг позволяет гарантировать, что пользователь назначен приложению. Если требуется двухфакторная проверка подлинности, появляется запрос на второй метод проверки подлинности.

2. Когда все проверки выполнены, служба токенов безопасности Azure AD выдает для приложения подписанный токен и перенаправляет пользователя обратно в службу прокси приложения.

3. Прокси приложения проверяет, что токен выдан правильному приложению. Служба проверяет и другие данные, например подпись Azure AD и срок действия маркера.

4. Прокси приложения создает зашифрованный файл cookie для проверки подлинности, который будет подтверждать, что проверка подлинности в приложении успешно выполнена. Этот файл cookie содержит метку времени, которая обозначает окончание срока его действия, вычисляемое по данным маркера Azure AD и другим данным, таким как имя пользователя, по которому выполнялась проверка подлинности. Файл cookie шифруется с помощью закрытого ключа, известного только службе прокси приложения.

5. Прокси приложения перенаправляет пользователя к исходному запрошенному URL-адресу.

Если на любом из этапов предварительной проверки подлинности происходит сбой, запрос пользователя отклоняется с сообщением о причинах проблемы.


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a>2. Служба помещает запрос в очередь соединителя

Соединители не закрывают исходящее подключение к службе прокси приложения. При поступлении запроса служба добавляет его в очередь в одном из открытых подключений, чтобы его получил соединитель.

Запрос содержит данные для приложения, например заголовки запроса и данные из зашифрованного файла cookie, имя пользователя, отправившего запрос, и идентификатор запроса. Хотя данные из зашифрованного файла cookie отправляются вместе с запросом, сам файл cookie аутентификации не передается.

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a>3. Соединитель обрабатывает запрос из очереди 

На основе запроса прокси приложения выполняет одно из следующих действий.

* Если запрос является простой операцией (например, когда в тексте нет данных, как в случае с операцией *GET* RESTful), соединитель подключается к целевому внутреннему ресурсу и ожидает ответа.

* Если в тексте запроса содержатся данные, как в случае с операцией *POST* RESTful, соединитель устанавливает исходящее подключение к экземпляру прокси приложения, используя сертификат клиента. Это подключение используется для запроса данных и открытия подключения к внутреннему ресурсу. Получив запрос от соединителя, служба прокси приложения начинает принимать содержимое от пользователя и перенаправляет данные в соединитель. Соединитель, в свою очередь, перенаправляет данные во внутренний ресурс.

#### <a name="4-the-connector-waits-for-a-response"></a>4. Соединитель ожидает ответ

Когда завершается выполнение запроса и передача содержимого в серверную службу, соединитель ожидает ответа.

Получив этот ответ, соединитель устанавливает исходящее подключение к службе прокси приложения, чтобы передать сведения о заголовке и начать потоковую передачу возвращаемых данных.

#### <a name="5-the-service-streams-data-to-the-user"></a>5. Служба передает пользователю поток данных 

На этом этапе приложение может выполнять определенную обработку. Если вы настроили прокси приложения для преобразования заголовков или URL-адресов в приложении, то при необходимости эта обработка выполняется на данном этапе.


## <a name="next-steps"></a>Дополнительная информация

[Аспекты топологии сети при использовании прокси приложения Azure AD](application-proxy-network-topology-considerations.md)

[Сведения о соединителях прокси приложения Azure AD](application-proxy-understand-connectors.md)
