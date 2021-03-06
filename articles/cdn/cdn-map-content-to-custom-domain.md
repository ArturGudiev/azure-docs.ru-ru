---
title: "Добавление личного домена к конечной точке CDN | Документация Майкрософт"
description: "Узнайте, как сопоставлять содержимое Azure CDN с личным доменом."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: mazha
ms.openlocfilehash: ec53b91b8aba4e38a8f7cb4b010d6be2a62150d5
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2017
---
# <a name="add-a-custom-domain-to-your-cdn-endpoint"></a>Добавление личного домена к конечной точке CDN
После создания профиля вы обычно создаете одну или несколько конечных точек CDN (поддомен azureedge.net) для передачи содержимого с использованием протоколов HTTP и HTTPS. По умолчанию эта конечная точка включена во всех URL-адресах (например, `http(s)://contoso.azureedge.net/photo.png`). Для удобства Azure CDN предоставляет возможность связать личный домен (например, `www.contoso.com`) с конечной точкой. Таким образом вы можете использовать для доставки содержимого личный домен вместо конечной точки. Эта возможность полезна, например, если вы хотите, чтобы ваше собственное доменное имя могли видеть ваши клиенты (для целей популяризации бренда).

Если у вас еще нет личного домена, сначала нужно приобрести его у поставщика доменов. Получив личный домен, выполните следующие действия:
1. [Получите доступ к записям DNS поставщика домена](#step-1-access-dns-records-by-using-your-domain-provider).
2. [Создайте записи DNS CNAME](#step-2-create-the-cname-dns-records).
    - Вариант 1. Прямое сопоставление личного домена с конечной точкой CDN
    - Вариант 2. Сопоставление личного домена с конечной точкой CDN с помощью поддомена **cdnverify** 
3. [Включите сопоставление записи CNAME в приложении Azure](#step-3-enable-the-cname-record-mapping-in-azure).
4. [Проверьте, что личный поддомен ссылается на конечную точку CDN](#step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint).
5. [Сопоставьте постоянный личный домен с конечной точкой CDN (зависимый шаг)](#step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint).

## <a name="step-1-access-dns-records-by-using-your-domain-provider"></a>Шаг 1. Доступ к записям DNS с помощью поставщика домена

Если вы используете Azure для размещения [доменов DNS](https://docs.microsoft.com/azure/dns/dns-overview), вам следует делегировать DNS поставщика домена в Azure DNS. Дополнительные сведения см. в статье [Делегирование домена в Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

В противном случае при использовании поставщика домена для обработки домена DNS войдите на сайт своего поставщика. Найдите страницу для управления записями DNS. Для этого ознакомьтесь с документацией поставщика или поищите области веб-сайта, обозначенные как **Доменное имя**, **DNS** или **Name Server Management** (Управление сервером доменных имен). Часто страницу записей DNS можно найти, открыв сведения о своей учетной записи и найдя такую ссылку, как **Мои домены**. Некоторые поставщики имеют разные ссылки для добавления записей различных типов.

> [!NOTE]
> У некоторых поставщиков, например GoDaddy, изменения записей DNS не вступают в силу, пока вы не щелкнете ссылку **Сохранить изменения**. 


## <a name="step-2-create-the-cname-dns-records"></a>Шаг 2. Создание записей DNS CNAME

Перед использованием личного домена с конечной точкой Azure CDN сначала необходимо создать запись канонического имени (CNAME) поставщика домена. Это тип записи в службе доменных имен (DNS), которая сопоставляет исходный домен с целевым доменом, указывая имя домена псевдонима для "канонического" или истинного доменного имени. Для Azure CDN исходный домен является вашим личным доменом (и поддоменом), а целевой домен — это конечная точка CDN. Azure CDN проверяет запись DNS CNAME при добавлении личного домена в конечную точку с помощью портала или API. 

Запись CNAME сопоставляет определенный домен и поддомен, например `www.contoso.com` или `cdn.contoso.com`. Невозможно сопоставить запись CNAME с корневым доменом, например `contoso.com`. Поддомен может быть связан только с одной конечной точкой CDN. Созданная запись CNAME перенаправляет весь трафик, адресованный поддомену, в указанную конечную точку. Например, если сопоставить `www.contoso.com` с конечной точкой CDN, то его нельзя будет сопоставить с другими конечными точками Azure, к примеру с конечной точкой облачной службы или конечной точкой учетной записи хранения. Однако можно использовать разные поддомены одного и того же домена для разных конечных точек служб. Кроме того, можно сопоставлять разные поддомены с одной конечной точкой CDN.

Для сопоставления личного домена с конечной точкой CDN используйте один из следующих вариантов:

- Вариант 1. Прямое сопоставление личного домена с конечной точкой CDN Если в личном домене нет рабочего трафика, вы можете напрямую сопоставить личный домен с конечной точкой CDN. Однако процесс сопоставления личного домена с конечной точкой CDN приводит к краткому периоду простоя для домена в течение регистрации домена на портале Azure. Запись сопоставления CNAME должна быть в следующем формате: 
 
  | ИМЯ             | ТИП  | Значение                  |
  |------------------|-------|------------------------|
  | `www.contoso.com` | `CNAME` | `contoso.azureedge.net` |


- Вариант 2. Сопоставление личного домена с конечной точкой CDN с помощью поддомена **cdnverify** Если рабочий трафик, который не может быть прерван, проходит в личном домене, вы можете создать временное сопоставление CNAME с конечной точкой CDN. Этот вариант позволяет использовать поддомен Azure **cdnverify**, чтобы обеспечить промежуточный этап регистрации. Так пользователи могут получать доступ к вашему домену без прерывания во время сопоставления DNS.

   1. Создайте запись CNAME и укажите псевдоним поддомена, который содержит поддомен **cdnverify**. Например, `cdnverify.www` или `cdnverify.cdn`. 
   2. Укажите имя узла, который является конечной точкой CDN, в следующем формате: `cdnverify.<EndpointName>.azureedge.net`. Запись сопоставления CNAME должна быть в следующем формате: 

   | ИМЯ                       | ТИП  | Значение                            |
   |----------------------------|-------|----------------------------------|
   | `cdnverify.www.contoso.com` | `CNAME` | `cdnverify.contoso.azureedge.net` | 


## <a name="step-3-enable-the-cname-record-mapping-in-azure"></a>Шаг 3. Включение сопоставления записи CNAME в Azure

После того как вы зарегистрировали личный домен, используя одну из предыдущих процедур, вы можете включить функцию личного домена в Azure CDN. 

1. Войдите на портал [Azure](https://portal.azure.com/) и перейдите к профилю CDN с конечной точкой, которую вы хотите сопоставить с личным доменом.  
2. В колонке **Профиль CDN** выберите конечную точку CDN, с которой требуется связать поддомен.
3. В левом верхнем углу колонки конечной точки щелкните **Пользовательский домен**. 

   ![Кнопка личного домена](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

4. В текстовом поле **Пользовательское имя узла** введите свой личный домен, включая поддомен. Например, `www.contoso.com` или `cdn.contoso.com`.

   ![Диалоговое окно добавления личного домена](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain-dialog.png)

5. Щелкните **Добавить**.

   Azure проверит наличие записи CNAME для введенного имени домена. Если запись CNAME правильна, пользовательский домен проходит проверку. Иногда может потребоваться некоторое время для распространения записи CNAME по серверам имен. Если домен не проверяется сразу, проверьте, что запись CNAME является правильной, подождите несколько минут и повторите попытку. Для конечных точек **Azure CDN от Verizon** (уровня "Премиум" и "Стандартный") на применение изменений в настройках личного домена к граничным узлам CDN может потребоваться до 90 минут.  


## <a name="step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Шаг 4. Проверка того, что личный поддомен ссылается на конечную точку CDN

После завершения регистрации личного домена убедитесь, что личный поддомен ссылается на конечную точку CDN.
 
1. Убедитесь, что общедоступное содержимое кэшируется в конечной точке. Например, если конечная точка CDN сопоставлена с учетной записью хранения, CDN кэширует содержимое в открытых контейнерах BLOB-объектов. Чтобы проверить личный домен, убедитесь, что контейнер допускает открытый доступ и содержит не менее одного большого двоичного объекта.

2. В браузере перейдите по адресу большого двоичного объекта с помощью личного домена. Например, если ваш личный домен — `cdn.contoso.com`, URL-адрес кэшированного большого двоичного объекта должен быть аналогичен следующему URL-адресу: `http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg`.


## <a name="step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint"></a>Шаг 5. Сопоставление постоянного личного домена с конечной точкой CDN (зависимый)

Этот шаг зависит от шага 2 (варианта 2) — сопоставления личного домена с конечной точкой CDN с помощью поддомена **cdnverify** Если вы используете временный поддомен **cdnverify** и проверили, что он работает, вы можете сопоставить свой постоянный личный домен с конечной точкой CDN.

1. На веб-сайте вашего поставщика домена создайте запись DNS CNAME для сопоставления постоянного личного домена с конечной точкой CDN. Запись сопоставления CNAME должна быть в следующем формате: 
 
   | ИМЯ             | ТИП  | Значение                  |
   |------------------|-------|------------------------|
   | `www.contoso.com` | `CNAME` | `contoso.azureedge.net` |
2. Удалите запись CNAME поддомена **cdnverify**, созданную ранее.

## <a name="see-also"></a>См. также
[Начало работы с Azure CDN](cdn-create-new-endpoint.md)  
[Делегирование зон DNS с помощью Azure DNS](../dns/dns-domain-delegation.md)
