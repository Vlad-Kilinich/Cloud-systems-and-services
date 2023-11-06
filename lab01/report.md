![image](https://github.com/Vladkilinichh/Cloud-systems-and-services/assets/63118851/1a7e3132-657a-4d40-82e0-eda6c9805683)# Отчёт по лабораторной работе №1  
---  
### Команда  
- Килинич Владислав К34212  
- Гладушко Ольга К34202  
---  
### Задание: Написать два Dockerfile – плохой и хороший. Плохой должен запускаться и работать корректно, но в нём должно быть не менее 3 “bad practices”. В хорошем Dockerfile они должны быть исправлены. В Readme описать все плохие практики из кода Dockerfile и почему они плохие, как они были исправлены в хорошем Dockerfile, а также две плохие практики по использованию этого контейнера.  
---  
### Ход работы  
Плохой Dockerfile  
```
FROM ubuntu:latest  

RUN apt-get -y update  
RUN apt-get install -y apache2  

RUN echo "<p align="center"> Hello, Packet! <br> <img src='https://raw.githubusercontent.com/OlgaGladushko/pictures/main/packet.webp'> </p>" > /var/www/html/index.html   
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]  

EXPOSE 80  
```
### Первая ошибка  
Образ на основе Ubuntu содержит дополнительное ПО, которое не требуется для нашего случая.  
Кроме того тег latest, может вызвать ошибки при сборке (когда выйдет другая версия). Всегда стоит указывать конкретный образ.  
```  
FROM ubuntu:latest  
```  

### Вторая ошибка  
Многослойность. добавляются дополнительные файлы и пакеты в образ, что ведет к увеличению размера конечного результата  
```  
RUN apt-get -y update  
RUN apt-get install -y apache2  
```  

### Третья ошибка   
Установка лишних пакетов. Каждый установленный пакет в Docker-образе влияет на размер образа, добавляя дополнительные файлы и зависимости.  
```
RUN apt-get -y update
```  
---
### Исправленный Dockerfile
```  
FROM alpine:3.14 

RUN apk add apache2 && \ 
    echo "<p align="center"> Hello, Packet! <br> <img src='https://raw.githubusercontent.com/OlgaGladushko/pictures/main/packet.webp'> </p>" > /var/www/localhost/htdocs/index.html 

CMD ["httpd", "-D", "FOREGROUND"]

EXPOSE 80
```
- Была указана конкретная версия минималистичного дистрибутива Linux (alpine) 
- Исправлена команда RUN  
- Лишние пакеты не устанавливаются 
Результат: 
<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab01/images/1.jpg?raw=true"/>  
</p>
Также при использовании docker run лучше озоглавливать контейнер  (docker run --name ...)  
<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab01/images/2.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

### Результат запуска контейнера:   

<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab01/images/3.jpg?raw=true" width="800" heidth = '700'/>  
</p>
---
# Вывод
В результате выполнения лабораторной работы было выполнено развертывание виртуальной машины на платформе Yandex Cloud. Также установлен CHR в VirtualBox и настроен VPN туннель между VPN server и CHR.
