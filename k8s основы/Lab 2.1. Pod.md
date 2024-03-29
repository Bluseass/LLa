# Pod

## 1. Создаём Pod

Нужно учитывать, чтобы вы находились вместе с этим файлом в одной директории   
(в моём случае /media/sf_FolderWinLin - общая папка с основной ОС)
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl create -f pod.yaml
pod/my-pod created
root@Ubunty-22:/media/sf_FolderWinLin# 
```

Проверяем запущенные поды:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|  NAME  | READY |  STATUS | RESTARTS | AGE |    
| ------ | ----- | ------- | -------- | --- |         
| my-pod |  1/1  | Running |    0     | 82s |
root@Ubunty-22:/media/sf_FolderWinLin#
```

Выполним команду:
```sh
kubectl delete -f pod.yaml
kubectl delete pod my-pod
kubectl delete pods -all
```

После чего проверим:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|  NAME  | READY |  STATUS | RESTARTS | AGE |    
| ------ | ----- | ------- | -------- | --- |         
| my-pod |  1/1  | Running |    0     | 10s |
root@Ubunty-22:/media/sf_FolderWinLin#
```
## 2. Скейлим приложение
Откроем pod.yaml редактором
```sh
apt install vim
vim pod.yaml
```

Меняем значение
```sh
-  name: my-pod
+  name: my-pod-1
```

Применяем изменения
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl apply -f pod.yaml
```

Проверяем результат
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|   NAME   | READY |  STATUS | RESTARTS | AGE |
| -------- | ----- | ------- | -------- | --- |
|  my-pod  |  1/1  | Running |    0     | 10m |   
| my-pod-1 |  1/1  | Running |    0     | 14s|
root@Ubunty-22:/media/sf_FolderWinLin#
```
## 3. Чистим за собой кластер
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete pod --all
pod "my-pod" deleted
pod "my-pod-1" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```
