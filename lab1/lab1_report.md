University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)     
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)     
Year: 2023/2024     
Group: K33212      
Author: Lebedeva Tatyana Alexandrovna      
Lab: Lab1       
Date of create: 21.10.2023       
Date of finished:        

## Лабораторная работ №1 "Установка ContainerLab и развертывание тестовой сети связи"   
## <a>Описание</a>   
В данной лабораторной работе вы познакомитесь с инструментом ContainerLab, развернете тестовую сеть связи, настроите оборудование на базе Linux и RouterOS.

## <a>Цель работы</a>   
Ознакомиться с инструментом ContainerLab и методами работы с ним, изучить работу VLAN, IP адресации и т.д.
## <a>Правила по оформлению</a> 
Правила по оформлению отчета по лабораторной работе вы можете изучить по <a href="https://itmo-ict-faculty.github.io/introduction-in-routing/education/labs2023_2024/reportdesign/">ссылке</a>
## <a>Ход работы</a>   
#### <a>Подготовка</a>   
1. Установка Docker на рабочий компьютер
2. Установка make и склонирование <a href="https://github.com/hellt/vrnetlab">hellt/vrnetlab</a>  
3. В проекте hellt/vrnetlab переход в папку routeros, загрузка в эту папку chr-6.47.9.vmdk и с помощью make docker-image собрание образа.
4. Установка ContainerLab используя специальный скрипт из официального репозитория       
   ```bash -c "$(curl -sL https://get.containerlab.dev)"```     
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/587d84f7-e231-428c-b02d-03a67676b791)

#### <a>Основная часть лабораторной работы</a>  
#### <a>Построение трехуровневой сети связи классического предприятия</a>  
1. Содержимое yaml файла, который использовался для развертывания тестовой сети:
    ```
    name: lab1
    topology:
      nodes:
        R01:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.50.2

        Linux1:
          kind: linux
          image: alpine:latest
          cmd: sleep infinity
          mgmt-ipv4: 192.168.50.3

        Linux2:
          kind: linux
          image: alpine:latest
          cmd: sleep infinity
          mgmt-ipv4: 192.168.50.4
      
        SW01.L3.01:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.50.5

        SW02.L3.01:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.50.6

        SW02.L3.02:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.50.7

      links:
          - endpoints: ["R01:eth1", "SW01.L3.01:eth1"]
          - endpoints: ["SW01.L3.01:eth2", "SW02.L3.01:eth1"]
          - endpoints: ["SW02.L3.01:eth2", "Linux1:eth1"]
          - endpoints: ["SW01.L3.01:eth3", "SW02.L3.02:eth1"]
          - endpoints: ["SW02.L3.02:eth2", "Linux2:eth1"]

    mgmt:
     network: static
     ipv4-subnet: 192.168.50.0/24
    ```
3. Сборка контейнера производится с помощью команды:    
   ```sudo containerlab deploy lab1.yaml```
5. Построение схемы сети производится с помощью команды:     
   ```sudo containerlab graph```

![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/315932fe-de76-4dcb-acb9-1694814c7125)

#### <a>Текст конфигураций для каждого сетевого устройства</a>
Для того, чтобы войти в конфигурацию роутера, необходимо вввести команду:
   ```sudo ssh abmin@clab-lab-R01```    
#### <a>Настройка роутера R01</a>:      
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/af072327-6cb8-4397-823d-74c87b497bdc)
Для того, чтобы войти в конфигурацию коммутатора, необходимо вввести команду:
   ```sudo ssh abmin@clab-lab-SW01.L3.01```    
#### <a>Настройка SW01.L3.01</a>:    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/f8479e1b-3d87-4b41-accb-39077b0d5c26)
Для того, чтобы войти в конфигурацию коммутатора, необходимо вввести команду:
   ```sudo ssh abmin@clab-lab-SW02.L3.01```    
#### <a>Настройка SW02.L3.01</a>:
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/dc04b734-c1c4-4e30-ae0c-1d16300284c3)
Для того, чтобы войти в конфигурацию коммутатора, необходимо вввести команду:
   ```sudo ssh abmin@clab-lab-SW02.L3.02```    
#### <a>Настройка SW02.L3.02</a>:
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/8068cf85-57c1-43cd-a72b-25f0b1f5428a)
#### <a>Проверка доступонсти. Результаты пингов</a>:
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/5bfff0ab-593f-4c55-a7f1-36d5f8c7a2c8)
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/ad57a3cd-34b6-44ee-889b-b11cfdb9ec30)

### <a>Вывод</a>:
В результате выполнения лабораторной работы было произведено ознакомление с инструментом ContainerLab и методами работы с ним, изучена работа VLAN, IP адресации и т.д.
