---
title: "Создание графовой базы данных Azure Cosmos DB с помощью Java | Документация Майкрософт"
description: "В этой статье представлен пример кода Java, который можно использовать для подключения и выполнения запросов к данным графа в Azure Cosmos DB с помощью Gremlin."
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/08/2018
ms.author: lbosq
ms.openlocfilehash: b28300c4ed0a0c6f35bf49808b8ed12d4e180610
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2018
---
# <a name="azure-cosmos-db-create-a-graph-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: создание графовой базы данных с помощью Java и портала Azure

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. С помощью Azure Cosmos DB вы можете быстро создавать базы данных управляемых документов, таблиц и диаграмм и обращаться к ним. 

Выполнив это краткое руководство, вы создадите простую базу данных графа с помощью средств портала Azure для Azure Cosmos DB. В этом руководстве также объясняется, как быстро создать консольное приложение Java с помощью графовой базы данных и драйвера OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver). Указания в этом руководстве применимы к любой операционной системе, с которой может работать Java. Из этого краткого руководства вы узнаете, как создавать и изменять графы с помощью пользовательского интерфейса или программных средств. 

## <a name="prerequisites"></a>предварительным требованиям
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Кроме того, сделайте следующее:

* [Комплект разработчика Java (JDK 1.7+)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * В Ubuntu выполните команду `apt-get install default-jdk`, чтобы установить JDK.
    * Обязательно настройте переменную среды JAVA_HOME так, чтобы она указывала на папку, в которой установлен пакет JDK.
* [Скачайте](http://maven.apache.org/download.cgi) и [установите](http://maven.apache.org/install.html) двоичный архив [Maven](http://maven.apache.org/).
    * В Ubuntu выполните команду `apt-get install maven`, чтобы установить Maven.
* [Git.](https://www.git-scm.com/)
    * В Ubuntu выполните команду `sudo apt-get install git`, чтобы установить Git.

## <a name="create-a-database-account"></a>Создание учетной записи базы данных

Перед созданием графовой базы данных необходимо создать учетную запись графовой базы данных Gremlin с Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Добавление графа

Теперь вы можете использовать обозреватель данных на портале Azure для создания базы данных графов. 

1. Щелкните **Обозреватель данных** > **Новый граф**.

    Справа отобразится область **Добавление графа** (вам может потребоваться прокрутить вправо, чтобы увидеть ее).

    ![Страница добавления графа в обозревателе данных на портале Azure](./media/create-graph-java/azure-cosmosdb-data-explorer-graph.png)

2. На странице **Добавление графа** введите параметры для нового графа.

    Параметр|Рекомендуемое значение|ОПИСАНИЕ
    ---|---|---
    Идентификатор базы данных|sample-database|Введите имя новой базы данных, например *sample-database*. Имя базы данных может иметь длину от 1 до 255 символов и не может содержать `/ \ # ?` или пробел.
    Идентификатор графа|sample-graph|Введите имя новой коллекции, например *sample-graph*. Для имен графов предусмотрены те же требования к символам, что и для идентификаторов баз данных.
    Емкость хранилища|Фиксированный (10 ГБ)|Измените значение на **Фиксированный (10 ГБ)**. Это значение представляет емкость хранилища базы данных.
    Throughput|400 ЕЗ|Укажите для пропускной способности 400 единиц запросов в секунду. Чтобы сократить задержку, позже вы можете увеличить масштаб пропускной способности.

3. После заполнения формы нажмите кнопку **ОК**.

## <a name="clone-the-sample-application"></a>Клонирование примера приложения

Теперь перейдем к работе с кодом. Мы клонируем приложение API Graph из GitHub, зададим строку подключения и выполним ее. Вы узнаете, как можно упростить работу с данными программным способом.  

1. Откройте командную строку, создайте папку git-samples, а затем закройте окно командной строки.

    ```bash
    md "C:\git-samples"
    ```

2. Откройте окно терминала git, например git bash, и выполните команду `cd`, чтобы перейти в папку для установки примера приложения.  

    ```bash
    cd "C:\git-samples"
    ```

3. Выполните команду ниже, чтобы клонировать репозиторий с примером. Эта команда создает копию примера приложения на локальном компьютере. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>Просмотр кода

Этот шаг не является обязательным. Если вы хотите узнать, как создать в коде ресурсы базы данных, изучите приведенные ниже фрагменты кода. Они взяты из файла `Program.java`, который расположен в папке C:\git-samples\azure-cosmos-db-graph-java-getting-started\src\GetStarted. Если вас это не интересует, можете сразу переходить к разделу [Обновление строки подключения](#update-your-connection-information). 

* Инициализация Gremlin `Client` из конфигурации в файле `src/remote.yaml`.

    ```java
    cluster = Cluster.build(new File("src/remote.yaml")).create();
    ...
    client = cluster.connect();
    ```

* Выполнение шагов Gremlin с использованием метода `client.submit`.

    ```java
    ResultSet results = client.submit(gremlin);

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```

## <a name="update-your-connection-information"></a>Обновление информации о подключении

Теперь вернитесь на портал Azure, чтобы получить данные для подключения. Скопируйте эти данные в приложение. Эти настройки обеспечат обмен данными между вашим приложением и размещенной базой данных.

1. На [портале Azure](http://portal.azure.com/) щелкните **Ключи**. 

    Скопируйте первую часть значения URI.

    ![Просмотр и копирование ключа доступа на портале Azure, страница "Ключи"](./media/create-graph-java/keys.png)
2. Откройте файл src/remote.yaml и вставьте значение вместо `$name$` в `hosts: [$name$.graphs.azure.com]`.

    Теперь строка 1 в remote.yaml будет выглядеть примерно так: 

    `hosts: [test-graph.graphs.azure.com]`

3. В значении `endpoint` измените `graphs` на `gremlin.cosmosdb`. (Если учетная запись базы данных графа создана до 20 декабря 2017 г., оставьте значение конечной точки без изменений и перейдите к следующему шагу.)

    Значение конечной точки должно выглядеть так:

    `"endpoint": "https://testgraphacct.gremlin.cosmosdb.azure.com:443/"`

4. На портале Azure используйте кнопку "Копировать", чтобы скопировать первичный ключ и вставить его вместо `$masterKey$` в `password: $masterKey$`.

    Теперь строка 4 в remote.yaml будет выглядеть примерно так: 

    `password: 2Ggkr662ifxz2Mg==`

5. Измените строку 3 файла remote.yaml. Внесите вместо

    `username: /dbs/$database$/colls/$collection$`

    значение 

    `username: /dbs/sample-database/colls/sample-graph`

6. Сохраните файл remote.yaml.

## <a name="run-the-console-app"></a>Запуск консольного приложения

1. В окне терминала Git перейдите в папку azure-cosmos-db-graph-java-getting-started с помощью команды `cd`.

    ```git
    cd "C:\git-samples\azure-cosmos-db-graph-java-getting-started"
    ```

2. В окне терминала Git введите приведенную ниже команду, чтобы установить необходимые пакеты Java.

   ```
   mvn package
   ```

3. В окне терминала Git выполните следующую команду, чтобы запустить приложение Java.
    
    ```
    mvn exec:java -D exec.mainClass=GetStarted.Program
    ```

    В окне терминала появятся вершины, добавляемые в граф. 
    
    Если возникают ошибки времени ожидания, проверьте, правильно ли вы [указали сведения о подключении](#update-your-connection-information), и попробуйте еще раз выполнить последнюю команду. 
    
    Когда программа завершит работу, нажмите клавишу ВВОД и переключитесь на окно веб-браузера с порталом Azure. 

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a>Просмотр и добавление демонстрационных данных

Теперь можно вернуться в обозреватель данных, чтобы просмотреть вершины, добавленные в граф, и добавить дополнительные точки данных.

1. Щелкните **Обозреватель данных**, затем разверните **sample-graph**, щелкните **Граф** и нажмите кнопку **Применить фильтр**. 

   ![Создание документов в обозревателе данных на портале Azure](./media/create-graph-java/azure-cosmosdb-data-explorer-expanded.png)

2. В списке **результатов** обратите внимание на новых пользователей, добавленных в граф. Выберите пользователя **ben**. Вы увидите, что он связан с пользователем robin. Здесь можно перетаскивать вершины мышью, увеличивать или уменьшать масштаб колесиком мыши, а также увеличивать размер графа с помощью двойной стрелки. 

   ![Новые вершины в графе в обозревателе данных на портале Azure](./media/create-graph-java/azure-cosmosdb-graph-explorer-new.png)

3. Давайте добавим несколько новых пользователей. Нажмите кнопку **New Vertex** (Создать вершину), чтобы добавить данные в граф.

   ![Создание документов в обозревателе данных на портале Azure](./media/create-graph-java/azure-cosmosdb-data-explorer-new-vertex.png)

4. Введите метку *person*.

5. Щелкните **Добавить свойство**, чтобы поочередно добавить каждое из указанных ниже свойств. Обратите внимание, что вы можете создать уникальные свойства для каждого пользователя в графе. Требуется только ключ идентификатора.

    key|value|Заметки
    ----|----|----
    id|ashley|Уникальный идентификатор вершины. Если не указать идентификатор, он создастся автоматически.
    gender|Женский| 
    Технология | java | 

    > [!NOTE]
    > В этом кратком руководстве мы создадим несекционированную коллекцию. Но если вы создаете секционированную коллекцию, указав ключ раздела во время создания коллекции, включите ключ раздела в качестве ключа каждой новой вершины. 

6. Последовательно выберите **ОК**. Чтобы увидеть кнопку **ОК** в нижней части экрана, может потребоваться развернуть экран.

7. Еще раз нажмите кнопку **New Vertex** (Создать вершину), чтобы добавить нового пользователя. 

8. Введите метку *person*.

9. Щелкните **Добавить свойство**, чтобы поочередно добавить следующие свойства:

    key|value|Заметки
    ----|----|----
    id|rakesh|Уникальный идентификатор вершины. Если не указать идентификатор, он создастся автоматически.
    gender|Мужской| 
    Учебное заведение|MIT| 

10. Последовательно выберите **ОК**. 

11. Нажмите кнопку **Применить фильтр** со стандартным значением фильтра `g.V()`, чтобы отобразить все значения графа. Все пользователи теперь отображаются в списке **Результаты**. 

    По мере добавления новых данных используйте фильтры для ограничения результатов. По умолчанию обозреватель данных использует `g.V()` для получения всех вершин в графе. Вы можете задать для него другой [запрос графа](tutorial-query-graph.md), например `g.V().count()`, который возвращает число всех вершин графа в формате JSON. Если вы изменили фильтр, для возвращения к полному списку результатов снова установите фильтр `g.V()` и щелкните **Применить фильтр**.

12. Теперь можно будет соединить пользователей rakesh и ashley. Выберите в списке **Результаты** пользователя **ashley**, а затем нажмите кнопку редактирования рядом с разделом **Целевые объекты** в нижнем правом углу. Чтобы отобразить область **Свойства**, может потребоваться развернуть окно.

   ![Изменение целевого объекта вершины в графе](./media/create-graph-java/azure-cosmosdb-data-explorer-edit-target.png)

13. В поле **Целевой объект** введите *rakesh*, затем в поле **Граничная метка** введите слово *знает* и щелкните значок галочки.

   ![Создание связи между пользователями ashley и rakesh в обозревателе данных](./media/create-graph-java/azure-cosmosdb-data-explorer-set-target.png)

14. Теперь выберите пользователя **rakesh** в списке результатов. Вы увидите, что пользователи ashley и rakesh связаны. 

   ![Две вершины, связанные в обозревателе данных](./media/create-graph-java/azure-cosmosdb-graph-explorer.png)

   Ресурс для этого руководства создан. Вы можете дополнить этот граф новыми вершинами, изменить существующие вершины или изменить запросы. Теперь давайте изучим метрики, которые предоставляет Azure Cosmos DB, а затем очистим все ресурсы. 

## <a name="review-slas-in-the-azure-portal"></a>Просмотр соглашений об уровне обслуживания на портале Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы узнали, как создать учетную запись Azure Cosmos DB, граф с помощью обозревателя данных, а также как запустить приложение. Теперь вы можете создавать более сложные запросы и внедрять эффективную логику обхода графа с помощью Gremlin. 

> [!div class="nextstepaction"]
> [Как выполнять запросы к данным в базе данных Azure Cosmos DB с помощью API Graph (предварительная версия)](tutorial-query-graph.md)

