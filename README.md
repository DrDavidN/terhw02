# «Основы Terraform. Yandex Cloud» - Дрибноход Давид

### Задание 1
5. Инициализируйте проект, выполните код. Исправьте намеренно допущенные синтаксические ошибки. Ищите внимательно, посимвольно. Ответьте, в чём заключается их суть.

Ответ: terraform apply выдал следующую ошибку

![image](https://github.com/DrDavidN/terhw02/assets/128225763/03786107-9a93-42d1-aa7e-c7c910143e56)

Причины ошибок:
* ```platform_id = "standart-v4"``` должно быть так ```platform_id = "standard-v1"``` слово standart указано с ошибкой, последняя буква должна быть d. Согласно документации https://cloud.yandex.ru/docs/compute/concepts/vm-platforms версий платформы всего 3, исправил на 1
* согласно документации https://cloud.yandex.ru/docs/compute/concepts/performance-levels минимальное количество ядер 2, поэтому исправил ```cores = 1``` на ```cores = 2```

Исправленный блок выглядит так

![image](https://github.com/DrDavidN/terhw02/assets/128225763/f9379897-c00b-4cd7-a1e3-2892bb2375f3)

Скрин созданной машины

![image](https://github.com/DrDavidN/terhw02/assets/128225763/e0da5df4-6739-4ed6-883c-ef7ba6dbb4a6)


6. Подключитесь к консоли ВМ через ssh и выполните команду ``` curl ifconfig.me```.
Примечание: К OS ubuntu "out of a box, те из коробки" необходимо подключаться под пользователем ubuntu: ```"ssh ubuntu@vm_ip_address"```. Предварительно убедитесь, что ваш ключ добавлен в ssh-агент: ```eval $(ssh-agent) && ssh-add``` Вы познакомитесь с тем как при создании ВМ создать своего пользователя в блоке metadata в следующей лекции.;

![image](https://github.com/DrDavidN/terhw02/assets/128225763/871ba05c-619f-41c1-90b2-5c251d71ee4a)


7. Ответьте, как в процессе обучения могут пригодиться параметры ```preemptible = true``` и ```core_fraction=5``` в параметрах ВМ.

Параметр ```preemptible = true``` отвечает за прерываемость машины, машина будет остановлена спустя 24ч после запуска или при нехватки ресурсов для запуска другой машины. Параметр поможет сберечь средства если забыли удалить ВМ после тестов.

Параметр ```core_fraction=5``` устанавливает использование процессороного времени в 5%, это экономит ресурсы.

### Задание 2

1. Изучите файлы проекта.
2. Замените все хардкод-**значения** для ресурсов **yandex_compute_image** и **yandex_compute_instance** на **отдельные** переменные. К названиям переменных ВМ добавьте в начало префикс **vm_web_** .  Пример: **vm_web_name**.
2. Объявите нужные переменные в файле variables.tf, обязательно указывайте тип переменной. Заполните их **default** прежними значениями из main.tf. 

Ответ:

![image](https://github.com/DrDavidN/terhw02/assets/128225763/e982b01a-2343-4312-8829-d2f29a62d6a0)

![image](https://github.com/DrDavidN/terhw02/assets/128225763/ff925102-4568-4933-b5b5-972551c0bf15)


3. Проверьте terraform plan. Изменений быть не должно. 

![image](https://github.com/DrDavidN/terhw02/assets/128225763/4e607e66-b3a8-45f0-9078-56d1afed1d3c)


### Задание 3

1. Создайте в корне проекта файл 'vms_platform.tf' . Перенесите в него все переменные первой ВМ.

![image](https://github.com/DrDavidN/terhw02/assets/128225763/1cdb81c3-75b5-4a14-bc69-4396ca54c072)

![image](https://github.com/DrDavidN/terhw02/assets/128225763/1b54b956-0c4e-430f-933f-3bad11f3520b)


2. Скопируйте блок ресурса и создайте с его помощью вторую ВМ в файле main.tf: **"netology-develop-platform-db"** ,  ```cores  = 2, memory = 2, core_fraction = 20```. Объявите её переменные с префиксом **vm_db_** в том же файле ('vms_platform.tf').

![image](https://github.com/DrDavidN/terhw02/assets/128225763/95f3a2cb-a242-4fb9-a3e4-1b683cb29c40)

3. Примените изменения.

![image](https://github.com/DrDavidN/terhw02/assets/128225763/c1e3ed5e-a34f-4c1d-b7be-f9bf2f2a21b2)


### Задание 4

1. Объявите в файле outputs.tf **один** output типа **map**, содержащий { instance_name = external_ip } для каждой из ВМ.

![image](https://github.com/DrDavidN/terhw02/assets/128225763/de8a4a82-dafd-407f-aadc-9c855552b401)

2. Примените изменения.

![image](https://github.com/DrDavidN/terhw02/assets/128225763/a052b31d-2bdf-4c16-b9fe-66749a50020c)


### Задание 5

1. В файле locals.tf опишите в **одном** local-блоке имя каждой ВМ, используйте интерполяцию ${..} с несколькими переменными по примеру из лекции.
![image](https://github.com/DrDavidN/terhw02/assets/128225763/d414f50b-dc50-41e3-9b29-044b2d3d6100)

2. Замените переменные с именами ВМ из файла variables.tf на созданные вами local-переменные.
![image](https://github.com/DrDavidN/terhw02/assets/128225763/6b61df1c-fff5-4734-957e-f78813fa2dd9)

3. Примените изменения.
![image](https://github.com/DrDavidN/terhw02/assets/128225763/5652c846-1cde-4e91-adfb-533e2d64ed3a)


### Задание 6

1. Вместо использования трёх переменных  ".._cores",".._memory",".._core_fraction" в блоке  resources {...}, объедините их в единую map-переменную **vms_resources** и  внутри неё конфиги обеих ВМ в виде вложенного map.  
   ```
   пример из terraform.tfvars:
   vms_resources = {
     web={
       cores=
       memory=
       core_fraction=
       ...
     },
     db= {
       cores=
       memory=
       core_fraction=
       ...
     }
   ```
![image](https://github.com/DrDavidN/terhw02/assets/128225763/e1b30ecd-2db0-4053-bd89-fe260b6847d6)

![image](https://github.com/DrDavidN/terhw02/assets/128225763/a7ffc20c-bffa-42b5-886d-24f6dbfd59ab)

   
3. Создайте и используйте отдельную map переменную для блока metadata, она должна быть общая для всех ваших ВМ.
```
   пример из terraform.tfvars:
   metadata = {
     serial-port-enable = 1
     ssh-keys           = "ubuntu:ssh-ed25519 AAAAC..."
   }
``` 

![image](https://github.com/DrDavidN/terhw02/assets/128225763/fa0b6988-173a-4b85-9a97-f8455103c891)

![image](https://github.com/DrDavidN/terhw02/assets/128225763/4599b7c5-67e5-44a6-b841-c04355f5f532)
 
5. Найдите и закоментируйте все, более не используемые переменные проекта.

![image](https://github.com/DrDavidN/terhw02/assets/128225763/0c4778b0-142e-45e3-8acc-d6359167d1cd)

![image](https://github.com/DrDavidN/terhw02/assets/128225763/ed04919a-4a97-4d3f-8578-98d3db4157f0)

6. Проверьте terraform plan. Изменений быть не должно.

![image](https://github.com/DrDavidN/terhw02/assets/128225763/a0a94c30-b300-4b61-8267-6e4d54f9d033)
