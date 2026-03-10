# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

## Задание 1: hub.docker.com 

<p align="center">
  <br>
  <em>https://hub.docker.com/r/ollrins/custom-nginx</em>
</p>

## Задание 2: Запуск custom-nginx

<p align="center">
  <img src="screenshots/task2-docker-ps.png" alt="Выполнение команд задания 2" width="900"/>
  <br>
  <em>Рисунок 1 - Запуск контейнера, переименование и проверка. Доступность страницы через curl</em>
</p>



## Задание 3: Изменение конфигурации nginx

<p align="center">
  <img src="screenshots/task3-attach.png" alt="docker attach" width="800"/>
  <br>
  <em>Рисунок 2 - Подключение к контейнеру и остановка через Ctrl+C</em>
</p>

<p align="center">
  <img src="screenshots/task3-edit-config.png" alt="Редактирование конфига" width="850"/>
  <br>
  <em>Рисунок 3 - Изменение порта с 80 на 81 через nano</em>
</p>

<p align="center">
  <img src="screenshots/task3-curl-inside.png" alt="Проверка внутри контейнера. Проверка с хоста" width="750"/>
  <br>
  <em>Рисунок 4 - Проверка curl внутри контейнера (80 не работает, 81 работает). Проверка с хостовой машины (не работает)</em>
</p>

<p align="center">
  <img src="screenshots/task3-curl-host.png" alt="Дополнительное задание" width="700"/>
  <br>
  <em>Рисунок 5 - Дополнительное задание</em>
</p>


## Задание 4: Общая папка между контейнерами

<p align="center">
  <img src="screenshots/task4-create-dir.png" alt="Общая папка между контейнерами" width="700"/>
  <br>
  <em>Рисунок 6 - Создание рабочей директории. Запуск контейнера CentOS с volume. Запуск контейнера Debian с volume. Создание файла из контейнера CentOS. Создание файла на хостовой машине. Проверка файлов из контейнера Debian </em>
</p>

<p align="center">
  <br>
  <em>Будет пояснение</em>
</p>



## Задание 5: Portainer и локальный registry

<p align="center">
  <img src="screenshots/task5-create-files.png" alt="Создание файлов" width="800"/>
  <br>
  <em>Рисунок 7 - Создание compose.yaml и docker-compose.yaml. Запуск docker compose up -d (только portainer). Добавление include в compose.yaml. Оба контейнера (portainer и registry) запущены </em>
</p>

<p align="center">
  <img src="screenshots/task5-compose.png" alt="Stack в Portainer" width="900"/>
  <br>
  <em>Рисунок 8 - compose.yaml</em>
</p>

<p align="center">
  <img src="screenshots/task5-push-to-registry.png" alt="Push в registry" width="850"/>
   <img src="screenshots/task5-push.png" alt="Push в registry" width="850"/>
  <br>
  <em>Рисунок 9 - Загрузка образа в локальный registry</em>
</p>



<p align="center">
  <img src="screenshots/task5-portainer-inspect.png" alt="Inspect в Portainer" width="1000"/>
  <br>
  <em>Рисунок 10 - Просмотр Config контейнера (AppArmorProfile до Driver)</em>
</p>

<p align="center">
  <img src="screenshots/task5-warning.png" alt="Warning" width="800"/>
  <br>
  <em>Рисунок 11 - Предупреждение об отсутствующем файле. Остановка проекта одной командой docker compose down</em>
</p>


## Задача 1: Подготовка и публикация образа

#### Проверка Docker
```bash
docker --version
docker compose version
sudo systemctl restart docker
```
### Скачивание образа
```bash
docker pull nginx:1.29.0
```
# Подготовка файлов
```bash
mkdir ~/nginx-custom
cd ~/nginx-custom
```
### Создание index.html
```bash
cat > index.html <<EOF
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
EOF
```
### Создание Dockerfile
```bash
cat > Dockerfile <<EOF
FROM nginx:1.29.0
COPY index.html /usr/share/nginx/html/index.html
EOF
```
### Сборка образа
```bash
docker build -t ollrins/custom-nginx:1.0.0 .
```
### Вход в Docker Hub
```bash
docker login -u
```
### Отправка образа
```bash
docker push ollrins/custom-nginx:1.0.0
```
## Задание 2: Запуск и управление контейнером

### Запуск контейнера
```bash
docker run -d --name "ФИО-custom-nginx-t2" -p 127.0.0.1:8080:80 ollrins/custom-nginx:1.0.0
```
### Переименование контейнера
```bash
docker rename "ФИО-custom-nginx-t2" custom-nginx-t2
```
### Проверочная команда
```bash
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
```
### Проверка через curl
```bash
curl http://127.0.0.1:8080
```

## Задание 3: Работа с контейнером (attach, exec, изменение конфига)

#### Подключение к потокам ввода/вывода
```bash
docker attach custom-nginx-t2
```
(нажать Ctrl+C)

#### Проверка статуса
```bash
docker ps -a
```
#### Перезапуск
```bash
docker start custom-nginx-t2
docker ps
```
#### Вход в контейнер
```bash
docker exec -it custom-nginx-t2 bash
```
#### Внутри контейнера:
```bash
apt-get update
apt-get install nano -y
nano /etc/nginx/conf.d/default.conf
```
#### (заменить listen 80 на listen 81, сохранить)
```bash
nginx -s reload
curl http://127.0.0.1:80
curl http://127.0.0.1:81
exit
```
#### Проверка с хоста
```bash
ss -tlpn | grep 127.0.0.1:8080
docker port custom-nginx-t2
curl http://127.0.0.1:8080
```

----------------------------------------------
## Исправляем конфигурацию контейнера вручную
Шаг 1: Останавливаем контейнер
```bash
docker stop custom-nginx-t2
```
Шаг 2: Находим папку с конфигами контейнера
#### Узнаем полный ID контейнера
```bash
docker inspect custom-nginx-t2 --format='{{.Id}}'
```
#### Для root-установки Docker:
```bash
ls -la /var/lib/docker/containers/ПОЛНЫЙ_ХЭШ/
```
#### Или если Docker запущен от текущего пользователя (rootless):
```bash
ls -la ~/.local/share/docker/containers/ПОЛНЫЙ_ХЭШ/
```
Шаг 3: Создаем бэкапы (на всякий случай)
```bash
cd /var/lib/docker/containers/ПОЛНЫЙ_ХЭШ/
cp hostconfig.json hostconfig.json.backup
cp config.v2.json config.v2.json.backup
```
Шаг 4: Редактируем hostconfig.json
Этот файл отвечает за привязку портов хоста к портам контейнера.
```bash
sudo nano hostconfig.json
json
"PortBindings": {
    "80/tcp": [
        {
            "HostIp": "127.0.0.1",
            "HostPort": "8080"
        }
    ]
}
```
Нам нужно изменить порт контейнера с 80 на 81 (так как nginx теперь слушает 81):
```bash
json
"PortBindings": {
    "81/tcp": [
        {
            "HostIp": "127.0.0.1",
            "HostPort": "8080"
        }
    ]
}
```
Шаг 5: Редактируем config.v2.json
Этот файл содержит конфигурацию самого контейнера, включая то, какие порты он экспортирует.
```bash
sudo nano config.v2.json
json
"Config": {
    "ExposedPorts": {
        "80/tcp": {}
    },
    ...
}
```
Меняем "80/tcp" на "81/tcp":

Шаг 6: Перезапускаем Docker 
После изменения файлов вручную нужно перезапустить Docker, чтобы он перечитал конфигурацию:
```bash
sudo systemctl restart docker
```
Шаг 7: Запускаем контейнер и проверяем
```bash
docker start custom-nginx-t2
docker port custom-nginx-t2
curl http://127.0.0.1:8080
```
hostconfig.json — изменили привязку: хост-порт 8080 теперь связан с контейнер-портом 81

config.v2.json — изменили список экспортируемых портов контейнера

----------------------------------------------

#### Удаление контейнера (не останавливая)
```bash
docker rm -f custom-nginx-t2
docker ps -a | grep custom-nginx-t2
```
## Задание 4: Общая папка (volumes) между контейнерами

#### Создание рабочей директории
```bash
mkdir -p ~/docker-share-test
cd ~/docker-share-test
pwd
```
#### Запуск контейнера CentOS
```bash
docker run -d --name centos-share -v $(pwd):/data centos:7 sleep infinity
```
#### Запуск контейнера Debian
```bash
docker run -d --name debian-share -v $(pwd):/data debian:latest sleep infinity
```
#### Проверка запущенных контейнеров
```bash
docker ps
```
#### Создание файла из CentOS
```bash
docker exec -it centos-share bash
cd /data
echo "Этот файл создан из CentOS контейнера" > from-centos.txt
ls -la
exit
```
#### Создание файла на хосте
```bash
cd ~/docker-share-test
echo "Этот файл создан на хостовой машине" > from-host.txt
ls -la
```
#### Проверка из Debian
```bash
docker exec -it debian-share bash
cd /data
ls -la
cat from-centos.txt
cat from-host.txt
exit
```
#### Финальная проверка на хосте
```bash
cd ~/docker-share-test
ls -la
cat from-centos.txt
cat from-host.txt
```
#### Остановка контейнера (если нужно)
```bash
docker stop 8400456e656f  # ID конкретного контейнера
```
## Задание 5: Portainer и локальный registry
```bash
# Создание директории
mkdir -p /tmp/netology/docker/task5
cd /tmp/netology/docker/task5
pwd
```
### Создание compose.yaml
```bash
cat > compose.yaml <<EOF
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
EOF
```
### Создание docker-compose.yaml
```bash
cat > docker-compose.yaml <<EOF
services:
  registry:
    image: registry:2
    ports:
    - "5000:5000"
EOF
```
#### Проверка файлов
```bash
ls -la
cat compose.yaml
cat docker-compose.yaml
```
#### Первый запуск (только portainer)
```bash
docker compose up -d
docker ps
```
#### Остановка
```bash
docker compose down
```
#### Редактирование compose.yaml с include
```bash
cat > compose.yaml <<EOF
include:
  - docker-compose.yaml

services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
EOF
```
#### Запуск обоих сервисов
```bash
docker compose up -d
docker ps
```
### Работа с локальным registry
```bash
docker images
docker tag ollrins/custom-nginx:1.0.0 127.0.0.1:5000/custom-nginx:latest
docker push 127.0.0.1:5000/custom-nginx:latest
curl http://127.0.0.1:5000/v2/_catalog
```
#### Удаление файла и предупреждение
```bash
rm docker-compose.yaml
ls -la
docker compose up -d
```
(наблюдаем warning)

#### Исправление compose.yaml
```bash
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
EOF
```
#### Остановка проекта одной командой
```bash
docker compose down
```
Дополнительные команды для навигации и проверки

#### Просмотр всех контейнеров
```bash
docker ps -a
```
#### Просмотр образов
```bash
docker images
```
#### Просмотр логов
```bash
docker logs custom-nginx-t2
```
#### Остановка всех контейнеров
```bash
docker stop $(docker ps -q)
```
#### Удаление всех остановленных контейнеров
```bash
docker container prune
```



