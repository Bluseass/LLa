# Практическое задание: 

### 1. Создание Pod: 
 - Создайте YAML-файл для Pod с именем "my-pod.yaml".
 - Определите метаданные и спецификацию Pod.
 - Запустите Pod с помощью команды kubectl apply -f my-pod.yaml.
 - Проверьте статус запущенного Pod с помощью команды kubectl get pods.

### 2. Масштабирование ReplicaSet:
 - Создайте YAML-файл для ReplicaSet с именем "my-replicaset.yaml".
 - Определите метаданные, спецификацию ReplicaSet и шаблон Pod.
 - Запустите ReplicaSet с помощью команды kubectl apply -f my-replicaset.yaml.
 - Проверьте количество запущенных подов с помощью команды kubectl get pods.
 - Измените количество реплик в ReplicaSet с помощью команды kubectl scale replicaset my-replicaset --replicas=5.
 - Убедитесь, что количество запущенных подов увеличилось до 5.

### 3. Развёртывание приложения с использованием Deployment:
 - Создайте YAML-файл для Deployment с именем "my-deployment.yaml".
 - Определите метаданные, спецификацию Deployment и шаблон Pod.
 - Запустите Deployment с помощью команды kubectl apply -f my-deployment.yaml.
 - Проверьте количество запущенных подов с помощью команды kubectl get pods.
 - Обновите образ контейнера в Deployment с помощью команды kubectl set image deployment/my-deployment my-container=my-new-image:latest.
 - Убедитесь, что обновление образа контейнера прошло успешно и новые поды с новым образом были развернуты.

### 4. Удаление объектов:
 - Удалите Pod с помощью команды kubectl delete pod my-pod.
 - Удалите ReplicaSet с помощью команды kubectl delete replicaset my-replicaset.
 - Удалите Deployment с помощью команды kubectl delete deployment my-deployment.

# Выполнение заданий:

## 1. Создание Pod:
Имеем файл с названием my-pod.yaml
```sh
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
  containers:
  - name: my-container
    image: nginx:latest
```

Выполним следующие команды:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl apply -f my-pod.yaml
pod/my-pod created
root@Ubunty-22:/media/sf_FolderWinLin#
```

После чего - проверим результат
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pods
NAME     READY   STATUS              RESTARTS   AGE
my-pod   0/1     ContainerCreating   0          27s
root@Ubunty-22:/media/sf_FolderWinLin#
```

## 2. Масштабирование ReplicaSet:
Имеем файл my-replicaset.yaml
```sh
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
```

Выполним следующие команды:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl apply -f my-replicaset.yaml
replicaset.apps/my-replicaset created
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
my-replicaset-54p8h   1/1     Running   0          28s
my-replicaset-727zx   1/1     Running   0          28s
my-replicaset-lrqp6   1/1     Running   0          28s
root@Ubunty-22:/media/sf_FolderWinLin#
```

Чтобы изменить колво реплик - делаем следующее:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl scale replicaset my-replicaset --replicas=5
replicaset.apps/my-replicaset scaled
root@Ubunty-22:/media/sf_FolderWinLin#
```

Видим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
my-replicaset-54p8h   1/1     Running   0          91s
my-replicaset-727zx   1/1     Running   0          91s
my-replicaset-9nls8   1/1     Running   0          11s
my-replicaset-kfk98   1/1     Running   0          11s
my-replicaset-lrqp6   1/1     Running   0          91s
root@Ubunty-22:/media/sf_FolderWinLin#
```

## 3. Развёртывание приложения с использованием Deployment:
Имеем файл my-deployment.yaml
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
```

Делаем следующее:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl apply -f my-deployment.yaml
deployment.apps/my-deployment created
root@Ubunty-22:/media/sf_FolderWinLin#
```

Смотрим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
my-deployment-584545b6d-9hx6w   1/1     Running   0          28s
my-deployment-584545b6d-9qbrh   1/1     Running   0          28s
my-deployment-584545b6d-fqkf8   1/1     Running   0          28s
root@Ubunty-22:/media/sf_FolderWinLin#
```

Обновляем образ контейнера
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl set image deployment/my-deployment my-container=my-new-image:latest
deployment.apps/my-deployment image updated
root@Ubunty-22:/media/sf_FolderWinLin#
```

Смотрим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pods
NAME                             READY   STATUS             RESTARTS   AGE
my-deployment-584545b6d-9hx6w    1/1     Running            0          79s
my-deployment-584545b6d-9qbrh    1/1     Running            0          79s
my-deployment-584545b6d-fqkf8    1/1     Running            0          79s
my-deployment-6bf4b8898f-whnp6   0/1     ImagePullBackOff   0          24s
root@Ubunty-22:/media/sf_FolderWinLin#
```


## 4. Удаление объектов:
Удаляем Pod:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete pod my-pod
pod "my-pod" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```

Удаляем ReplicaSet
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete replicaset my-replicaset
replicaset.apps "my-replicaset" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```

Удаляем Deployment
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete deployment my-deployment
deployment.apps "my-deployment" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```
