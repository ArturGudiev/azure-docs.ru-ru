---
title: "Загрузка данных в службу \"Хранилище данных SQL Azure\" с помощью службы \"Фабрика данных Azure\" | Документация Майкрософт"
description: "Копирование данных в Azure Data Lake Store в хранилище данных SQL Azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: eec6eeb3419c5f5f4c8d22398051f7cf057ac980
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Загрузка данных в службу "Хранилище данных SQL Azure" с помощью службы "Фабрика данных Azure"

[Хранилище данных SQL Azure](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) — это развернутая в облаке база данных, способная обрабатывать большие объемы реляционных и нереляционных данных. Хранилище данных SQL создано на основе реализованной архитектуры вычислений с массовым параллелизмом (MPP), оптимизированной для рабочих нагрузок хранилища корпоративных данных. Оно предоставляет эластичность облака и гибкие возможности масштабирования хранилища и вычислительной мощности независимо друг от друга.

Приступить к работе с хранилищем данных SQL Azure теперь проще, чем когда-либо, с помощью фабрики данных Azure. Фабрика данных Azure — это полностью управляемая облачная служба интеграции данных. С ее помощью можно заполнять хранилище данных SQL данными из имеющейся системы и сократить время создания решений аналитики.

Служба "Фабрика данных Azure" предоставляет следующие преимущества загрузки данных в хранилище данных SQL Azure.

* **Простота настройки.** Вам доступен интуитивно понятный 5-этапный мастер без необходимости создавать сценарии.
* **Расширенная поддержка хранилищ данных.** Встроенная поддержка обширного набора локальных и облачных хранилищ данных. Подробный список см. в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).
* **Безопасность и совместимость.** Данные передаются по протоколу HTTPS или ExpressRoute. Наличие глобальной службы гарантирует, что ваши данные никогда не покинут заданных географических границ.
* **Беспрецедентная производительность благодаря PolyBase.** Применение Polybase — это наиболее эффективный способ перемещения данных в хранилище данных SQL Azure. Промежуточные большие двоичные объекты позволяют обеспечить высокую скорость загрузки данных из источников данных всех типов, в том числе хранилища BLOB-объектов Azure и Data Lake Store. (В Polybase эти хранилища поддерживаются по умолчанию.) Дополнительные сведения см. в руководстве [по настройке производительности действия копирования](copy-activity-performance.md).

В этой статье показано, как с помощью средства копирования данных службы "Фабрика данных" _загружать данные из базы данных SQL Azure в хранилище данных SQL Azure_. Чтобы копировать данные из других типов хранилищ, необходимо выполнить аналогичные шаги.

> [!NOTE]
> Дополнительные сведения см. в статье [Копирование данных в хранилище данных Azure SQL и из него с помощью фабрики данных Azure](connector-azure-sql-data-warehouse.md).
>
> Эта статья относится к версии 2 фабрики данных Azure, которая в настоящее время доступна в предварительной версии. Если вы используете общедоступную версию службы "Фабрика данных" (версия 1), см. статью [Перемещение данных с помощью действия копирования](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>предварительным требованиям

* Подписка Azure. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.
* Хранилище данных SQL Azure. В этом хранилище данных содержатся данные, копируемые из базы данных SQL. Если у вас нет хранилища данных SQL Azure, см. руководство [Загрузка данных из хранилища BLOB-объектов Azure в хранилище данных SQL Azure с помощью PolyBase](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* База данных SQL Azure. В этом руководстве демонстрационные данные Adventure Works LT копируются из базы данных SQL Azure. Вы можете создать базу данных SQL, следуя указаниям в статье [Создание базы данных SQL Azure на портале Azure](../sql-database/sql-database-get-started-portal.md). 
* Учетная запись хранения Azure. Учетная запись хранения Azure используется в качестве _промежуточного_ хранилища больших двоичных объектов в операции массового копирования. Если у вас нет учетной записи хранения Azure, см. инструкции по [ее созданию](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-a-data-factory"></a>Создать фабрику данных

1. В меню слева выберите **Создать** > **Data + Analytics** (Данные и аналитика) > **Фабрика данных**: 
   
   ![Создание фабрики данных](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. В полях на странице **Новая фабрика данных** задайте значения, как показано на следующем изображении:
      
   ![Страница "Новая фабрика данных"](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
    * **Имя.** Введите глобальное уникальное имя фабрики данных Azure. Если отобразится сообщение об ошибке "Имя \"LoadSQLDWDemo\" фабрики данных недоступно", введите другое имя. Например, вы можете использовать имя _**ваше_имя**_**ADFTutorialDataFactory**. Попробуйте создать фабрику данных еще раз. Правила именования артефактов службы "Фабрика данных" см. в [этой](naming-rules.md) статье.
    * **Подписка.** Выберите подписку Azure, в рамках которой нужно создать фабрику данных. 
    * **Группа ресурсов.** Выберите имеющуюся группу ресурсов из раскрывающегося списка или щелкните параметр **Create new** (Создать) и введите имя группы ресурсов. Сведения о группах ресурсов см. в статье, где описывается [использование групп ресурсов для управления ресурсами Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Версия.** Выберите **V2 (предварительная версия)**.
    * **Расположение.** Выберите расположение фабрики данных. В раскрывающемся списке отображаются только поддерживаемые расположения. Хранилища данных, используемые в фабрике данных, могут находиться в других расположениях и регионах. К этим хранилищам относятся Azure Data Lake Store, служба хранилища Azure, база данных SQL Azure и т. д.

3. Нажмите кнопку **Создать**.
4. После создания перейдите к фабрике данных. Вы увидите домашнюю страницу **фабрики данных**, как показано на следующем изображении:
   
   ![Домашняя страница фабрики данных](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Выберите плитку **Создание и мониторинг**, чтобы открыть на отдельной вкладке приложение интеграции данных.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Загрузка данных в хранилище данных Azure SQL

1. Чтобы запустить средство копирования данных, на странице **Get started** (Начало работы) выберите плитку **Copy Data** (Копирование данных):

   ![Плитка средства копирования данных](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. На странице **Properties** (Свойства) укажите **CopyFromSQLToSQLDW** в поле **Task name** (Имя задачи) и нажмите кнопку **Далее**.

    ![Страница свойств](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. На странице **Исходное хранилище данных** выберите **База данных SQL Azure** и нажмите кнопку **Далее**.

    ![Страница исходного хранилища данных](./media/load-azure-sql-data-warehouse/specify-source.png)
4. На странице **Specify the Azure SQL database** (Выбор базы данных SQL Azure) выполните следующие действия. 
   1. Выберите нужный сервер Azure SQL в списке **Server name** (Имя сервера).
   2. Выберите базу данных Azure SQL в списке **Database name** (Имя базы данных).
   3. Укажите имя пользователя в поле **User name** (Имя пользователя).
   4. Укажите **пароль** пользователя.
   5. Щелкните **Далее**.
   
   ![Выбор базы данных SQL Azure](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. На странице **Select tables from which to copy the data or use a custom query** (Выберите таблицы, из которых нужно скопировать данные, или введите пользовательский запрос) введите **SalesLT**, чтобы отфильтровать таблицы. Установите флажок **Select all** (Выделить все), чтобы скопировать все таблицы, а затем нажмите кнопку **Далее**. 

    ![Выбор исходных таблиц](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. На странице **целевого хранилища данных** выберите **Хранилище данных SQL Azure** и нажмите кнопку **Далее**:

    ![Страница целевого хранилища данных](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. На странице **Specify the Azure SQL Data Warehouse** (Выбор хранилища данных SQL Azure) выполните следующие действия. 

   1. Выберите нужный сервер Azure SQL в списке **Server name** (Имя сервера).
   2. Выберите хранилище данных SQL Azure в поле **Имя базы данных**.
   3. Укажите имя пользователя в поле **User name** (Имя пользователя).
   4. Укажите **пароль** пользователя.
   5. Щелкните **Далее**.
   
   ![Выбор хранилища данных SQL Azure](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. Просмотрите содержимое страницы **Сопоставление таблицы** и нажмите кнопку **Далее**. Отобразится интеллектуальное сопоставление таблиц. Исходные таблицы сопоставляются с целевыми на основе их имен. Если исходная таблица не существует в месте назначения, по умолчанию фабрика данных Azure создает таблицу с таким же именем. Вы также можете сопоставить исходную таблицу с имеющейся целевой таблицей. 

   > [!NOTE]
   > Автоматическое создание таблицы для приемника хранилища данных SQL применяется, когда SQL Server или база данных SQL Azure является источником. При копировании данных из другого источника хранилища данных необходимо создать схему в приемнике хранилища данных SQL Azure, прежде чем начать копирование.

   ![Страница сопоставления таблиц](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. Просмотрите содержимое страницы **Schema mapping** (Сопоставление схемы) и нажмите кнопку **Далее**. Интеллектуальное сопоставление таблиц основано на имени столбца. Если в фабрике данных включена возможность автоматического создания таблиц, при наличии несовместимостей между исходным и целевым хранилищами выполняется преобразование типов данных. Если между исходным и целевым столбцами обнаружится преобразование неподдерживаемого типа данных, рядом с соответствующей таблицей появится сообщение об ошибке.

    ![Страница сопоставления схем](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. На странице **Settings** (Параметры) выберите службу хранилища Azure из раскрывающегося списка **Имя учетной записи хранения**. Эта учетная запись используется как промежуточное хранилище данных перед их загрузкой в хранилище данных SQL Azure с помощью PolyBase. После завершения копирования промежуточные данные в службе хранилища Azure удаляются автоматически. В разделе **Advanced settings** (Дополнительные настройки) снимите флажок **Use type default** (Использовать тип по умолчанию):

    ![Страница «Параметры»](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. Просмотрите параметры на странице **Summary** (Сводка), а затем нажмите кнопку **Далее**.

    ![Страница "Сводка"](./media/load-azure-sql-data-warehouse/summary-page.png)
13. На **странице развертывания** выберите **Monitor** (Мониторинг), чтобы отслеживать конвейер (задачу).

    ![Страница развертывания](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Обратите внимание, что слева автоматически выбирается вкладка **Мониторинг**. В столбце **Actions** (Действия) содержатся ссылки на сведения о выполнении действий или повторный запуск конвейера: 

    ![Мониторинг выполнений конвейера](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Щелкните ссылку **View Activity Runs** (Просмотр выполнений действий) в столбце **Actions** (Действия), чтобы просмотреть выполнения действий, связанные с этим запуском конвейера. В конвейере содержится 10 действий копирования, каждое из которых копирует одну таблицу с данными. Чтобы вернуться к представлению запусков конвейера, щелкните ссылку **Конвейеры** в верхней части окна. Щелкните **Обновить**, чтобы обновить список. 

    ![Мониторинг выполнений действий](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Чтобы отслеживать сведения о выполнении каждого действия копирования, щелкните ссылку **Сведения** в разделе **Actions** (Действия) в представлении мониторинга действия. Вы можете отслеживать такие сведения, как объем данных, копируемых из источника в приемник, пропускная способность данных, шаги выполнения с длительностью и используемые параметры:

    ![Мониторинг сведений о выполнении действия](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Дополнительная информация

Перейдите к следующей статье, чтобы узнать о поддержке хранилища данных SQL Azure: 

> [!div class="nextstepaction"]
>[Перемещение данных в хранилище данных Azure SQL и из него с помощью фабрики данных Azure](connector-azure-sql-data-warehouse.md).