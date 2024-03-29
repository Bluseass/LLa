# Deployment

## 1. Создаем deployment

Error from server (BadRequest): error when creating "deployment.yaml": Deployment in version "v1" cannot be handled as a Deployment: strict decoding error: unknown field "spec.strategy.rollingUpdate.maxUnaivailable"
`###################################################################################################################################`
Решение следующее: 
```sh
Открыть файл deployment.yaml и удалить в нём следующие строчки:
strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnaivailable: 5
```

Проверяем список pods:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
NAME                             READY   STATUS    RESTARTS   AGE
my-deployment-5fdc88b77b-5l9nd   1/1     Running   0          2m13s
my-deployment-5fdc88b77b-lqvb4   1/1     Running   0          2m13s
root@Ubunty-22:/media/sf_FolderWinLin#
```
Проверяем список replicaset:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get replicaset
NAME                       DESIRED   CURRENT   READY   AGE
my-deployment-5fdc88b77b   2         2         2       2m29s
root@Ubunty-22:/media/sf_FolderWinLin#
```

## 2. Обновляем версию image
Обновляем версию image:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl set image deployment my-deployment nginx=nginx:1.13
deployment.apps/my-deployment image updated
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверяемр результат
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
NAME                             READY   STATUS    RESTARTS   AGE
my-deployment-7d846675f9-b7vb5   1/1     Running   0          23s
my-deployment-7d846675f9-ltlmq   1/1     Running   0          26s
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверяем что в новых pod новый image:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl describe pod my-deployment-7d846675f9-ltlmq
Name:             my-deployment-7d846675f9-ltlmq
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Thu, 21 Dec 2023 08:24:41 +0300
Labels:           app=my-app
                  pod-template-hash=7d846675f9
Annotations:      <none>
Status:           Running
IP:               10.244.0.19
IPs:
  IP:           10.244.0.19
Controlled By:  ReplicaSet/my-deployment-7d846675f9
Containers:
  nginx:
    Container ID:   docker://c0ff016b0445c3e4e95203fd5c94a00aedc5a6812732c1cff0515470235543d4
    Image:          nginx:1.13
    Image ID:       docker-pullable://nginx@sha256:b1d09e9718890e6ebbbd2bc319ef1611559e30ce1b6f56b2e3b479d9da51dc35
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 21 Dec 2023 08:24:43 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-6b7k6 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-6b7k6:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  93s   default-scheduler  Successfully assigned default/my-deployment-7d846675f9-ltlmq to minikube
  Normal  Pulled     92s   kubelet            Container image "nginx:1.13" already present on machine
  Normal  Created    91s   kubelet            Created container nginx
  Normal  Started    91s   kubelet            Started container nginx
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверяем что стало с replicaset:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get replicaset
NAME                       DESIRED   CURRENT   READY   AGE
my-deployment-5fdc88b77b   0         0         0       5m25s
my-deployment-7d846675f9   2         2         2       2m19s
root@Ubunty-22:/media/sf_FolderWinLin# 
```

## 3. Чистим за собой кластер
Чистим класер:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete deployment --all
deployment.apps "my-deployment" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```
