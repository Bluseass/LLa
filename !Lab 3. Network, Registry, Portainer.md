# !Lab 3. Network, Registry, Portainer.md

## Network - необходим для связи между контейнерами.

Работа с сетью
```sh
# Создание сети (по-умолчанию тип: bridge)
# тип: Bridge - позволяет соединить контейнер с сетью docker.
docker network create my-net

# Проверка сети
docker network ls

# Удалить сеть
docker network rm my-net
```

Вывод подробной информации о сети
```sh
docker network inspect my-net
```

Запуск контейнера
```sh
docker run -d -p 5000:5000 --restart always --name reg registry:2

где:
Флаг **-d** - Режим detach (Работа в фоновом режиме);
Флаг **-p** - Сокращение от **--port**;
restart always - Всегда перезагружать, при выключении;
name - Указываем название контейнера;
```


## Registry - 

### Portainer - Необходим для управления некоторыми службами. Кроме того позволяет управлять жизненным циклом контейнера, запущенным на хосте.

Установка Portainer
```sh
docker run -d -p 9000:9000 --restart always -v "/var/run/docker.sock:/var/run/docker.sock" --name portainer portainer/portainer-ce
```

Команда для отслеживания статистики использования ресурсов контейнерами
```sh
docker stats
```
Вывод:

CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O   PIDS
21488b393eeb   reg         0.00%     6.684MiB / 12.42GiB   0.05%     726B / 0B   0B / 0B     9
bd0b45ca47cc   portainer   0.09%     11.5MiB / 12.42GiB    0.09%     976B / 0B   0B / 0B     12

| CONTAINER ID | NAME | CPU % | MEM USAGE / LIMIT | MEM % | NET I/0 | BLOCK I/O | PIDS |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| 664071007f5a | portainer | 0.00% | 12.63MiB / 31.42GiB | 0.04% | 0B / 0B | 0B / 528kB | 9 |
| 156f30e064e5 | reg-web | 0.39% | 1.28gIb / 31.42GiB | 4.09% | 13.7kB / 129kB | 0B / 0B | 41 |
| 230f3233c327 | reg | 0.01% | 8.383MiB / 31.42GiB | 0.03% | 1.32kB / 1.12kB | 0B / 0B | 9 |
| 9a4e7a7b76da | my-nginx | 0.00% | 10.71MiB / 31.42GiB | 0.03% | 84B / 0B | 0B / 8.19kB | 9 |


### Лимит на ресурсы
Можно регулировать лимиты на использование тех или иных ресуров. В нашем случае это будут:

**(1) Ограничение памяти в Mb до 50MiB:**
```sh
docker run -d -p 8083:80 -m 50m nginx
```
**(2) Ограничение памяти в Mb до 50MiB:**
CONTAINER ID   NAME                    CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O   PIDS
5ead0055728c   unruffled_brahmagupta   0.00%     12.77MiB / 50MiB      25.54%    656B / 0B     0B / 0B     17
21488b393eeb   reg                     0.00%     6.637MiB / 12.42GiB   0.05%     796B / 0B     0B / 0B     9
bd0b45ca47cc   portainer               0.00%     11.5MiB / 12.42GiB    0.09%     1.05kB / 0B   0B / 0B     12

## Практическое задание
1. Остановить все запущенные контейнеры, кроме Portainer. Удалить все контейнеры, образы, диски, и всё остальное неиспользуемое
```sh
**docker stop $(docker ps -a -q)** - Остановка всех контейнеров.
**docker container rm $(docker ps -a -q)** - Удаляет все контейнеры.
**docker image prune -a** - Удаляет все образы.
**docker volume rm $(docker volume ls -q)** - Удаляет все диски.
**docker system prune -a** - Удаляет все остановленные контейнеры и образы(неиспользуемые).
```
2. Установите докер registry от данного автора
```sh
docker pull jc21/registry-ui - Установка Doker Registry UI 
```
3. Установите Shipyard и подключите к API docker
```sh
docker pull shipyard/shipyard - Установка Shipyard
```docker pull shipyard/shipyard
