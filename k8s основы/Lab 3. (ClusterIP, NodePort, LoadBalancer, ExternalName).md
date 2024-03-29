# Практическое задание:

1. Создание ClusterIP сервиса:
- Создайте манифест для пода, который будет представлять ваше приложение.
- Создайте манифест для ClusterIP сервиса, который будет пробрасывать трафик на под вашего приложения.
- Примените оба манифеста в вашем кластере Kubernetes.
- Убедитесь, что сервис и под работают корректно, путем проверки доступности сервиса из других подов в кластере.

2. Создание NodePort сервиса:
 - Создайте манифест для пода, который будет представлять ваше приложение.
 - Создайте манифест для NodePort сервиса, который будет пробрасывать трафик на под вашего приложения через открытый порт на каждом узле кластера.
 - Примените оба манифеста в вашем кластере Kubernetes.
 - Убедитесь, что сервис и под работают корректно, путем проверки доступности сервиса с помощью IP-адреса и порта узла кластера.

3. Создание LoadBalancer сервиса:
 - Создайте манифест для пода, который будет представлять ваше приложение.
 - Создайте манифест для LoadBalancer сервиса, который будет автоматически создавать и настраивать балансировщик нагрузки в вашей облачной среде.
 - Примените оба манифеста в вашем кластере Kubernetes.
 - Убедитесь, что сервис и под работают корректно, путем проверки доступности сервиса через внешний IP-адрес балансировщика нагрузки.

4. Создание ExternalName сервиса:
 - Создайте манифест для ExternalName сервиса, который будет проксировать DNS-имя к внешнему ресурсу из вашего кластера.
 - Примените манифест в вашем кластере Kubernetes.
 - Убедитесь, что сервис работает корректно, путем проверки доступности внешнего ресурса через DNS-имя сервиса.

# Выполнение заданий:
## 1. Создание ClusterIP сервиса:
Созданим простой манифест для Pod и ClusterIP сервиса. Приложение будет в виде веб-сервера на порту 80.   
Делаем два манифеста: "my-app-pod.yaml" и "my-app-service.yaml".   

my-app-pod.yaml
```sh
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
labels:
  app: my-app
spec:
  containers:
  - name: my-app-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

my-app-service.yaml:
```sh
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Применим оба манифеста:
```sh
kubectl apply -f my-app-pod.yaml
kubectl apply -f my-app-service.yaml
```

Проверяем что Pod и сервис работают нормально. Чтобы убедиться что сервис доступен из других подов в кластере, создаём временный под и используем `curl` для отправки запроса к сервису.
```sh
kubectl run -i --tty --rm debug --image=nginx -- bash
apt-get update && apt-get install curl
curl my-app-service
```

## 2. Создание NodePort сервиса:
Делаем два манифеста. Один для Pod, другой для сервиса.
my-app-pod.yaml:
```sh
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
labels:
  app: my-app
spec:
  containers:
  - name: my-app-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

my-app-service-nodeport.yaml:
```sh
apiVersion: v1
kind: Service
metadata:
  name: my-app-service-nodeport
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

Применяем манифесты:
```sh
kubectl apply -f my-app-pod.yaml
kubectl apply -f my-app-service-nodeport.yaml
```

Проверяем что Pod и сервис работают корректно. Проверяем доступонсть сервиса.
```sh
kubectl get nodes -o wide
```
## 3. Создание LoadBalancer сервиса:
Создадим два манифеста: для Pod и LoadBalancer. 

my-app-pod.yaml:
```sh
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
labels:
  app: my-app
spec:
  containers:
  - name: my-app-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

my-app-service-lb.yaml:
```sh
apiVersion: v1
kind: Service
metadata:
  name: my-app-service-lb
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

Применяем оба манифеста:
```sh
kubectl apply -f my-app-pod.yaml
kubectl apply -f my-app-service-lb.yaml
```

Проверяем доступность сервиса
```sh
kubectl get services my-app-service-lb -o wide
```


## 4. Создание ExternalName сервиса:
Создаём манифест
my-externalname-service.yaml:
```sh
apiVersion: v1
kind: Service
metadata:
  name: my-externalname-service
spec:
  type: ExternalName
  externalName: google.com  # Замените это на ваш внешний ресурс
```

Применяем манифест
```sh
kubectl apply -f my-externalname-service.yaml
```

Проверяем работу сервиса
```sh
kubectl get services
```

Убеждаемся что он работает корректно
```sh
kubectl run -i --tty --rm debug --image=alpine --restart=Never -- sh
apk add --no-cache curl
curl my-externalname-service
```

