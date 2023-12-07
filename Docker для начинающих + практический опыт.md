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
---
# ✳ **Лабораторная №3 (IMAGES)**

❓*Сколько `images` доступно на докер-хосте?*
```bash
docker images
```
```
1
```

❓*Какой размер образа `ubuntu`?*
```
77.8MB
```

❓*Какой тег у нового образа `NGINX`?
ИНФО: Мы только что спуллили новый образ.*
```bash
docker images
1.19-alpine
```

❓*Мы только что скачали код приложения. Какой базовый образ используется в этом Dockerfile?
Ищи Dockerfile в директории `/var/webapp-rockets`.*
```bash
vi /var/webapp-rockets/Dockerfile 
или
grep -i FROM /var/webapp-rockets/Dockerfile
```

```
FROM python:3.6
RUN pip install flask
COPY . /opt/
EXPOSE 8080
WORKDIR /opt
ENTRYPOINT ["python3", "app.py"]                   
~                                                                     
~                                                  
"/var/webapp-rockets/Dockerfile" [readonly][noeol] 11L, 113C                          11,1          All
```

```
python:3.6
```

❓*В какое расположение внутри контейнера будет скопирован исходный код во время создания образа?*
```
COPY . /opt/
```

❓*Когда контейнер создан с помощью этого Dockerfile, какая команда используется для запуска приложения внутри него?*
```
ENTRYPOINT ["python3", "app.py"]
```

❓*Какой `port` у приложения внутри контейнера?*
```
EXPOSE 8080
```

❓*Создай докер-образ используя этот Dockerfile и назови его `webapp-rockets`. Не присваивай никаких тегов.*
```bash
Ctrl+C
:qa and enter
cd /var/webapp-rockets/; docker build . -t webapp-rockets
```

❓*Запусти экземпляр образа `webapp-rockets` и опубликуй порт `8080` контейнера на `30082` порту докер-хоста.*
```bash
docker run -d -p 30082:8080 webapp-rockets
```

❓*Открой веб-приложение используя вкладку приложения в обучающем модуле.
После выполнения ты можешь остановить работающий контейнер с помощью комбинации `CTRL + C` или командой `docker stop $(docker ps -q --filter ancestor=webapp-rockets)`.*

❓*Какая базовая ОС использована в образе `python:3.6`?
Если потребуется, ты всегда можешь запустить такой контейнер.
docker run python:3.6 cat /etc/*release*
```
cat: /etc/lsb-release: No such file or directory
PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
NAME="Debian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

❓*Какой примерный размер образа `webapp-rockets`?*
```bash
docker images
```
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
webapp-rockets      latest              3987f5ea8cb8        5 minutes ago       914MB
ubuntu              latest              e4c58958181a        7 weeks ago         77.8MB
python              3.6                 54260638d07c        23 months ago       902MB
nginx               1.19-alpine         a64a6e03b055        2 years ago         22.6MB
```

❓*Это действительно большой размер для образа. Докер-образы предполагаются как маленькие и легковесные. Давай немного обрежем его.*

❓*Создай новый образ этого приложения, который будет поменьше. Измени старый Dockerfile, назови образ `webapp-rockets`, дай ему тег `lite`.
Поищи базовый образ поменьше для python:3.6. Убедись, что размер готового образа будет меньше чем `150MB`.   
Пароль для sudo - `selena`*
```bash
sudo nano Dockerfile
ввести selena
изменить FROM python:3.6-alpine
docker build . -t webapp-rockets:lite
```

❓*Запусти новый контейнер из образа `webapp-rockets:lite` и прокинь порт `8080` контейнера на порт `30083` докер-хоста.*
```bash
docker run -d -p 30083:8080 webapp-rockets:lite
```
---
# ✳ **Лабораторная №4 (EnvVars)**

❓*Исследуй переменные окружения в запущенном контейнере и определи значение переменной `ROCKET_SIZE`*

```bash
docker inspect zen_villani
```

```
"Env": [
                "ROCKET_SIZE=average",
                "PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LANG=C.UTF-8",
                "GPG_KEY=0D96DF4D4110E5C43FBFB17F2D347EA6AA65421D",
                "PYTHON_VERSION=3.6.12",
                "PYTHON_PIP_VERSION=20.2.4",
                "PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/fa7dc83944936bf09a0e4cb5d5ec852c0d256599/get-pip.py",
                "PYTHON_GET_PIP_SHA256=6e0bb0a2c2533361d7f297ed547237caf1b7507f197835974c0dd7eba998c53c"
            ]
```

❓*Запусти контейнер с именем `rocket-app` из образа `rotorocloud/simple-webapp-rockets` и установи переменную `ROCKET_SIZE` в значение `big`. Сделай приложение доступным на порту `30888` докер-хоста. Приложение в контейнере работает на `8080` порту.*

```bash
docker run -p 30888:8080 --name rocket-app -e ROCKET_SIZE=big -d rotorocloud/simple-webapp-rockets
```

❓*Посмотри на работу приложения, используя ссылку вверху своего обучающего модуля и убедись, что программа показывает верный размер ракеты.*

❓*Разверни экземпляр базы `mysql` с помощью образа `mysql` и назови контейнер `mysql-db`.
Для этого контейнера установи пароль `db_pass123`. Ты можешь посмотреть правильный образ mysql на Docker Hub и там же узнать, как правильно проинициализировать root-пароль для ее контейнеризированной версии.*

    Name: mysql-db, Image: mysql, Env: MYSQL_ROOT_PASSWORD=db_pass123

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=db_pass123 --name mysql-db mysql
```

❓*Узнай, сколько баз данных создано в контейнере с `mysql`
База может подниматься около минуты.*

```bash
docker exec -i mysql-db mysql -uroot -pdb_pass123 <<< "show databases;"
```

```
Database
information_schema
mysql
performance_schema
sys
```

```
4
```

❓*Как видишь, без знания установленного пароля, база данных не дает совершать с собой никаких действий.
При помощи переменных окружения инициализируются большинство контейнеризированных приложений.*

❓*Попробуй cнова обратиться к базе и посмотри текущее значение переменной окружения `MYSQL_ROOT_PASSWORD`.
ИНФО: Мы что-то изменили, детально исследуй контейнер*

```
```

❓*Все верно, мы поменяли пароль в базе, но переменная осталась прежняя. Снова узнай, сколько баз данных создано в контейнере с `mysql`.
ИНФО: Новый пароль `db_newpass123`.*

```bash
docker exec -i mysql-db mysql -uroot -pdb_newpass123 <<< "show databases;"
```

```
Database
ROTORO
WAS_HERE
information_schema
mysql
performance_schema
sys
```

```
6
```

❓*Как ты убедился, переменные окружения не всегда отражают текущее положение дел. Очень часто (но не всегда) они используются для первоначальной инициализации переменных в контейнере, а дальше контейнер живет своей жизнью.*

---
# ✳ **Лабораторная №5 (Commands and Entrypoints)**

❓*Мы представили несколько `Dockerfiles` нескольких популярных продуктов в каталоге `/home/moon/`. Изучи их и ответь на несколько вопросов*

```bash
cd /home/moon/
ls
```

❓*Какая `ENTRYPOINT` установлена для создания образа nosql базы данных?
cat Dockerfile-mongodb*

```bash
FROM dockerfile/ubuntu
```

```
Install MongoDB.
RUN \
  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 && \
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' > /etc/apt/sources.list.d/mongodb.list && \
  apt-get update && \
  apt-get install -y mongodb-org && \
  rm -rf /var/lib/apt/lists/*

Define mountable directories.
VOLUME ["/data/db"]

Define working directory.
WORKDIR /data

Define default command.
CMD ["mongod"]

Expose ports.
- 27017: process
- 28017: http
EXPOSE 27017
```

```
ENTRYPOINT not set
```

❓*Какая `ENTRYPOINT` установлена для создания образа реляционной базы данных, коммюнити версии `mysql`?*

```bash
cat /home/moon/Dockerfile-mariadb
```

```
ENTRYPOINT ["docker-entrypoint.sh"]
```

❓*Какая `CMD` установлена для создания образа системы управления контентом `wordpress`?*

```bash
cat /home/moon/Dockerfile-wordpress
```

```
CMD ["apache2-foreground"]
```

❓*Какой будет окончательная команда при запуске контейнера из образа `wordpress` из данного Dockerfile? Прими во внимание и инструкцию `ENTRYPOINT`, и инструкцию `CMD`*

```
docker entrypoint and cmd
```

❓*Какая команда выполняется при запуске контейнера, созданного из Dockerfile с названием `ubuntu`?*

```bash
cat /home/moon/Dockerfile-ubuntu | grep CMD
```

```
CMD ["bash"]
```

❓*Запусти контейнер из образа `ubuntu` и переопредели команду (CMD) для старта в контейнере, заменив ее на `sleep 1000` 
Запусти это в `detached mode`.
Ubuntu container run with empty entrypoint
Ubuntu container run with 'sleep 1000' command*

```bash
docker run -d ubuntu sleep 1000
```
---
# ✳ **3.8 Тест: Images**

❓*`Dockerfile`:*
*FROM ubuntu:20.04 ** 
*COPY . /app*  
*RUN make /app*  
*CMD python /app/app.py*
*После сборки образ назвали `parser`. Что произойдет, если мы запустим команду: `docker run parser sleep 1h`?*

```
Docker изменит инструкцию CMD на 'sleep 1h'
```

❓*Выбери инструкцию CMD для команды `echo -n "My home is $HOME"`, которая выводит значение переменной окружения и считается **предпочитаемой**.*

```bash
CMD [ "sh", "-c", "echo -n My home is $HOME" ]
```

❓*Мы решили создать свой образ и в процессе его сборки требуется скачать файл с `https://rootfs.tar.xz` и автоматически распаковать пути `/`. Какую команду выбрать?*

```
ADD https://rootfs.tar.xz /
```

❓*Что поможет уменьшить размер образа?*

- [x] Предотвращение копирования нежелательных файлов в контекст сборки при помощи '.dockerignore'
- [ ] Объединить все инструкции с одинаковыми названиями в одну, например будет одна RUN, одна CMD и т.д.
- [x] Установка только необходимых пакетов в образ
- [ ] Объявление функций в Dockerfile
- [x] Использовать multi-stage сборки
- [x] Комбинирование нескольких инструкций в одну и очистка временных файлов в этой же инструкции

❓*Как определить, что Dockerfile содержит в себе multi-stage?*

```
В Dockerfile присутствуют несколько инструкций FROM
```

❓*`Dockerfile`:
FROM golang:1.12-alpine as builder
ENV GO111MODULE=on
WORKDIR /app  
COPY . .
RUN apk --no-cache add git alpine-sdk build-base gcc
RUN go get \  
    && go get golang.org/x/tools/cmd/cover \  
    && go get github.com/mattn/goveralls
RUN go build -o example cmd/example/main.go
FROM alpine:latest  
RUN apk --no-cache add ca-certificates  
WORKDIR /root/  
COPY --from=**<неизвестно>** /app/example .  
CMD ["./example"]
****Изучи Dockerfile. Какое значение нужно добавить после флага `--from` во второй стадии сборки, чтобы скомпилированный бинарник go-приложения из первой стадии был помещен в финальный образ?***

```
builder
```

❓*Какая команда напечатает значения 'Architecture' и 'Os' образа с названием `rockets`?*

```bash
docker image inspect rockets -f '{{.Os}} {{.Architecture}}'
```

❓*Если мы используем `CMD` для предоставления аргумента по умолчанию для инструкции `ENTRYPOINT`, то должны быть указаны обе инструкции, как `CMD`, так и `ENTRYPOINT`?*

```
Да
```

❓*Какая команда удалит все неиспользуемые образы на докер-хосте?*

```bash
docker image prune -a
```

❓*У нас есть директория с 
$ tree /opt/prj01/  
/opt/prj01/  
|-- Dockerfile  
|-- v1  
|   `-- app.py  
`-- v2  
    `-- app.py
Как собрать образ при помощи Dockerfile из директории `/opt/prj01/`, используя для контекста путь `/opt/prj01/v2` и назвать этот образ `app:v2`?*

```bash
docker build /opt/prj01/v2 -f /opt/prj01/Dockerfile -t app:v2
```

❓*Как увидеть список всех слоев образа `redis` вместе с размером каждого слоя?*

```bash
docker image history redis
```
---
# ✳ **4.2 Тест: Compose**

❓*При помощи чего мы можем настроить контейнеры и связь между ними используя декларативный подход?*

```
Docker Compose
```

❓*Какой YAML-файл, содержащий описание services, networks и volumes для развертывания контейнеризированного приложения, обычно используется?*

```
docker-compose.yaml
```

❓*Какая команда используется для создания и запуска контейнеров на переднем плане, используя готовый файл `docker-compose.yaml`?*

```
docker-compose up
```

❓*Какая команда используется для создания и запуска контейнеров на заднем плане, используя готовый файл `docker-compose.yaml`?*

- [ ] docker-compose up --background
- [x] docker-compose up -d
- [x] docker-compose up --detach
- [ ] docker-compose up

❓*Как посмотреть список созданных compose контейнеров?*

```bash
docker-compose ps
```

❓*Какая команда поможет в проверке логов всего стека, указанного в `docker-compose.yaml`?*

```
docker-compose logs
```

❓*Какой командой можно остановить (не удаляя) весь стек контейнеров, описанных в compose-файле?*

```
docker-compose stop
```

❓*Какой командой можно удалить весь стек контейнеров, описанных в compose-файле?*

```
docker-compose down
```

❓*Как используется поле `version` в compose-файле? (выбери 3)*

- [x] docker-compose использует максимально последнюю подходящую схему, не ориентируясь на версию
- [ ] docker-compose использует эту версию для выбора точной схемы проверки compose-файла
- [x] Поле версии носит лишь информативный характер
- [x] docker-compose анализирует файл и самостоятельно принимает решение о том, по схеме какой версии работать
- [ ] Это важный параметр, обеспечивающий обратную совместимость, на него полагается docker-compose

❓*Compose-файлы версий 2 и 3 должны явно указывать свою версию в корневом разделе YAML документа?*

```
Да
```

❓*Используя команду `docker-compose up` мы можем поднять несколько контейнеров на разных докер-хостах?*

```
Нет
```

❓*`docker-compose.yaml`:
version: "3.8"  
services:  
  web:  
    build: .  
    depends_on:  
      - db  
      - redis  
    volumes:  
      - .:/code  
      - logvolume01:/var/log  
    ports:  
      - "8080:80"  
  redis:  
    image: redis  
  db:  
    image: postgres  
volumes:  
  logvolume01: {}
На какой порт хоста будет выставлено приложение при исполнении `docker-compose up -d`?*

```
8080
```

❓*`docker-compose.yaml`:
version: "3.8"  
services:  
  web:  
    build: .  
    depends_on:  
      - db  
      - redis  
    volumes:  
      - .:/code  
      - logvolume01:/var/log  
    ports:  
      - "8080:80"  
  redis:  
    image: redis  
  db:  
    image: postgres  
volumes:  
  logvolume01: {}
Что из утверждений верно?*

```
Образ web будет собран, а образы redis и postgres будут скачаны с Dockerhub, если их еще нет на хосте
```

❓*`docker-compose.yaml`:
version: "3.8"  
services:  
  web:  
    build: .  
    depends_on:  
      - db  
      - redis  
    volumes:  
      - .:/code  
      - logvolume01:/var/log  
    ports:  
      - "8080:80"  
  redis:  
    image: redis  
  db:  
    image: postgres  
volumes:  
  logvolume01: {}
Что можно сказать о монтированиях для службы `web` в этом compose-файле?*
_Примечание: о монтировании мы будем говорить в теме хранения_*

```
/code это Bind Mount, /var/log это Volume mount
```

❓*`docker-compose.yaml`:
version: "3.8"  
services:  
  web:  
    build: .  
    depends_on:  
      - db  
      - redis  
    volumes:  
      - .:/code  
      - logvolume01:/var/log  
    ports:  
      - "8080:80"  
  redis:  
    image: redis  
  db:  
    image: postgres  
volumes:  
  logvolume01: {}
Что из утверждений верно об этом файле?*

```
Службы redis и db должны будут запуститься до службы web
```
---
# ✳ **Лабораторная №6 (Docker Compose)**

❓*Создай экземпляр базы данных `postgres` в контейнере с названием `db`, образ `postgres`, переменные окружения `POSTGRES_PASSWORD=mysecretpassword`
Container "db" running with env variable POSTGRES_PASSWORD?*

```bash
docker run --name db -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

❓*Теперь создай простой контейнер с `wordpress`, он должен называться `wordpress`, образ: `wordpress`, сделай `link` этому контейнеру к контейнеру с именем `db` выставь `30085` порт на докер-хост

```bash
docker run -d --name=wordpress --link db:db -p 30085:80 wordpress
```

❓*Сейчас у нас уже развернут рабочий сайт на `wordpress`, ты можешь посмотреть на него через вкладку в терминале. Давай теперь сделаем это с помощью `Docker Compose`!
Если ты видишь ответ `Страница недоступна`, поменяй в URL протокол на `https`*

```
OK
```

❓*Сначала надо прибрать за собой, давай удалим все, что осталось от предыдущих шагов.
Удали все контейнеры - `db` и `wordpress`

```bash
docker ps
docker stop wordpress db
docker rm wordpress db
```

❓*Создай файл `docker-compose.yml` в размещении `/root/wordpress`. Обрати внимание, размещение должно быть `/root/wordpress/docker-compose.yml`, иначе система не сможет правильно проверить результат. Сделай `sudo -i`, пароль `selena`. После этого запусти стек с помощью `docker-compose` в `detached mode`.
Важно, чтобы в файле спецификация была такой же, как и раньше, т.е. контейнеры были `wordpress` и `db` и не забудь прокинуть нужные порты*

```
sudo -i
selena

nano docker-compose.yml
version: '3.0'
services:
  db:
    environment:
      POSTGRES_PASSWORD: mysecretpassword
    image: postgres
  wordpress:
    image: wordpress
    links:
    - db
    ports:
    - 30085:80

docker-compose up -d
```


❓*Мы что-то поменяли, сколько реплик контейнеров в службе `wordpress`?*

```bash
docker-compose ps
```

```
0
```

❓*Увеличь количество реплик `wordpress` до 2.
Пользуйся средствами `docker-compose`*

```bash
docker-compose up -d --scale wordpress=2
docker-compose ps
```

❓*Останови стек `docker-compose`.
Убедись, что `Docker Compose` корректно положил стек*

```bash
docker-compose down
docker-compose ps
```

