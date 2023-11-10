# Лабораторная работа №2 
---  
### Команда  
- Килинич Владислав К34212  
- Гладушко Ольга К34202  

### Задание
Локально поднять kubernetes кластер и развернуть свой сервис, используя 2 ресурса kubernetes.

### Ход работы  

Для начала загрузили docker image на Docker Hub  

<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab01/images/2.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

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
В ходе лабораторной работы были созданы два Dockerfile, с примерами хороших и плохих практик использования. Результат запуска контейнеров одинаков, однако есть ощутимая разница в размере контейнера 231МБ против 11.5 (разница в 20 раз)