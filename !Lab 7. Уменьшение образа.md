# Уменьшение образа

Всегда необходимо следить за размером образа, т.к. память и общее количество потребляемых ресурсов - всегда является критичной проблемой.

Для начала необходимо вывести все имеющиеся образы командой `docker image ls` (Если образа нет, создать при помощи команды `docker container run {container_name}`.
| REPOSITORY | TAG | IMAGE ID | CREATED | SIZE |
| ------ | ------ | ------ | ------ | ------ |
| nginx | latest | a6bd71f48f68 | 11 days ago | 187MB |

Командой `docker history nging:latest` можно вывести содержимое нашего контейнера послойно.
```sh
docker history nging:latest

IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
a6bd71f48f68   3 weeks ago   /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
<missing>      3 weeks ago   /bin/sh -c #(nop) COPY file:9e3b2b63db9f8fc7…   4.62kB
<missing>      3 weeks ago   /bin/sh -c #(nop) COPY file:57846632accc8975…   3.02kB
<missing>      3 weeks ago   /bin/sh -c #(nop) COPY file:3b1b9915b7dd898a…   298B
<missing>      3 weeks ago   /bin/sh -c #(nop) COPY file:caec368f5a54f70a…   2.12kB
<missing>      3 weeks ago   /bin/sh -c #(nop) COPY file:01e75c6dd0ce317d…   1.62kB
<missing>      3 weeks ago   /bin/sh -c set -x     && groupadd --system -…   112MB
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bookworm   0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENV NJS_VERSION=0.8.2        0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENV NGINX_VERSION=1.25.3     0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>      3 weeks ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>      3 weeks ago   /bin/sh -c #(nop) ADD file:d261a6f6921593f1e…   74.8MB
```

## Особенности сборки пложеноий

### Пользователь не должен быть root
Особенность OC Linux в том, что каждый образ, который основывается на ней - изначально делает все действия из под пользователя root. Из-за этого при сборке необходимо использовать Обычного пользователя.
```sh
FROM alpine
WORKDIR /newuser
RUN addgroup -S newgroup && adduser -D newuser -G newgroup
RUN chown -R newuser /newuser
USER newuser
RUN whoami
```

### Использование переменных окружения

#### Dockerfile
В Dockerfile переменные окружения устанавливаются как ключ-значение. 
```sh
Посмотреть установленные переменные можно командой env.

ENV DIRNAME=/name
ENV DIRPATH=/path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd
```