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

Мы начнем с настройки prometheus с помощью helm, который установили заранее ```choco install kubernetes-helm```:  
```  
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np  
```  

Таким образом, теперь мы можем легко получить доступ к веб-интерфейсу Prometheus  
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/1.jpg?raw=true" width="600" heidth = '500'>  
</p>  
И получаем следующий результат  

<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/5.jpg?raw=true">  
</p>  

### Настройка Grafana

Как и prometheus с помиощью helm установим и настроим Grafana:  
```
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-np
```  
Декодируем пароль от Grafana для дальнейшей авторизации:  
```
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | certutil -decode
```
Результат запуска ```minikube service grafana-np```  
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/2.jpg?raw=true"  width="600" heidth = '500'>  
</p>  
Также открывается веб-интерфейс Grafana, куда вводим ранее декодирвоанный пароль и логин admin. Создаем соединение с prometheus  
И получаем следующий результат:  

<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/4.jpg?raw=true">  
</p>  

### Найстрока Дашборда  
Выбираем готовый дашборд Grafana c ID=13332 и теперь можем мониторить Kubernetes кластер.
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab04/images/3.jpg?raw=true">  
</p>  

---  
# Вывод
В ходе лабораторной работы были настроены сервисы Grafana и Prometheus. Также был произведен мониторинг сервиса Prometheus с помощью дашборда в Grafana
