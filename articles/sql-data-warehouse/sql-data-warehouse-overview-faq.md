---
title: "Часто задаваемые вопросы о хранилище данных SQL Azure | Документация Майкрософт"
description: "В этой статье собраны вопросы о хранилище данных SQL Azure, часто задаваемые пользователями и разработчиками."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 3/1/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 4c00710ecc0c91f8407eca81b78176075fcbd6ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a>Часто задаваемые вопросы о хранилище данных SQL

## <a name="general"></a>Общие сведения

В. Какие возможности защиты данных предлагаются в хранилище данных SQL?

О. Хранилище данных SQL предлагает несколько решений для защиты данных, таких как прозрачное шифрование данных (TDE) и аудит. Дополнительные сведения см. в статье [Безопасность].

В. Где можно выяснить, каким правовым нормам или бизнес-стандартам соответствует хранилище данных SQL?

О. На странице [соответствия требованиям корпорации Майкрософт] доступны различные сертификаты соответствия (такие как SOC и ISO), которые можно отфильтровать по продукту. Сначала выберите сертификат соответствия, а затем в правой части страницы в разделе "Microsoft in-scope cloud services" (Охваченные облачные службы корпорации Майкрософт) разверните "Azure", чтобы просмотреть службы Azure, которые соответствуют выбранным требованиям.

В. Можно ли подключить PowerBI?

О. Да! Служба PowerBI поддерживает прямые запросы, выполняемые из хранилища данных SQL, но она не предназначена для большого количества пользователей или данных в режиме реального времени. В рабочей среде PowerBI рекомендуется использовать службу PowerBI поверх служб Azure Analysis Services или Analysis Service IaaS. 

В. Какие ограничения емкости имеет хранилище данных SQL?

О. Текущие ограничения см. на странице [Ограничения емкости хранилища данных SQL]. 

В. Почему операция масштабирования, приостановки или возобновления занимает так много времени?

О. На длительность операций управления вычислениями может влиять ряд факторов. Как правило, продолжительность операций обусловлена откатом транзакций. При инициации операции масштабирования или приостановки блокируются все входящие сеансы, и запросы утрачиваются. Чтобы оставить систему в стабильном состоянии, перед началом операции необходимо выполнить откат транзакций. Чем больше число транзакций и размер их журналов, тем дольше будет задержка операции при восстановлении стабильного состояния системы.

## <a name="user-support"></a>Поддержка пользователей

В. Куда я могу отправить свой запрос на функцию?

О. Если у вас есть запрос на функцию, отправьте его на нашей странице [UserVoice].

В. Как я могу разрабатывать свои решения?

О. Для получения помощи при разработке с помощью хранилища данных SQL задайте свои вопросы на нашей странице [Stack Overflow]. 

В. Как отправить запрос в службу поддержки?

О. [Запросы в службу поддержки] можно отправить на портале Azure.

## <a name="sql-languagefeature-support"></a>Поддержка языка или функций SQL 

В. Какие типы данных поддерживает хранилище данных SQL?

О. Типы данных, поддерживаемые хранилищем данных SQL, перечислены [здесь].

В. Какие функции таблиц поддерживаются?

О. Хранилище данных SQL поддерживает множество функций, а те немногие функции, которые не поддерживаются, перечислены в разделе [Неподдерживаемые функции таблиц].

## <a name="tooling-and-administration"></a>Инструментарий и администрирование

В. Поддерживается ли создание проектов Базы данных в Visual Studio?

О. В настоящее время создание в Visual Studio проектов Базы данных для хранилища данных SQL не поддерживается. Если вы хотите проголосовать за то, чтобы поддержка проектов Базы данных была добавлена, то посетите нашу страницу UserVoice и [отправьте запрос на эту функцию].

В. Поддерживает ли хранилище данных SQL интерфейсы REST API?

О. Да. Большинство функции REST, которые могут использоваться в Базе данных SQL, также доступны в хранилище данных SQL. Сведения об API можно найти в документации REST или на сайте [MSDN].


## <a name="loading"></a>Загрузка

В. Какие драйверы клиента поддерживаются?

О. Сведения о поддержке драйверов для хранилища данных можно найти в статье [Драйверы для хранилища данных SQL Azure].

В. Какие форматы файлов поддерживает PolyBase при работе с хранилищем данных SQL?

О. Orc, RC, Parquet и неструктурированный текст с разделителями.

В. К чему можно подключиться из хранилища данных SQL, используя PolyBase? 

О. К [Azure Data Lake Store] и [большим двоичным объектам службы хранилища Azure].

В. Возможно ли вычисление со стековой памятью при подключении к большим двоичным объектам службы хранилища Azure или Azure Data Lake Store? 

О. Нет, PolyBase в хранилище данных SQL взаимодействует только с компонентами хранилища. 

В. Можно ли подключиться к HDI?

О. HDI может использовать Azure Data Lake Store или WASB в качестве уровня HDFS. Если одна из этих служб используется в качестве уровня HDFS, то можно загрузить данные в хранилище данных SQL. Тем не менее, создание вычислений со стековой памятью для экземпляра HDI невозможно. 

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения о хранилище данных SQL в целом см. на [этой странице].


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Драйверы для хранилища данных SQL Azure]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Запросы в службу поддержки]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Безопасность]: ./sql-data-warehouse-overview-manage-security.md
[соответствия требованиям корпорации Майкрософт]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[Ограничения емкости хранилища данных SQL]: ./sql-data-warehouse-service-capacity-limits.md
[здесь]: ./sql-data-warehouse-tables-data-types.md
[Неподдерживаемые функции таблиц]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[большим двоичным объектам службы хранилища Azure]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[отправьте запрос на эту функцию]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[этой странице]: ./sql-data-warehouse-overview-faq.md