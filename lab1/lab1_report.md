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
    
