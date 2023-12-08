# Отчёт по Лабораторной работе №3
University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Year: 2023/2024  
Authors:   
Kilinich Vladislav К34212  
Gladushko Olga K34202
---  

### Задание
Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся.

### Ход работы  

Для выполнения данного задания решено было использовать ```GitHub Actions```. В первую очередь необходимо было настроить ```Actions secrets and variables```, куда мы поместим логин и токен от DockerHub (место куда будет загружатся образ).  

Создаем токен в DockerHub
<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab03/images/1.png?raw=true" width="600" heidth = '500'>  
</p>  

А после добавили данные в  Actions secrets:

<p align="center">  
<img src="https://github.com/Vlad-Kilinich/Cloud-systems-and-services/blob/main/lab03/images/2.jpg?raw=true" width="600" heidth = '500'>  
</p>  
---  

### Настройка yml  
Для того, чтобы собирался докер образ необходимо прописать yml файл в ```github/workflows```  
Для начала указываем условия для сбора образа: после push в ветке main

```
name: CI

on:
  push:
    branches: main
    paths: "lab03/**"
```
Авторизация в Dockerhub
```
    steps:
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
```
Предоставляем доступ к репозиторию
```
        name: repository checkout
        uses: actions/checkout@v4
```
Создание образа
```
        name: push_me_and_then_just_touch_me
        uses: docker/build-push-action@v5
        with:
          context: "./lab03"
          push: true
          tags: vladkilinich/lab03:latest
```

---  
# Вывод
В ходе лабораторной работы были созданы два yaml файла для описания ресурсов Kubernetes, был поднят кластер minikube, с помощью которого создали сервер.
