# Резервное копирование

Его можно поделить на 2 части:

## Резервное копирование в docker.
Обновление пакетов внутри образа ubuntu.
```sh
$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
5e8117c0bd28: Pull complete 
Digest: sha256:8eab65df33a6de2844c9aefd19efe8ddb87b7df5e9185a4ab73af936225685bb
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

## Резервное копирование в docker с диском.

Docker build — команда подсистемы Docker, использующая файл Dockerfile и запускающая процесс создания образа.
```sh
docker build docker buildx
```

Docker Compose — это средство для определения и запуска приложений Docker с несколькими контейнерами.
```sh
docker-compose build docker label
---
docker-compose helthcheck
```

Docker context - контекст в Docker CLI, предоставляющий оптимизированный механизм для взаимодействия с несколькими конечными точками Docker. 
```sh
docker context remote connect docker
```

## Multi-stage 

Многоэтапная сборка необходима для уменьшения размера итогового образа, ведь каждая команда в Dockerfile добавляет отдельный слой к 
итоговому образу. Поэтому  всегда нужно помнить, что необходимо очищать любые не нужные нам артефакты в текущем слое, прежде чем 
перейти к следующему. Чтобы написать действительно эффективный Dockerfile, традиционно необходимо было использовать кучу shell-трюков, 
чтобы с одной стороны сделать как можно меньше слоев, а с другой, чтобы оставить в каждом созданном слое минимальное количество артефактов.

### Multi-stage на Golang

Docker образ в один этап. Его размер: 308MB.
```sh
FROM golang:1.16-alpine
WORKDIR /build
COPY go.mod . # go.sum
RUN go mod download
COPY . .
RUN go build -o /main main.go
ENTRYPOINT ["/main"]
```

Если же мы пересоберём этот образ, но с использованием многоэтапной сборки, то увидим, что размер стал: 11.8MB
```sh
FROM golang:1.16-alpine as builder
WORKDIR /build
COPY go.mod . # go.sum
RUN go mod download
COPY . .
RUN go build -o /main main.go
# Финальный этап, копируем собранное приложение
FROM alpine:3
COPY --from=builder main /bin/main
ENTRYPOINT ["/bin/main"]
```

В первом случае если разбираться то можем понять, что:
```sh
из размера golang:1.16-alpine - 302 MB
---
размер исходников - 0.5 MB
---
размер скомпилированного приложения - 6.2 MB
```

Во втором случае можем понять, что: 
```sh
размера alpine:3 - 5.59 MB
---
размер скомпилированного приложения - 6.2 MB
```
