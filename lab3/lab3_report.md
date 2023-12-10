University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)     
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)     
Year: 2023/2024     
Group: K33212      
Author: Lebedeva Tatyana Alexandrovna      
Lab: Lab2          
Date of create: 08.12.2023       
Date of finished:        

## Лабораторная работ №3 "Эмуляция распределенной корпоративной сети связи, настройка OSPF и MPLS, организация первого EoMPLS"    

## <a>Описание</a>   
Наша компания "RogaIKopita Games" с прошлой лабораторной работы выросла до серьезного игрового концерна, ещё немного и они выпустят свой ответ Genshin Impact - Allmoney Impact. И вот для этой задачи они купили небольшую, но очень старую студию "Old Games" из Нью Йорка, при поглощении выяснилось что у этой студии много наработок в области компьютерной графики и совет директоров "RogaIKopita Games" решил взять эти наработки на вооружение. К сожалению исходники лежат на сервере "SGI Prism", в Нью-Йоркском офисе никто им пользоваться не умеет, а из-за короновируса сотрудники офиса из Санкт-Петерубурга не могут добраться в Нью-Йорк, чтобы забрать данные из "SGI Prism". Ваша задача подключить Нью-Йоркский офис к общей IP/MPLS сети и организовать EoMPLS между "SGI Prism" и компьютером инженеров в Санк-Петербурге.

## <a>Цель работы</a>  
Изучить протоколы OSPF и MPLS, механизмы организации EoMPLS.в.

## <a>Правила по оформлению</a>  
Правила по оформлению отчета по лабораторной работе вы можете изучить по <a href="https://itmo-ict-faculty.github.io/introduction-in-routing/education/labs2023_2024/reportdesign/">ссылке</a>

## <a>Ход работы</a>   
#### <a>Основная часть лабораторной работы:</a>  
Необходимо сделать IP/MPLS сеть связи для "RogaIKopita Games" изображенную на рисунке 1 в ContainerLab. Необходимо создать все устройства указанные на схеме и соединения между ними.
#### <a>Построение сети связи</a>  
1. Содержимое yaml файла, который использовался для развертывания тестовой сети:
    ```
    name: lab3
    topology:
      nodes:
        R01_NYC:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9
        R01_LND:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9    
        R01_HKI:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9    
        R01_SPB:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9        
        R01_MSK:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9         
        R01_LBN:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9      
        PC1:
          kind: linux
          image: ubuntu:latest
          ports:
            - "8023:23"
        SGI_Prism:
          kind: linux
          image: ubuntu:latest
          ports:
            - "9023:23"
      links:
        - endpoints: ["SGI_Prism:eth1","R01_NYC:eth1"]
        - endpoints: ["R01_NYC:eth2","R01_LND:eth1"]
        - endpoints: ["R01_NYC:eth3","R01_LBN:eth1"]
        - endpoints: ["R01_LND:eth2","R01_HKI:eth1"]
        - endpoints: ["R01_LBN:eth2","R01_HKI:eth2"]
        - endpoints: ["R01_LBN:eth3","R01_MSK:eth1"]
        - endpoints: ["R01_HKI:eth3","R01_SPB:eth1"]
        - endpoints: ["R01_SPB:eth2","R01_MSK:eth2"]
        - endpoints: ["R01_SPB:eth3","PC1:eth1"]
    mgmt:
      network: statics
      ipv4-subnet: 192.168.1.0/24
    ```
2. Сборка контейнера производится с помощью команды:    
   ```sudo containerlab deploy lab3.clab.yaml```    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/403f72e8-66ce-4870-947a-4841949722fc)

4. Построение схемы сети производится с помощью команды:     
   ```sudo containerlab graph```
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/5bdc7279-a1b8-446e-9bb6-b2d8c480ef5b)
  
### <a>Текст конфигураций для каждого сетевого устройства</a>
#### <a>Настройка R01_SPB с компьютером инженеров в Санк-Петербурге:</a>    
##### <a>Настройка PC1:</a>    
Для того, чтобы войти в конфигурацию PC1, необходимо вввести команду:        
   ```telnet 192.168.1.3```   
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/6d3e4269-ae74-437b-baaf-d71d0d6b6e39)    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/e74e278e-9a60-4f7c-97e3-5e16030dbc3d)
##### <a>Настройка R01_SPB:</a>  
Для того, чтобы войти в конфигурацию R01_SPB, необходимо вввести команду:        
   ```telnet 192.168.1.9```    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/8674e65a-5110-4165-8608-127234332026)

## <a>Результаты пингов и проверка локальной связности:</a>     
#### <a>Проследим маршрут следования данных от R01_NYC до R01_SPB:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/646c5b19-f28c-4346-88ec-fbff93530d18)

#### <a>Конфигурация MPLS:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/3a296d51-66dc-479d-a38c-c022d3373df0)

#### <a>ICMP запрос от SGI_Prism к PC1:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/6bad02d3-ac5f-4b44-83e9-c561d6e19ce8)

## <a>Выводы</a>  
В результате выполнения данной лабораторной работы были изучены протоколы OSPF и MPLS, механизмы организации EoMPLS.в.
