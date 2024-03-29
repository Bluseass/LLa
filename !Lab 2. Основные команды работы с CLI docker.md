# !Lab 2. Основные команды работы с CLI docker.md

Благодаря лабораторной работы я изучил основные команды в Docker, необходимые для: 

Создание и запуск контейнера:
```sh
docker container run my_image
```

Сборка образа:
```sh
docker image build -t my_repo/my_image:my_tag .
```

Пуш образа в удалённый репозиторий:
```sh
docker image push my_repo/my_image:my_tag
```

## Список всех команд, использованных в ходе лабораторной работы в Play with Docker

Создаём и запускаем nginx контейнер
```sh
docker container run nginx
```
Просмотр всех образов
```sh
docker images
```
Список контейнеров (которые UP и EXIT)
```sh
docker ps -a
```
Запуск контейнера
```sh
docker start (name_container or container_id)
```
Остановка контейнера 
```sh
docker kill (name_container or container_id)
```
Удаление контейнера
```sh
docker rm (container_id)
```
Запуск контейнера с привязкой к портам
```sh
docker container run -d -p 80:80 --name proxy nginx
```
Список образов
```sh
docker image ls
```
