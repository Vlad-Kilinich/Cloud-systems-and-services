# Отчёт по Лабораторной работе №4  

University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Year: 2023/2024  
Authors:   
Kilinich Vladislav К34212  
Gladushko Olga K34202

---  

### Задание
Сделать мониторинг сервиса, поднятого в кубере (использовать, например, prometheus и grafana).

### Ход работы  

Мы начнем с добавления репозитория в нашу конфигурацию helm, которую установили заранее ```choco install kubernetes-helm```:  
```  
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np  
```  

Таким образом, теперь мы можем легко получить доступ к веб-интерфейсу Prometheus  
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/1.jpg?raw=true" width="600" heidth = '500'>  
</p>  
И получаем следующих результат  

<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/5.jpg?raw=true">  
</p>  

---  

### Настройка yml  

### Проверка работоспособности

---  
# Вывод
В ходе лабораторной работы был создан main.yml с помощью которого мы смогли создать docker-образ и сохранить его в DockerHub после push

