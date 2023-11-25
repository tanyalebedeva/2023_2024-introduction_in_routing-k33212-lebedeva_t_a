University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)     
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)     
Year: 2023/2024     
Group: K33212      
Author: Lebedeva Tatyana Alexandrovna      
Lab: Lab2          
Date of create: 23.11.2023       
Date of finished:        

## Лабораторная работ №2 "Эмуляция распределенной корпоративной сети связи, настройка статической маршрутизации между филиалами"    

## <a>Описание</a>   
В данной лабораторной работе вы первый раз познакомитесь с компанией "RogaIKopita Games" LLC которая занимается разработкой мобильных игр с офисами в Москве, Франкфурте и Берлине. Для обеспечения работы своих офисов "RogaIKopita Games" вам как сетевому инженеру необходимо установить 3 роутера, назначить на них IP адресацию и поднять статическую маршрутизацию. В результате работы сотрудник из Москвы должен иметь возможность обмениваться данными с сотрудником из Франкфурта или Берлина и наоборот.

## <a>Цель работы</a>  
Ознакомиться с принципами планирования IP адресов, настройке статической маршрутизации и сетевыми функциями устройств.

## <a>Правила по оформлению</a>  
Правила по оформлению отчета по лабораторной работе вы можете изучить по <a href="https://itmo-ict-faculty.github.io/introduction-in-routing/education/labs2023_2024/reportdesign/">ссылке</a>

## <a>Ход работы</a>   
#### <a>Основная часть лабораторной работы:</a>  
#### <a>Построение сети связи</a>  
1. Содержимое yaml файла, который использовался для развертывания тестовой сети:
    ```
    name: lab2
    topology:
      nodes:
        R01.MSK:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 172.30.20.13
        R01.BRL:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 172.30.20.14
        R01.FRT:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 172.30.20.15
        PC1:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 172.30.20.16
        PC2:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 172.30.20.17
        PC3:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 172.30.20.18
    links:
        - endpoints: ["R01.MSK:eth2", "R01.FRT:eth2"]
        - endpoints: ["R01.MSK:eth1", "R01.BRL:eth1"]
        - endpoints: ["R01.BRL:eth2", "R01.FRT:eth1"]
        - endpoints: ["R01.MSK:eth3", "PC1:eth3"]
        - endpoints: ["R01.FRT:eth3", "PC2:eth3"]
        - endpoints: ["R01.BRL:eth3", "PC3:eth3"]
    mgmt:
      network: statics
      ipv4-subnet: 172.30.20.0/24

    ```
2. Сборка контейнера производится с помощью команды:    
   ```sudo containerlab deploy lab1.yaml```
3. Построение схемы сети производится с помощью команды:     
   ```sudo containerlab graph```
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/a09baa95-7a21-4459-9639-1652ea5a3313)

#### <a>Настройка роутера R01.MSK:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/d30eceae-6955-4d01-98a8-9384f7d74a3c)

#### <a>Настройка роутера R01.FRT:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/843cf854-52dd-4a56-8b40-cb45689792c0)

#### <a>Настройка роутера R01.BRL:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/8e72ca0d-e717-41b9-b117-f7a1241a3756)

#### <a>Настройка PC1:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/5519db1a-a818-4191-bf98-b1e304210e84)

#### <a>Настройка PC2:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/4ab3184c-9cfc-4e37-9662-9a232480c595)

#### <a>Настройка PC3:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/9741eae4-54e1-44f5-b9db-1404697430ca)

#### <a>Проверка доступности:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/3de7be3f-cc01-40a9-bd38-95f020d34d25)
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/55b8f881-4171-4dc9-b6a9-776c51cd62e4)

### <a>Выводы:</a>   
