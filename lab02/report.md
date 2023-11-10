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
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab02/images/1.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

После установили minikube с помощью команд  

```  
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```  
  
```  
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
}
```  

### Создание yaml файлов  


```  
apiVersion: apps/v1
kind: Deployment

metadata:
  name: my-service

spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-service
  template:
    metadata:
      labels:
        app: my-service
    spec:
      containers:
        - image: vladkilinich/dockerfile:service
          imagePullPolicy: Always
          name: my-service
          ports:
            - containerPort: 80
```  


```
apiVersion: v1
kind: Service

metadata:
  name: my-service-lb

spec:
  type: LoadBalancer
  ports:
    - targetPort: 80 
      port: 80
      protocol: TCP
      
  selector:
    app: my-service
```




### Результат запуска контейнера:   
<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab01/images/3.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

---  
# Вывод
В ходе лабораторной работы были созданы два Dockerfile, с примерами хороших и плохих практик использования. Результат запуска контейнеров одинаков, однако есть ощутимая разница в размере контейнера 231МБ против 11.5 (разница в 20 раз)
