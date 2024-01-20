University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)     
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)     
Year: 2023/2024     
Group: K33212      
Author: Lebedeva Tatyana Alexandrovna      
Lab: Lab2          
Date of create: 23.11.2023       
Date of finished: 28.11.2023       

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
          mgmt-ipv4: 192.168.60.2
        R01.BRL:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.60.3
        R01.FRT:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.60.4
        PC1:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.60.5
        PC2:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.60.6
        PC3:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
          mgmt-ipv4: 192.168.60.7
      links:
          - endpoints: ["R01.MSK:eth3", "R01.FRT:eth3"]
          - endpoints: ["R01.MSK:eth2", "R01.BRL:eth3"]
          - endpoints: ["R01.BRL:eth2", "R01.FRT:eth2"]
          - endpoints: ["R01.MSK:eth1", "PC1:eth1"]
          - endpoints: ["R01.FRT:eth1", "PC2:eth1"]
          - endpoints: ["R01.BRL:eth1", "PC3:eth1"]
    mgmt:
      network: statics
      ipv4-subnet: 192.168.60.0/24

    ```
2. Сборка контейнера производится с помощью команды:    
   ```sudo containerlab deploy lab2.yaml```
   ![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/62f96881-e6cd-4744-98be-eee34babf599)    

4. Построение схемы сети производится с помощью команды:     
   ```sudo containerlab graph```
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/b2b63a7d-45e4-485f-9476-d735318b3407)     


#### <a>Текст конфигураций для каждого сетевого устройства</a>
Для того, чтобы войти в конфигурацию московского роутера, необходимо вввести команду:        
   ```sudo ssh abmin@clab-lab-R01.MSK```
#### <a>Настройка роутера R01.MSK:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/c4cff310-9578-47eb-a3fc-cc367bb1d341)    
        
Для того, чтобы войти в конфигурацию франкфуртского роутера, необходимо вввести команду:      
   ```sudo ssh abmin@clab-lab-R01.FRT```
#### <a>Настройка роутера R01.FRT:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/2b6e243e-2619-4232-baf7-c0c2649de8a0)     
     
Для того, чтобы войти в конфигурацию берлинского роутера, необходимо вввести команду:      
   ```sudo ssh abmin@clab-lab-R01.BRL```
#### <a>Настройка роутера R01.BRL:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/13587f1f-40dc-4523-9420-69950f27ba4c)     
    
Для того, чтобы войти в конфигурацию PC1, необходимо вввести команду:      
   ```sudo ssh abmin@clab-lab-PC1```
#### <a>Настройка PC1:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/59da90e7-946e-4157-a052-9142826106ae)    
    
Для того, чтобы войти в конфигурацию PC2, необходимо вввести команду:      
   ```sudo ssh abmin@clab-lab-PC2```
#### <a>Настройка PC2:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/beebbaaf-6e33-4749-b610-dbd5a015c66d)    
         
Для того, чтобы войти в конфигурацию PC3 необходимо вввести команду:      
   ```sudo ssh abmin@clab-lab-PC3```
#### <a>Настройка PC3:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/f36d9764-f38a-4a0d-a5e9-fd6a3996a8a0)     


#### <a>Проверка доступности:</a>     
##### <a>MSK->FRT, MSK->BRL
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/51b39a07-b309-4ff4-aa82-c68a641ed2e8)    
##### <a>FRT->MSK, FRT->BRL
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/1344b1a4-ce8b-42a4-81b2-e4175053208f)    
##### <a>BRL->MSK, BRL->FRT
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/a2e1114b-0a3e-432a-b56f-fc5dde9bba7d)    

### <a>Выводы:</a>   
В результате выполнения лабораторной работы было выполнено ознакомление с с принципами планирования IP адресов, настройкой статической маршрутизации и сетевыми функциями устройств.
