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

✳ **Лабораторная №1 (Простые команды)**

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


