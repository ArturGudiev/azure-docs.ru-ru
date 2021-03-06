---
title: "Мониторинг и устранение неполадок приложения облачного хранилища в Azure | Документация Майкрософт"
description: "Мониторинг и устранение неполадок облачного приложения с помощью инструментов диагностики, метрик и оповещений."
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: a1b3a1d4bb397e19f033b8f3bfe68ca6a63725c4
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/22/2018
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>Мониторинг и устранение неполадок приложения облачного хранилища

Это руководство является четвертой и последней частью цикла. В этом руководстве показано, как выполнять мониторинг и устранение неполадок приложения облачного хранилища.

В четвертой части цикла вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * включение ведения журнала и метрик;
> * включение оповещений для ошибок авторизации;
> * выполнение тестирования трафика с помощью неверных маркеров SAS;
> * скачивание и анализ журналов.

[Azure Storage Analytics](../common/storage-analytics.md) ведет журнал и предоставляет данные метрик для учетной записи хранения. По этим данным можно определить состояние работоспособности учетной записи хранения. Прежде чем вы сможете просмотреть сведения об учетной записи хранения, вам необходимо настроить сбор данных. Этот процесс предполагает включение ведения журнала, настройку метрик и включение предупреждений.

Ведение журнала и метрик для учетных записей хранения можно включить на вкладке **Диагностика** на портале Azure. Есть два типа метрик. **Сводная.** Позволяет собирать метрики исходящего и входящего трафика, доступности, задержки и процента успешных операций. Эти метрики вычисляются для служб BLOB-объектов, очередей, таблиц и файлов. **Для каждого API.** Позволяет собирать один набор метрик для каждой операции с хранилищем в API службы хранилища Azure. Ведение журналов хранилища позволяет записывать сведения об успешных и неудачных запросах в вашей учетной записи хранения. Из этих журналов можно получить подробные сведения об операциях чтения, записи удаления в таблицах Azure, очередях и больших двоичных объектах. Они также позволяют выявить причины невыполнения запросов, таких как превышение времени ожидания, ошибки регулирования и авторизации.

## <a name="log-in-to-the-azure-portal"></a>Войдите на портал Azure.

Войдите на [портал Azure](https://portal.azure.com)

## <a name="turn-on-logging-and-metrics"></a>Включение ведения журнала и метрик

В меню слева щелкните **Группы ресурсов**, а затем — **myResourceGroup** и выберите свою учетную запись хранения в списке ресурсов.

В разделе **Диагностика** установите для параметра **Состояние** значение **Вкл.** Убедитесь, что **Статистические метрики больших двоичных объектов**, **Большой двоичный объект на метрики API** и **Журналы больших двоичных объектов** включены.

Затем нажмите кнопку **Сохранить**.

![Область диагностики](media/storage-monitor-troubleshoot-storage-application/figure1.png)

## <a name="enable-alerts"></a>Включение оповещений

Оповещения предоставляют возможность отправить сообщение электронной почты администраторам или активировать веб-перехватчик, если метрика превысила пороговое значение. В этом примере включается оповещение для метрики `SASClientOtherError`.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Переход к учетной записи хранения на портале Azure

В меню слева щелкните **Группы ресурсов**, а затем — **myResourceGroup** и выберите свою учетную запись хранения в списке ресурсов.

В разделе **Мониторинг** выберите **Правила оповещений**.

В разделе **Добавление правила оповещения** выберите **+ Add alert** (+ Добавить оповещение) и введите необходимые сведения. Выберите `SASClientOtherError` из раскрывающегося списка **Метрика**.

![Область диагностики](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Имитация ошибки

Для имитации действительного оповещения можно попытаться запросить несуществующий большой двоичный объект из учетной записи хранения. Для этого замените `<incorrect-blob-name>` несуществующим значением. Запустите следующий пример кода несколько раз, чтобы имитировать неудачные запросы большого двоичного объекта.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

На следующем изображении представлен пример предупреждения, основанного на имитированном сбое из предыдущего примера.

 ![Пример оповещения](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Скачивание и просмотр журналов

Журналы хранилища хранят данные в наборе больших двоичных объектов в контейнере BLOB-объектов **$logs** в учетной записи хранения. Этот контейнер не отображается в списке всех контейнеров больших двоичных объектов в учетной записи, но его содержимое можно просмотреть, если обратиться к нему напрямую.

В этом сценарии [Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) используется для взаимодействия с учетной записью хранения Azure.

### <a name="download-microsoft-message-analyzer"></a>Скачивание Microsoft Message Analyzer

Скачайте [Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) и установите приложение.

Запустите приложение и выберите **Файл** > **Открыть** > **From Other File Sources** (Из других источников файлов).

В диалоговом окне **File Selector** (Селектор файлов) выберите **+ Add Azure Connection** (+Добавить подключение Azure). Введите **имя учетной записи хранения** и **ключ учетной записи**, а затем нажмите кнопку **ОК**.

![Microsoft Message Analyzer. Диалоговое окно добавления соединения службы хранилища Azure.](media/storage-monitor-troubleshoot-storage-application/figure3.png)

После подключения разверните контейнеры в хранилище в представлении в виде дерева, чтобы просмотреть большие двоичные объекты журналов. Выберите последний журнал и нажмите кнопку **ОК**.

![Microsoft Message Analyzer. Диалоговое окно добавления соединения службы хранилища Azure.](media/storage-monitor-troubleshoot-storage-application/figure4.png)

В диалоговом окне **Новый сеанс** щелкните **Запуск** для просмотра журналов.

Открыв журнал, можно просмотреть события хранилища. Как видно на следующем изображении, в учетной записи хранения была активирована ошибка `SASClientOtherError`. Дополнительные сведения о ведении журнала хранилища см. в [этой статье](../common/storage-analytics.md).

![Microsoft Message Analyzer. Просмотр событий.](media/storage-monitor-troubleshoot-storage-application/figure5.png)

[Обозреватель службы хранилища](https://azure.microsoft.com/features/storage-explorer/) — это еще один инструмент, который можно использовать для взаимодействия с учетными записями хранения, в том числе с контейнером **$logs** и журналами, которые в нем содержатся.

## <a name="next-steps"></a>Дополнительная информация

В четвертой и последней части цикла вы узнали, как выполнять мониторинг и устранять неполадки учетной записи хранения, а также как выполнять следующие задачи:

> [!div class="checklist"]
> * Включение ведения журнала и метрик
> * включение оповещений для ошибок авторизации;
> * выполнение тестирования трафика с помощью неверных маркеров SAS;
> * скачивание и анализ журналов.

Перейдите по ссылке, чтобы просмотреть готовые хранилища.

> [!div class="nextstepaction"]
> [Примеры Azure CLI для хранилища BLOB-объектов](storage-samples-blobs-cli.md)