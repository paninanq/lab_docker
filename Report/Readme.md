# Отчет по лабораторной работе №4
## Шаг 1 - запуск в контейнере приложения aafire
Напишем Dockerfile. 
```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y libaa-bin fortune 
```
Далее в терминале в папке с этим файлом запускаем команду сборки образа с тегом “libaa-bin”.
```
docker build -t libaa-bin .
```
Подключимся к контейнеру напрямую и запустим aafire.
```
docker run -it libaa-bin aafire
```
![aafire](/Report/aafire.jpg)
## Шаг 2 - настройка сети между двумя контейнерами
Так как для проверки сети требуется утилита ping, изменим образ контейнера.
```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y libaa-bin fortune && apt-get install iputils-ping -y
```
Запустим два контейнера и оставим их в рабочем состоянии.
Создадим сеть при помощи команды
```
docker network create myNetwork
```
Посмотрим названия работающих контейнеров с помощью команды 
```
docker ps
```
![ps](/Report/ps.jpg)
Подключим контейнеры к сети
```
docker network connect myNetwork nice_cohen
docker network connect myNetwork beautiful_swartz

```
Посмотрим ip-адреса контейнеров в настройках сети с помощью команды
```
docker network inspect myNetwork
```
![set](/Report/network.jpg)
Видим, что ip-адрес beautiful_swartz - 172.18.0.3, ip-адрес nice_cohen - 172.18.0.2
Проверим доступ из контейнера beautiful_swartz в контейнер nice_cohen 
![ping2](/Report/ping2.jpg)
Проверим доступ из контейнера nice_cohen в контейнер beautiful_swartz
![ping2](/Report/ping1.jpg)
Таким образом, сеть между двумя контейнерами настроена.
