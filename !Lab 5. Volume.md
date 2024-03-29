# Volume

Для управления данными приложения, необходимо использовать память хостовой системы. Рассмотрим существующие подходы к монтированию:
 - Volume(тома) хранятся в части файловой системы хоста, управляемой Docker (/ var / lib / docker / volume / в Linux). Процессы, не относящиеся к Docker, не должны изменять эту часть файловой системы. Тома - предпочтительный способ сохранить данные в Docker.
 - Bind(привязка существующих каталогов внутрь контейнеров). Данный подход к монтированию может хранить данные в любом месте хост-системы. Процессы, не относящиеся к Docker, на хосте Docker или в контейнере Docker могут изменять их в любое время.
 - Tmpfs mounts хранятся только в временной памяти хоста и никогда не записываются в его файловую систему.

Volume можно создать явно, используя команду docker volume create. В другом случае Docker может создать том во время создания контейнера или службы.
 - Обмен данными между несколькими запущенными контейнерами. Если вы не создаете его явно, том создается при первом монтировании в контейнер. 
 - Когда хосту Docker не гарантируется наличие заданного каталога или файловой структуры. Тома помогают отделить конфигурацию хоста Docker от среды выполнения контейнера.
 - Если вы хотите хранить данные своего контейнера на удаленном хосте или у облачного провайдера, а не локально.
 - Когда вам нужно создать резервную копию, восстановить или перенести данные с одного хоста Docker на другой, тома - лучший выбор. 
 - Когда вашему приложению требуется высокопроизводительный ввод-вывод на рабочем столе Docker.
 - Когда вашему приложению требуется полностью собственное поведение файловой системы на Docker Desktop.

## Volume

Создаем Volume
```sh
docker volume create storage
docker volume create db
```

Список всех Volume
```sh
docker volume ls
```

Запуск контейнера с MongoDB
```sh
docker container run -d -v storage:/data/db --name mongo_test mongo
```

Проинспектируем
```sh
docker container inspect mongo_test 
```

Результат инспектирования
```sh
"Mounts": [
            {
                "Type": "volume",
                "Name": "05edef188be24ef93ce1eeb8e6bb1454fc53d659b491cedf7880efc453ad67bf",
                "Source": "/var/lib/docker/volumes/05edef188be24ef93ce1eeb8e6bb1454fc53d659b491cedf7880efc453ad67bf/_data",
                "Destination": "/data/configdb",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "storage",
                "Source": "/var/lib/docker/volumes/storage/_data",
                "Destination": "/data/db",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ]
```

CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS       NAMES
0012333a02b0   mongo     "docker-entrypoint.s…"   About a minute ago   Up About a minute   27017/tcp   mongo_test