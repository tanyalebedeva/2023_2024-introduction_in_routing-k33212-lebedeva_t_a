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
## <a>Ход работы</a>   
#### <a>Подготовка</a>   
1. Установка Docker на рабочий компьютер
2. Установка make и склонирование hellt/vrnetlab
3. В проекте hellt/vrnetlab переход в папку routeros, загрузка в эту папку chr-6.47.9.vmdk и с помощью make docker-image собрание образа.
4. Установка ContainerLab используя специальный скрипт из официального репозитория
   ```bash -c "$(curl -sL https://get.containerlab.dev)"```
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

