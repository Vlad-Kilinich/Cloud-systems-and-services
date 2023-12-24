University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Year: 2023/2024  
Authors:   
Kilinich Vladislav К34212  
Gladushko Olga K34202

---  

### Задание
Настроить подключение к сервису в миникубе через https. Используйте самоподписанный сертификат.

### Ход работы  

Для начала создадим сертификаты, которые будем использовать в дальнейшем:  
```  
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout private.key -out cert.crt -subj "/CN=lab2.lab2"
```
Также добавим новую запись в файл hosts:  
```
127.0.0.1       lab2.lab2
```
Создаем секрет packet-tls и в качестве сертификата и ключа укажем созданные ранее файлы:  
```
kubectl create secret tls packet-tls --cert cert.crt --key privateKey.key
```
### Найстрока yaml файла  
Создаем новый yaml файл, где указываем имя секрета packet-tls и имя хоста lab2.lab2  
```
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: ingress

spec:
  ingressClassName: nginx
  tls:
    - hosts:
        - lab2.lab2
      secretName: packet-tls
  rules:
    - host: lab2.lab2
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: packet-service
                port:
                  number: 80
```
После создания файла применяем  
```kubectl create -f ingress.yaml```  
и подключаем ingress  
``` minikube addons enable ingress ```  

### Проверка 
Для начала запустим команду ```minikube tunnel```, а затем ```minikube service my-service-lb```


Попадаем на сайт и видим проблему с сертификатом:  
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/assets/63118851/ab152352-283c-4541-b757-03f976601b6d" width="600" heidth = '500'>  
</p>  

Переходим на сайт и смотрим подробную информацию про сертификат:  
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab02⭐%EF%B8%8F/images/3.jpg?raw=true" width="600" heidth = '500'>  
</p>  
Браузер по умолчанию не доверяет не проверянным сертификатам, поэтому предупреждает нас об опасности.  
---  
# Вывод
В ходе лабораторной работы было настроено подключение к сервису в миникубе через https, с помощью самоподписанного сертификата.
