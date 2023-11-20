# [Docker для начинающих + практический опыт](https://stepik.org/course/123300/promo)

Установка VirtualBox, Ubuntu: 
*Перед установкой VB проверить в BIOS возможность виртуализации Enabled*
+ [Установка И Настройка Виртуальный Машины Ubuntu Для Python Разработчика | Linux VirtualBox](https://www.youtube.com/watch?v=2FkDJPPm8kk)
+ [virtualbox.org](https://www.virtualbox.org/wiki/Downloads)
+ [ubuntu.com](https://ubuntu.com/download/desktop)
+ [Команды Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/)

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
docker/whalesay: sudo docker run docker/whalesay cowsay Hi, Aleksei!
```
---
# ✳ **Лабораторная №1 (Простые команды)**

❓*Какая версия Docker Engine запущена на хосте?*
```bash
docker version
```

❓*Сколько контейнеров в статусе (RUNNING) запущено в хосте?*
```bash
docker ps
```

❓*Сколько образов лежит на докер хосте?*
```bash
docker images
```

❓*Запустить контейнер с образом redis*
```bash
docker run redis
```

❓*Останови только что созданный контейнер*
```bash
Ctrl + C
```

❓*Сколько контейнеров (RUNNING) запущено в хосте?*
```bash
docker ps
```

❓*Сколько контейнеров (RUNNING) запущено в хосте? (Мы создали несколько контейнеров)*
```bash
docker ps
```

❓*Сколько всего контейнеров (PRESENT) на хосте? (Включая работающие и остановленные)*
```bash
docker ps -a
```

❓*Какой образ использован для запуска контейнера nginx1?*
```bash
nginx:alpine
```

❓*Какое название у контейнера созданного из образа ubuntu?*
```bash
drunkin_mendeleev
```

❓*Какой ID у контейнера использующего образ alpine и в данный момент не работающего?*
```bash
указать id образа
```

❓*Какое состояние у остановленного контейнера с alpine?*
```bash
exited
```

❓*Удали все контейнеры с докер-хоста. Удали все: RUNNUNG and NOT RUNNING. Не забудь, что перед удалением контейнеры нужно остановить*
```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker ps -a
```

❓*Удали образ ubuntu*
```bash
docker rmi ubuntu
```

❓*Тебе требуется докер-образ, который понадобится для запуска позже. Pull образ nginx:1.14-alpine. Только скачай образ, но не создавай контейнер*
```bash
docker pull nginx:1.14-alpine
```

❓*Запусти контейнер с образом nginx:1.14-alpine и дай ему имя webapp*
```bash
docker run --name webapp nginx:1.14-alpine
```

❓*Cleanup: удали все images на хосте. Удали все необходимые образы*
```bash
docker rmi -f $(docker images)
```
---
# ✳ **Лабораторная №2 (Особенности RUN)**

❓*Вначале давай исследуем окружение. Сколько контейнеров запущено на хосте?*
```bash
docker ps
```

❓*Какой образ использует контейнер?*
```
nginx:alpine 
```

❓*Сколько портов только IPV4*
```
2
```

❓*Какие номера портов выставлены в `контейнер`?*
```
80 и 6006
```

❓*Какие номера портов выставлены с `хоста`?*
```
4d0f0c345a30        nginx:alpine        "/docker-entrypoint.…"   5 minutes ago       Up 5 minutes        0.0.0.0:6006->6006/tcp, 0.0.0.0:30080->80/tcp   webportal
```

```
30080 и 6006
```

❓*Запусти контейнер `rotorocloud/simple-webapp-rockets` с тегом `v2`, соспоставь порт 8080 контейнера к порту 30123 хоста.*

```bash
docker run -p 30123:8080 rotorocloud/simple-webapp-rockets:v2
```

❓*Ты можешь увидеть веб-приложение, открыв кастомный порт 30123 через меню обучающего модуля или кликнув по значку приложения вверху, рядом с подсказкой.
кликнуть i
Мы ошиблись при запуске контейнера `webportal`, его внешний порт должен быть 30500. Исправь это. Не забудь назвать его `webportal`.
Посмотри информацию о старом контейнере. Порты невозможно переопределить на запущенном контейнере.
Name: webportal, Image: nginx:alpine, Container port 80, Host Port: 30500*

```bash
docker stop webportal
  
docker rm webportal  
  
docker run --name=webportal -d -p 30500:80 -p 6006:6006 nginx:alpine
```

❓*Какой `MacAddress` адрес у контейнера `webportal`?*
```bash
docker inspect webportal
```
```json
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "bfaca9a07a1f191c99ecbe97bbb4de840ee4889ee2cdcb8d6ffde1a74c016e7d",
                    "EndpointID": "65daa9c6fb262165086ef133ce03e1329534596ab0d5aa6cc1e35990fd50e9a5",
                    "Gateway": "172.20.230.1",
                    "IPAddress": "172.20.230.2",
                    "IPPrefixLen": 24,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:14:e6:02",
                    "DriverOpts": null
                }
```
                
❓*Какой внутренний IP адрес у контейнера `webportal`?*
```bash
docker inspect webportal
```
```
"IPAddress": "172.20.230.2",
```

❓*Посмотри логи контейнера `webportal`, какие статусы там в основном присутствуют?*
```bash
docker logs webportal
```
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/11/20 17:46:59 [notice] 1#1: using the "epoll" event method
2023/11/20 17:46:59 [notice] 1#1: nginx/1.25.3
2023/11/20 17:46:59 [notice] 1#1: built by gcc 12.2.1 20220924 (Alpine 12.2.1_git20220924-r10) 
2023/11/20 17:46:59 [notice] 1#1: OS: Linux 5.15.0-46-generic
2023/11/20 17:46:59 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/11/20 17:46:59 [notice] 1#1: start worker processes
2023/11/20 17:46:59 [notice] 1#1: start worker process 78
2023/11/20 17:46:59 [notice] 1#1: start worker process 79
2023/11/20 17:46:59 [notice] 1#1: start worker process 80
2023/11/20 17:46:59 [notice] 1#1: start worker process 81
2023/11/20 17:46:59 [notice] 1#1: start worker process 82
2023/11/20 17:46:59 [notice] 1#1: start worker process 83
2023/11/20 17:46:59 [notice] 1#1: start worker process 84
2023/11/20 17:46:59 [notice] 1#1: start worker process 85
2023/11/20 17:46:59 [notice] 1#1: start worker process 86
2023/11/20 17:46:59 [notice] 1#1: start worker process 87
172.20.230.1 - - [20/Nov/2023:17:53:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"
172.20.230.1 - - [20/Nov/2023:17:53:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"
172.20.230.1 - - [20/Nov/2023:17:53:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"
172.20.230.1 - - [20/Nov/2023:17:53:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"
172.20.230.1 - - [20/Nov/2023:17:53:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"
```
---
# ✳ **2.9 Тест: команды**

❓*Как удалить все запущенные и остановленные контейнеры на хосте? (Выбери 3)*

- [x] docker rm -f $(docker ps -aq) 
- [ ] docker container rm $(docker container ls -q) 
- [ ] docker container stop $(docker container ls -q) 
- [ ] docker container stop $(docker container ps -q) 
- [x] docker container rm -f $(docker container ps -aq)  
- [x] docker container rm -f $(docker container ls -aq) 

❓*Какая из команд создаст (не запустит) контейнер с образом `redis` и именем `redis`?*
```bash
docker container create --name redis redis
```

❓*Мы развернули несколько контейнеров. Как увидеть, какой из них потребляет больше всего памяти?*
```bash
docker container stats
```

❓*Как запустить контейнер с названием `webserver`  с образом `nginx` в интерактивном режиме? (Выбери 2)*
  
- [ ] docker container run -it nginx
- [ ] docker container run -it nginx --name webserver
- [x] docker run -it --name webserver nginx
- [x] docker container run -it --name webserver nginx

❓*Какая команда используется для обновления restart policy контейнера redis в значение `always`?*
```bash
docker container update --restart always redis
```

❓*Как скопировать директорию `nginx` из контейнера `webapp` на хост по пути `/tmp/`?*
```bash
docker container cp webapp:/etc/nginx /tmp/
```

❓*Можно ли сопоставить несколько контейнеров с одним и тем же портом к одному порту на докер-хосте?*
```
Нет
```

❓*Какой командой мы можем проверить драйвер логирования по умолчанию?*
```bash
docker system info
```

❓*Как запустить контейнер `redis` и быть уверенным, что для него не будет настроено логов?*
```bash
docker run -it -d --log-driver none redis
```

❓*Как проверить статус службы `docker` на хосте?*
  
```bash
sudo systemctl status docker
```

❓*При помощи какого механизма Docker сопоставляет порты контейнера и хоста?*
> **Встроенный балансировщик нагрузки:**
Docker содержит встроенный балансировщик нагрузки, который можно использовать для распределения нагрузки между несколькими контейнерами. Это можно сделать, настроив разные контейнеры на работу на одном и том же порту и указав балансировщику нагрузки, какие контейнеры должны принимать трафик.

> **Используя правила FirewallD:**
FirewallD - это программное обеспечение для настройки брандмауэра в Linux, которое может использоваться для управления сетевыми правилами для контейнеров Docker. Например, вы можете настроить FirewallD для разрешения или запрета доступа к определенным портам в контейнерах Docker.

> **При помощи правил IPTables:**
IPTables - это программа для настройки брандмауэра в Linux, которая может использоваться для управления сетевыми правилами для контейнеров Docker. Например, вы можете настроить IPTables для разрешения или запрета доступа к определенным портам в контейнерах Docker.

> **Через подключаемый сетевой аддон:**
Docker позволяет использовать подключаемые сетевые аддоны для настройки сетевых связей между контейнерами и хостом. Например, вы можете использовать сетевой аддон overlay network для создания сети, в которой могут работать несколько контейнеров. Каждый контейнер будет иметь свой уникальный IP-адрес, который может быть использован для управления доступом к портам контейнера

```
При помощи правил IPTables
```

❓*Какой флаг настроит Docker на проброс порта 80 UDP в контейнере на порт 8080 докер-хоста?*
```bash
-p 8080:80/udp
```

❓*Если не указано иное, Docker публикует открытый порт на всех сетевых интерфейсах?*
> Да, если не указано иное, то Docker публикует открытый порт на всех сетевых интерфейсах. Это означает, что порт будет доступен для подключения как с локальной машины, так и с других машин в сети, если они имеют доступ к сети, на которой запущен Docker.
> Чтобы определить, на каком сетевом интерфейсе будет опубликован порт, можно явно указать IP-адрес интерфейса в команде `docker run`. Например, для публикации порта на интерфейсе с IP-адресом 192.168.0.10 необходимо использовать параметр `-p 192.168.0.10:8080:80`, где 8080 - порт на хост-машине, а 80 - порт в контейнере.

```
Да
```

❓*Какой командой мы можем получить сводку важной информации о контейнере `webapp` (например сетевые адреса, переменные окружения, политику рестарта и т.д.)?*
```bash
docker container inspect webapp
```

❓*Чем из приведенного мы можем получить поток логов контейнера `my-app`?*
```bash
docker container logs -f my-app
```
