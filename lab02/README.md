# Лабораторная работа №2 
---  
### Команда  
- Килинич Владислав К34212  
- Гладушко Ольга К34202  

### Задание
Локально поднять kubernetes кластер и развернуть свой сервис, используя 2 ресурса Kubernetes.

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
Проверим установку с помощью следующей команды: 


<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab02/images/6.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

### Создание yaml файлов  

В качестве первого ресурса мы выбрали Deployment. Он отвечает за управление созданием и масштабированием подов в кластере, обеспечивает безопасные обновления и возможность отката к предыдущим версиям приложений при необходимости. Deployment позволяет определить, сколько экземпляров приложения должно быть запущено и поддерживать это количество в кластере (replicas).  

Также указываем путь к контейнеру в Docker Hub:  
image: vladkilinich/dockerfile:service  

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
В качестве второго ресурса мы использовали LoadBalancer, он отвечает за обеспечение доступности и обнаружение подов внутри кластера, а также за предоставление точки доступа к этим подам изнутри или снаружи кластера. 

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

### Запуск minicube  

<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab02/images/2.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

Создаем файлы описания вида deployment и service командой
```
kubectl create -f deployment.yaml -f service.yaml
```
<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab02/images/4.jpg?raw=true" width="800" heidth = '700'/>  
</p>  

И Получаем следующий результат, перейдя по ссылки в предыдущем скрине
<p align="center">  
<img src="https://github.com/Vladkilinichh/Cloud-systems-and-services/blob/main/lab02/images/5.jpg?raw=true" width="600" heidth = '500'/>  
</p>  

---  
# Вывод
В ходе лабораторной работы были созданы два yaml файла для описания ресурсов Kubernetes, был поднят кластер minikube, с помощью которого создали сервер.



