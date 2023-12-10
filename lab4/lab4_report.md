University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)     
Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)     
Year: 2023/2024     
Group: K33212      
Author: Lebedeva Tatyana Alexandrovna      
Lab: Lab4          
Date of create: 10.12.2023       
Date of finished:    

## Лабораторная работ №3 "Эмуляция распределенной корпоративной сети связи, настройка iBGP, организация L3VPN, VPLS"    

## <a>Описание</a>   
Компания "RogaIKopita Games" выпустила игру "Allmoney Impact", нагрузка на арендные сервера возрасли и вам поставлена задача стать LIR и организовать свою AS чтобы перенести все сервера игры на свою инфраструктуру. После организации вашей AS коллеги из отдела DEVOPS попросили вас сделать L3VPN между 3 офисами для служебных нужд. (Рисунок 1) Данный L3VPN проработал пару недель и коллеги из отдела DEVOPS попросили вас сделать VPLS для служебных нужд.

## <a>Цель работы</a>  
Изучить протоколы BGP, MPLS и правила организации L3VPN и VPLS.

## <a>Правила по оформлению</a>  
Правила по оформлению отчета по лабораторной работе вы можете изучить по <a href="https://itmo-ict-faculty.github.io/introduction-in-routing/education/labs2023_2024/reportdesign/">ссылке</a>

## <a>Ход работы</a>   
#### <a>Основная часть лабораторной работы:</a>  
Необходимо сделать IP/MPLS сеть связи для "RogaIKopita Games" изображенную на рисунке 1 в ContainerLab. Необходимо создать все устройства указанные на схеме и соединения между ними.
#### <a>Построение сети связи</a>  
1. Содержимое yaml файла, который использовался для развертывания тестовой сети:
    ```
    name: lab4
    topology:
      nodes:
        R01_NY:
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
        R01_SVL:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9        
        R01_LBN:
          kind: vr-ros
          image: vrnetlab/vr-routeros:6.47.9   
        PC2:
          kind: linux
          image: ubuntu:latest
        PC1:
          kind: linux
          image: ubuntu:latest
        PC3:
          kind: linux
          image: ubuntu:latest
      links:
        - endpoints: ["PC2:eth1","R01_NY:eth1"]
        - endpoints: ["R01_NY:eth2","R01_LND:eth1"]
        - endpoints: ["R01_LND:eth2","R01_LBN:eth1"]
        - endpoints: ["R01_LND:eth3","R01_HKI:eth1"]
        - endpoints: ["R01_LBN:eth2","R01_HKI:eth2"]
        - endpoints: ["R01_HKI:eth3","R01_SPB:eth1"]
        - endpoints: ["R01_LBN:eth3","R01_SVL:eth1"]
        - endpoints: ["R01_SVL:eth2","PC3:eth1"]
        - endpoints: ["R01_SPB:eth3","PC1:eth1"]
    mgmt:
      network: statics
      ipv4-subnet: 192.168.1.0/24
    ```
2. Сборка контейнера производится с помощью команды:    
   ```sudo containerlab deploy lab4.clab.yaml```    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/bfab71ca-e7a2-4767-84fa-58e7a96ad7c8)

4. Построение схемы сети производится с помощью команды:     
   ```sudo containerlab graph```
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/0711c388-ec2a-435c-92cf-951780185db5)

### <a>Текст конфигураций для каждого сетевого устройства</a>
#### <a>Настройка R01_SPB с компьютером инженеров в Санк-Петербурге:</a>    
