> [!NOTE]
> * Общедоступный IP-адрес VPN-шлюза изменится при переходе с предыдущего номера SKU на новый.
> * Классические VPN-шлюзы нельзя перенести в новый SKU. Классические VPN-шлюзы могут использовать лишь предыдущие версии (старые) SKU.
> 

Размер VPN-шлюза Azure невозможно изменить путем перехода от старых SKU на новые семейства SKU. Если в модели развертывания Resource Manager есть VPN-шлюзы, использующие более старые версии номеров SKU, можно перейти на новые номера SKU. Для этого удалите существующий VPN-шлюз для виртуальной сети и создайте новый.

Рабочий процесс перехода:

1. Удалите все подключения к шлюзу виртуальной сети.
2. Удалите старый VPN-шлюз.
3. Создайте новый VPN-шлюз.
4. Обновите локальные VPN-устройства, используя новый IP-адрес VPN-шлюза (для подключений типа "сеть — сеть").
5. Обновите значение IP-адреса шлюза для всех шлюзов локальной сети типа "виртуальная сеть — виртуальная сеть", которые будут подключаться к этому шлюзу.
6. Скачайте новые пакеты конфигурации VPN-клиента для клиентов P2S, которые подключаются к виртуальной сети через этот VPN-шлюз.
7. Повторно создайте подключения к шлюзу виртуальной сети.