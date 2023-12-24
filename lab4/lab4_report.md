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
      ipv4-subnet: 172.20.20.0/24
    ```
2. Сборка контейнера производится с помощью команды:    
   ```sudo containerlab deploy lab4.clab.yaml```    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/ef503b52-2620-47e2-8611-289ee3a51c49)


4. Построение схемы сети производится с помощью команды:     
   ```sudo containerlab graph```
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/0711c388-ec2a-435c-92cf-951780185db5)

### <a>Текст конфигураций для каждого сетевого устройства</a>
#### <a>Настройка R01_NY:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/cb2b6d1d-af22-4dd0-b147-01dea6eff133)    
#### <a>Настройка R01_SBP:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/d501bd16-8d7d-4227-8e42-be0373571436)    
#### <a>Настройка R01_SVL:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/eb77e0d7-edad-468c-af46-7f7d16e5cff8)    
#### <a>Настройка R01_HKI:</a>     
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/c19086c1-9b81-466b-88e1-bfb8ef1c370d)    
#### <a>Настройка R01_LND:</a>     
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/d3553d59-dd34-4b94-89b9-c257d615b260)    
#### <a>Настройка R01_LBN:</a>   
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/7db25040-1ca7-45e5-9515-6968e6e638d5)    

### <a>Результаты пингов. Проверка связности VRF:</a> 

![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/ec6c75ec-f655-46e8-bc3a-d8f0995746ba)
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/1e0131f1-cf25-4207-b897-33160d4498eb)
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/1631fcca-217d-4bb6-b820-da5b22d31f9c)

### <a>Вторая часть</a> 
#### <a>Настройка PC1:</a>   
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/5635729c-eb59-411e-a23a-c5187eab4bf2)    
#### <a>Настройка PC2:</a>   
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/b8be430f-c392-48ed-929a-3b6e56c1cd47)    
#### <a>Настройка PC3:</a>   
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/aff74d13-c19b-4beb-9084-fcdf7aad90d5)    
#### <a>Настройка R01_SVL:</a>   
 ![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/c2726fcd-e0d7-4109-aa1f-fb846372b5bd)

#### <a>Настройка R01_NY:</a>   
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/e75e0d28-39ae-4b97-a728-f08d7d13775b)
    
#### <a>Настройка R01_SPB:</a>  
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/b75533f9-346b-45ef-8960-c2103f2f75f6)    

### <a>Результаты пингов. Проверка локальной связности:</a>    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/19a57c15-e556-41d1-95fe-d5c540fbccec)    
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/4545fc33-2b76-400d-b3ff-461057cd8990)     
![image](https://github.com/tanyalebedeva/2023_2024-introduction_in_routing-k33212-lebedeva_t_a/assets/90707032/4c05a472-197d-4d92-acf5-5769aa24ece0)     

## <a>Выводы</a>  
В результате выполнения данной лабораторной раюоты были изучены протоколы BGP, MPLS и правила организации L3VPN и VPLS.













