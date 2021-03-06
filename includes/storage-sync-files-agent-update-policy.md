Агент службы синхронизации файлов Azure обновляется на регулярной основе. Это позволяет добавлять новые возможности и устранять ошибки. Мы рекомендуем настроить Центр обновления Майкрософт, чтобы получать обновления службы синхронизации файлов Azure по мере их выпуска. Мы понимаем, что некоторые организации могут строго контролировать и проверять обновления.

Для развертываний, использующих более ранние версии агента службы синхронизации файлов Azure:

- Служба синхронизации хранилища будет поддерживать предыдущую основную версию в течение трех месяцев после выпуска новой основной версии. Например, версия 1.\* будет поддерживаться службой синхронизации хранилища в течение трех месяцев после выпуска версии 2.\*.
- По истечении трех месяцев служба синхронизации хранилища заблокирует синхронизацию зарегистрированных серверов, использующих версию с истекшим сроком действия, с соответствующими группами синхронизации.
- В течение трех месяцев поддержки предыдущей основной версии все исправления ошибок отправляются в текущую (новую) основную версию.

> [!Note]  
> Если срок действия версии службы синхронизации файлов Azure, которую вы используете, истекает в течение трех месяцев, мы сообщим вам об этом с помощью всплывающего уведомления на портале Azure.
