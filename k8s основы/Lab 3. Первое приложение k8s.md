# Первое приложение k8s
## Развернем простое приложение на go

Необходимо скачать готовое приложение на Go:
```sh
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, Gopher!")
}

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8888", nil)
}
```

Инициализируем репозиторий Git и делаем коммит:
```sh
git init
git add main.go
git commit -m "Initial commit"
```

Создаём Dockerfile
```sh
# Используем официальный образ Golang в качестве базового
FROM golang:latest

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /app

# Копируем исходный код в контейнер
COPY . .

# Собираем приложение
RUN go build -o main .

# Указываем порт, который будет открыт в контейнере
EXPOSE 8888

# Команда, которая будет запущена при старте контейнера
CMD ["./main"]
```

Собираем образ:
```sh
docker build -t demo_hello
```

Проверим что всё работает:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# docker images
REPOSITORY                    TAG           IMAGE ID       CREATED              SIZE
<none>                        <none>        3abaa7d110a6   About a minute ago   340MB
demo_hello                    latest        766424879132   About a minute ago   2.31MB
```

Создадим манифест:
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
  labels:
    app: hello
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello
        image: 333355/demo_hello
        ports:
        - containerPort: 8888
```

Применяем манифест:
```sh
kubectl apply -f deploy_demo_hello.yml
```

Проверим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# k get pod
NAME                     READY   STATUS    RESTARTS   AGE
hello-723ba2abbd-bz2x   1/1     Running   0          11m
root@Ubunty-22:/media/sf_FolderWinLin# k get deployment
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
hello   1/1     1            1           33m
```

Создаём сервис
```sh
apiVersion: v1
kind: Service
metadata:
  name: hello
  labels:
    app: hello
spec:
  selector:
    app: hello
  ports:
    - protocol: TCP
      port: 8888
      targetPort: 8888
  type: LoadBalancer
```

Пробросим порты:
```sh
kubectl port-forward service/hello 9999:8888
```
