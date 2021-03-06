---
title: "Предоставление общего доступа к пользовательским представлениям службы \"Аналитика временных рядов Azure\" с помощью параметризованных URL-адресов | Документация Майкрософт"
description: "В этой статье объясняется, как создавать параметризованные URL-адреса в службе \"Аналитика временных рядов Azure\", чтобы с легкостью представлять общий доступ к пользовательскому представлению."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: MarkMcGeeAtAquent
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: rest-api
ms.topic: get-started-article
ms.workload: big-data
ms.date: 11/21/2017
ms.openlocfilehash: ffa8e96ab9a5344c924400fe55b4d1e6aee95f06
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Предоставление общего доступа к пользовательскому представлению с помощью параметризованного URL-адреса

Чтобы предоставить общий доступ к пользовательскому представлению в обозревателе службы "Аналитика временных рядов", можно создать параметризованный URL-адрес этого представления программными средствами.

В обозревателе службы "Аналитика временных рядов" поддерживаются параметры запроса URL-адреса, что позволяет указать представления в интерфейсе непосредственно из URL-адреса.  Например, используя только URL-адрес, можно указать целевую среду, предикат поиска и нужный период времени. Когда пользователь щелкает настраиваемый URL-адрес, в интерфейсе отображается ссылка непосредственно на конкретный ресурс на портале "Аналитика временных рядов".  Применяются политики доступа к данным. 

## <a name="environment-id"></a>Идентификатор среды

Параметр `environmentId=<guid>` указывает идентификатор целевой среды.  Это часть полного доменного имени для доступа к данным. Этот идентификатор можно найти в правом верхнем углу области сведений о среде на портале Azure.  Это все символы, которые предшествуют `env.timeseries.azure.com`. Пример параметра идентификатора среды: `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Время

С помощью параметризованного URL-адреса можно указать абсолютное или относительное значение времени.

### <a name="absolute-time-values"></a>Абсолютные значения времени

Для абсолютных значений времени используйте параметры `from=<integer>` и `to=<integer>`. 

`from=<integer>` — это значение начала времени на JavaScript (в миллисекундах) для периода поиска.

`to=<integer>` — это значение времени окончания на JavaScript (в миллисекундах) для периода поиска. 

Сведения о том, как указать миллисекунды для даты на JavaScript, см. на странице об [Epoch & Unix Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Относительные значения времени

Для относительных значений времени используйте `relativeMillis=<value>`, где *value* указывается в миллисекундах на JavaScript на основе последних данных в серверной части.

Например, `&relativeMillis=3600000` позволяет отобразить данные за последние 60 минут.

Допустимые значения соответствуют значениям в **быстром меню времени** в обозревателе службы "Аналитика временных рядов" и включают следующее:

- 1800000 (последние 30 мин.);
- 3600000 (последние 60 мин.);
- 10800000 (последние 3 часа);
- 21600000 (последние 6 часов);
- 43200000 (последние 12 часов);
- 86400000 (последние 24 часа);
- 604800000 (последние 7 дней);
- 2592000000 (последние 30 часов).

### <a name="optional-parameters"></a>Необязательные параметры

Параметр `timeSeriesDefinitions=<collection of term objects>` указывает условия представления службы "Аналитика временных рядов", которые содержат такие элементы:

- "name":"<string>"
  - имя *условия*;
- "splitBy":"<string>"
  - имя столбца, по которому выполняется *разбиение*;
- "measureName":"<string>"
  - имя столбца *меры*;
- "predicate":"<string>"
  - предложение *where* для фильтрации на стороне сервера.
-  "useSum":"true"
  - Это необязательный параметр, который определяет использование функции суммирования для меры.  Обратите внимание: если выбрана мера "События", число выбрано по умолчанию.  Обратите внимание: если мера "События" не выбрана, по умолчанию выбирается среднее значение.  

Параметр "multiChartStack=<true/false>" активирует наложение в диаграмме, а "multiChartSameScale=<true/false>" позволяет использовать одну и ту же шкалу оси Y для всех условий в пределах необязательного параметра.  

- "multiChartStack=false"
  - Значение "true" включено по умолчанию, так что нужно передать значение "false", чтобы активировать наложение.
- "multiChartStack=false&multiChartSameScale=true" 
  - Чтобы использовать одну и ту же шкалу оси Y для разных условий, должно быть включено наложение.  По умолчанию установлено значение "false", так что нужно передать значение "true", чтобы активировать эту функцию.  
  
"TimeBucketUnit=<Unit>&timeBucketSize=<integer>" дает возможность настроить ползунок интервала, чтобы обеспечить более детальное или более плавное объединенное представление диаграммы.  
- "timeBucketUnit=<Unit>&timeBucketSize=<integer>"
  - Единицы — дни, часы, минуты, секунды, миллисекунды.  Единицы всегда следует писать прописными буквами.
  - Определите число единиц, передав требуемое целое число для timeBucketSize.  Обратите внимание, что можно сократить представление до 7 дней.  
  
Параметр "timezoneOffset=<integer>" позволяет вам установить для часового пояса просматриваемой диаграммы смещение в формате UTC.  
  - "timezoneOffset=-<integer>"
    - Значение целого числа всегда указывается в миллисекундах.  
    - Следует отметить, что эта функциональность немного отличается от того, что включено в обозревателе TSI, где мы можем выбрать локальное (время браузера) или время в формате UTC.  
 
### <a name="examples"></a>Примеры

Например, чтобы добавить определения временных рядов в качестве параметра URL-адреса, можно использовать следующий пример:

```https
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Используйте эти примеры определений временных рядов для: 

- идентификатора среды;
- данных за последние 60 минут;
- условий (F1PressureID, F2TempStation и F3VibrationPL), представляющих необязательные параметры.
 
Так вы сможете создать следующий параметризованный URL-адрес для представления:

```https
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Если представление, описанное в URL-адресе выше, создано с помощью обозревателя службы "Аналитика временных рядов", оно будет выглядеть примерно так:

![Условия в обозревателе службы "Аналитика временных рядов"](media/parameterized-url/url1.png)

Полное представление (включая диаграмму) будет выглядеть следующим образом:

![Представление диаграммы](media/parameterized-url/url2.png)

## <a name="next-steps"></a>Дополнительная информация
[Запрос данных с помощью C#](time-series-insights-query-data-csharp.md)
