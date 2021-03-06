1. Войдите на [портале Azure](https://portal.azure.com).

2. Слева вверху последовательно выберите **Создать ресурс** > **Вычисления** > **Windows Server 2016 Datacenter**.

    ![Переход к образам виртуальных машин Azure на портале](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. В Windows Server 2016 Datacenter выберите классическую модель развертывания. Щелкните Создать.

    ![Снимок экрана с доступными на портале образами виртуальных машин Azure](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Колонка «Основные»

В колонке "Основные" указываются административные сведения о виртуальной машине.

1. Введите **имя** виртуальной машины. В этом примере _HeroVM_ — это имя виртуальной машины. Оно должно содержать от 1 до 15 знаков и не может включать специальные знаки.

2. Введите в соответствующих полях **имя пользователя** и надежный **пароль**, которые будут использоваться для создания локальной учетной записи на виртуальной машине. Локальная учетная запись используется для входа на виртуальную машину и управления ею. В этом примере _azureuser_ — это имя пользователя.

 Пароль должен содержать от 8 до 123 символов и включать по меньшей мере три из следующих символов: одна строчная буква, одна прописная буква, одна цифра и один специальный символ. См. дополнительные сведения о [требованиях к имени пользователя и паролю](../articles/virtual-machines/windows/faq.md).

3. **Подписку** указывать необязательно. Часто используется параметр "Оплата по мере использования".

4. Выберите существующую **группу ресурсов** или укажите имя, чтобы создать новую. В этом примере _HeroVMRG_ — это имя группы ресурсов.

5. Выберите **расположение** центра обработки данных Azure, в котором будет выполняться виртуальная машина. В этом примере в качестве расположения выбрана **Восточная часть США**.

6. Когда все поля заполнены, нажмите кнопку **Далее**, чтобы перейти к следующей колонке.

    ![Снимок экрана: параметры в колонке "Основные сведения" для настройки виртуальной машины Azure](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Колонка "Размер"

В колонке "Размер" определяется конфигурация виртуальной машины, а также выбираются различные параметры, такие как операционная система, количество процессоров, тип дискового накопителя и предполагаемые ежемесячные затраты на использование.  

Выберите размер виртуальной машины и нажмите кнопку **Выбрать**, чтобы продолжить. В этом примере _DS1_\__V2 Standard_ — это размер виртуальной машины.

  ![Снимок экрана: колонка выбора размера с доступными размерами виртуальной машины Azure](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Колонка "Настройки"

В колонке "Параметры" указываются параметры хранилища и сети. Вы можете принять настройки по умолчанию. Azure автоматически создаст необходимые записи.

Если вы выбрали размер виртуальной машины, который поддерживает хранилище Azure уровня "Премиум", то можете опробовать его, выбрав для параметра "Тип диска" значение "Премиум (SSD)".

После внесения изменений нажмите кнопку **ОК**.

## <a name="4-summary-blade"></a>4. Колонка "Сводка"

В колонке "Сводка" собраны параметры, указанные в предыдущих колонках. Когда будете готовы создать образ, нажмите кнопку **ОК**.

 ![Колонка "Сводка" с указанными настройками виртуальной машины](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

После создания виртуальная машина отображается на портале в разделе **Все ресурсы**. А также на панели мониторинга отображается элемент этой виртуальной машины. Также создаются и отображаются в списке соответствующие облачная служба и учетная запись хранения. Виртуальная машина и облачная служба запускаются автоматически, после чего для них отображается состояние **Работает**.

 ![Настройка агента и конечных точек виртуальной машины](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
