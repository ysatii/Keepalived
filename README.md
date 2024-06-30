# Домашнее задание к занятию 1 «Disaster recovery и Keepalived»` - Мельник Юрий Александрович


## Задание 1

### `Уст `
## `Про `

1. `В `
 

### `Требования к результату`

1. `Прикрепите в файл README.md скриншот авторизации в админке.`
2. `Приложите в файл README.md текст использованных команд в GitHub.`

### Подготовим 3 машины на яндекс облаке, первую будем использовать в роли Zabbix Server, остальные будут клиентами, на них будет установлен агент 
![alt text](https://github.com/ysatii/gitlab-hw/blob/zabbix/img1/image1_1.jpg)

## Решение 1
1. `На машину с именем "zabbix-deb" установим PostgreSQL`
 ```
apt update 

apt install postgresql
 ```

![alt text](https://github.com/ysatii/gitlab-hw/blob/zabbix/img1/image1_5.jpg)

## Задание 2

 


1. `Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.`
2. `Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах`
3. `Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.`
4. `Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script`
5. `ПНа проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html.`

 



## Решение 2

1. `Создадим Две виртуальные машины и проверим с помощью команды ping видят ли они друг друга по сети`   
   Машина 1  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2.jpg)  
 Машина 2  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_1.jpg)

2. `Установим сервис на обе машины`
 ```
 sudo apt-get install keepalived
 ```  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_2.jpg)

3. `Произвденем настройку конфигурационных файлов`  
 сервер1 , мастер к виду   
  ```
  vrrp_instance VI_1 {
    	state MASTER
    	interface enp0s3
    	virtual_router_id 15
    	priority 255
    	advert_int 1

    	virtual_ipaddress {
          	10.0.2.100/24
    	}
 }
 
 ```  
  ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_3.jpg)  
 сервер 2, бэпап к виду   
 ```
 
 vrrp_instance VI_1 {
    	state BACKUP
    	interface enp0s3
    	virtual_router_id 15
    	priority 200
    	advert_int 1

    	virtual_ipaddress {
          	10.0.2.100/24
    	}

 }
 
 ```  
  ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_4.jpg)
 
4. `Произвдем запуск сервисов`  
 сервер 1, мастер  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_5.jpg)  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_6.jpg)  
 сервер 2, бэкап   
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_7.jpg)  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_8.jpg)  
 заметим что плаващий  10.0.2.100 только на сервер1, но с сервер 2 пинг проходит на 10.0.2.100 
 
5. `Произвденем проверку сервиса Keepalived`  
 остановим сервис на сервер1 и проверим адреса
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_9.jpg)  
 произведем пинг с сервер 1 на плавающий 10.0.2.100  
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_10.jpg)  
 сервис на второй машине перешол в состояние мастер
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_11.jpg)  
 запустим сервис снова на первой машине 
 ![alt text](https://github.com/ysatii/Keepalived/blob/main/img/image2_11.jpg)  
 сервер 1 Мастер восстановил свою работоспособность! 
