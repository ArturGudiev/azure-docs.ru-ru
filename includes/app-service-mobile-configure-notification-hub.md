Компонент мобильных приложений в службе приложений Azure использует [Центры уведомлений Azure] для отправки push-уведомлений, поэтому вам нужно настроить центр уведомлений для мобильного приложения.

1. На [портале Azure] щелкните **Службы приложений**, а затем выберите серверную часть приложения. В разделе **Параметры** выберите **Push**.
2. Чтобы добавить в приложение ресурс концентратора уведомлений, нажмите кнопку **Подключить**. Вы можете создать центр или подключиться к существующему.

    ![Настройка концентратора](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Теперь центр уведомлений подключен к серверной части проекта вашего мобильного приложения. Позднее вы настроите этот концентратор уведомлений, чтобы подключиться к системе отправки уведомлений платформы (PNS), которая отправляет push-уведомления на устройства.

[портале Azure]: https://portal.azure.com/
[Центры уведомлений Azure]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
