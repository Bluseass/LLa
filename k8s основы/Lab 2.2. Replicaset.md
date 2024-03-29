# ReplicaSet

## 1. Создаем Replicaset
Выполним команду:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl apply -f replicaset.yaml
replicaset.apps/my-replicaset created
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod

|         NAME        |  READY  |   STATUS  |  RESTARTS | AGE |                           
| ------------------- | ------- | --------- | --------- | --- |
| my-replicaset-9pmpw |   1/1   |  Running  |     0     | 13s |
| my-replicaset-v52ww |   1/1   |  Running  |     0     | 13s |
root@Ubunty-22:/media/sf_FolderWinLin#
```

## 2. Скейлим Replicaset
Выполним команду:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl scale replicaset my-replicaset --replicas 3
replicaset.apps/my-replicaset scaled
root@Ubunty-22:/media/sf_FolderWinLin# 
```

Проверим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|         NAME        |  READY  |   STATUS  |  RESTARTS | AGE |                           
| ------------------- | ------- | --------- | --------- | --- |
| my-replicaset-9pmpw |   1/1   |  Running  |     0     | 59s |
| my-replicaset-m28cd |   1/1   |  Running  |     0     | 22s |
| my-replicaset-v52ww |   1/1   |  Running  |     0     | 59s |
root@Ubunty-22:/media/sf_FolderWinLin# 
```

Заскейлим на 2:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl scale replicaset my-replicaset --replicas 2
replicaset.apps/my-replicaset scaled
root@Ubunty-22:/media/sf_FolderWinLin# 
```

## 3. Удаляем один из Pod
Выполним команду:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete pod my-replicaset-9pmpw
pod "my-replicaset-9pmpw" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|         NAME        |  READY  |   STATUS  |  RESTARTS |  AGE |                           
| ------------------- | ------- | --------- | --------- | ---- |
| my-replicaset-8f6c6 |   1/1   |  Running  |     0     |  14s |
| my-replicaset-v52ww |   1/1   |  Running  |     0     | 113s |
root@Ubunty-22:/media/sf_FolderWinLin#
```
## 4. Добавляем в Replicaset лишний Pod
Редактируем файл:
```sh
vim pod1.yaml
```

Применяем изменения
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl apply -f pod41.yaml
pod/my-pod-1 created
root@Ubunty-22:/media/sf_FolderWinLin#
```

В итоге выводим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|         NAME        |  READY  |   STATUS  |  RESTARTS |   AGE  |                           
| ------------------- | ------- | --------- | --------- | ------ |
| my-replicaset-8f6c6 |   1/1   |  Running  |     0     |  8m15s |
| my-replicaset-v52ww |   1/1   |  Running  |     0     |  9m54s |
root@Ubunty-22:/media/sf_FolderWinLin#
```
## 5. Обновляем версию Image
Выполним команду:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl set image replicaset my-replicaset nginx=nginx:1.13
replicaset.apps/my-replicaset image updated
root@Ubunty-22:/media/sf_FolderWinLin# 
```

Проверим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
NAME                  READY   STATUS    RESTARTS   AGE
my-replicaset-8f6c6   1/1     Running   0          11m
my-replicaset-v52ww   1/1     Running   0          12m
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверяем сам Replicaset:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl describe replicaset my-replicaset
Name:         my-replicaset
Namespace:    default
Selector:     app=my-app
Labels:       <none>
Annotations:  <none>
Replicas:     2 current / 2 desired
Pods Status:  2 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=my-app
  Containers:
   nginx:
    Image:        nginx:1.13
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age    From                   Message
  ----    ------            ----   ----                   -------
  Normal  SuccessfulCreate  12m    replicaset-controller  Created pod: my-replicaset-v52ww
  Normal  SuccessfulCreate  12m    replicaset-controller  Created pod: my-replicaset-9pmpw
  Normal  SuccessfulCreate  12m    replicaset-controller  Created pod: my-replicaset-m28cd
  Normal  SuccessfulDelete  11m    replicaset-controller  Deleted pod: my-replicaset-m28cd
  Normal  SuccessfulCreate  11m    replicaset-controller  Created pod: my-replicaset-8f6c6
  Normal  SuccessfulDelete  4m40s  replicaset-controller  Deleted pod: my-pod-1
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверяем версию image в pod:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl describe pod my-replicaset-8f6c6
Name:             my-replicaset-8f6c6
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Thu, 21 Dec 2023 07:52:39 +0300
Labels:           app=my-app
Annotations:      <none>
Status:           Running
IP:               10.244.0.15
IPs:
  IP:           10.244.0.15
Controlled By:  ReplicaSet/my-replicaset
Containers:
  nginx:
    Container ID:   docker://54babe62c2b7e7af0620d386304bba0d67d0fbc45e560279a4f2a3902c9420ab
    Image:          quay.io/testing-farm/nginx:1.12
    Image ID:       docker-pullable://quay.io/testing-farm/nginx@sha256:09e210fe1e7f54647344d278a8d0dee8a4f59f275b72280e8b5a7c18c560057f
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 21 Dec 2023 07:52:40 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-kfjl7 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-kfjl7:
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
  Normal  Scheduled  12m   default-scheduler  Successfully assigned default/my-replicaset-8f6c6 to minikube
  Normal  Pulled     12m   kubelet            Container image "quay.io/testing-farm/nginx:1.12" already present on machine
  Normal  Created    12m   kubelet            Created container nginx
  Normal  Started    12m   kubelet            Started container nginx
root@Ubunty-22:/media/sf_FolderWinLin#
```

Помогаем поду обновиться:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete po my-replicaset-8f6c6
pod "my-replicaset-8f6c6" deleted
root@Ubunty-22:/media/sf_FolderWinLin# 
```

Смотрим результат:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl get pod
|         NAME        |  READY  |   STATUS  |  RESTARTS |   AGE  |                           
| ------------------- | ------- | --------- | --------- | ------ |
| my-replicaset-q2qd5 |   1/1   |  Running  |     0     |   28s  |
| my-replicaset-v52ww |   1/1   |  Running  |     0     |   15   |
root@Ubunty-22:/media/sf_FolderWinLin#
```

Проверяем версию нового Pod:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl describe pod my-replicaset-q2qd5
Name:             my-replicaset-q2qd5
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Thu, 21 Dec 2023 08:06:20 +0300
Labels:           app=my-app
Annotations:      <none>
Status:           Running
IP:               10.244.0.16
IPs:
  IP:           10.244.0.16
Controlled By:  ReplicaSet/my-replicaset
Containers:
  nginx:
    Container ID:   docker://161ac1029b9ea68c925edf1274dae64bf111acf11c899ed9897c4f62f1410a6e
    Image:          nginx:1.13
    Image ID:       docker-pullable://nginx@sha256:b1d09e9718890e6ebbbd2bc319ef1611559e30ce1b6f56b2e3b479d9da51dc35
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 21 Dec 2023 08:06:32 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-j9frx (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-j9frx:
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
  Normal  Scheduled  106s  default-scheduler  Successfully assigned default/my-replicaset-q2qd5 to minikube
  Normal  Pulling    104s  kubelet            Pulling image "nginx:1.13"
  Normal  Pulled     96s   kubelet            Successfully pulled image "nginx:1.13" in 8.105s (8.105s including waiting)
  Normal  Created    95s   kubelet            Created container nginx
  Normal  Started    94s   kubelet            Started container nginx
root@Ubunty-22:/media/sf_FolderWinLin#
```



## 6. Чистим за собой кластер
Чистим кластер:
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl delete replicaset --all
replicaset.apps "my-replicaset" deleted
root@Ubunty-22:/media/sf_FolderWinLin#
```

Документация(pod):
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl explain pod
KIND:       Pod
VERSION:    v1

DESCRIPTION:
    Pod is a collection of containers that can run on a host. This resource is
    created by clients and scheduled onto hosts.

FIELDS:
  apiVersion	<string>
    APIVersion defines the versioned schema of this representation of an object.
    Servers should convert recognized schemas to the latest internal value, and
    may reject unrecognized values. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

  kind	<string>
    Kind is a string value representing the REST resource this object
    represents. Servers may infer this from the endpoint the client submits
    requests to. Cannot be updated. In CamelCase. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

  metadata	<ObjectMeta>
    Standard object's metadata. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

  spec	<PodSpec>
    Specification of the desired behavior of the pod. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

  status	<PodStatus>
    Most recently observed status of the pod. This data may not be up to date.
    Populated by the system. Read-only. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
root@Ubunty-22:/media/sf_FolderWinLin#
```

Документация(pod.spec):
```sh
root@Ubunty-22:/media/sf_FolderWinLin# kubectl explain pod.spec
KIND:       Pod
VERSION:    v1
FIELD: spec <PodSpec>

DESCRIPTION:
    Specification of the desired behavior of the pod. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
    PodSpec is a description of a pod.

FIELDS:
  activeDeadlineSeconds	<integer>
    Optional duration in seconds the pod may be active on the node relative to
    StartTime before the system will actively try to mark it failed and kill
    associated containers. Value must be a positive integer.

  affinity	<Affinity>
    If specified, the pod's scheduling constraints

  automountServiceAccountToken	<boolean>
    AutomountServiceAccountToken indicates whether a service account token
    should be automatically mounted.

  containers	<[]Container> -required-
    List of containers belonging to the pod. Containers cannot currently be
    added or removed. There must be at least one container in a Pod. Cannot be
    updated.

  dnsConfig	<PodDNSConfig>
    Specifies the DNS parameters of a pod. Parameters specified here will be
    merged to the generated DNS configuration based on DNSPolicy.

  dnsPolicy	<string>
    Set DNS policy for the pod. Defaults to "ClusterFirst". Valid values are
    'ClusterFirstWithHostNet', 'ClusterFirst', 'Default' or 'None'. DNS
    parameters given in DNSConfig will be merged with the policy selected with
    DNSPolicy. To have DNS options set along with hostNetwork, you have to
    specify DNS policy explicitly to 'ClusterFirstWithHostNet'.

    Possible enum values:
     - `"ClusterFirst"` indicates that the pod should use cluster DNS first
    unless hostNetwork is true, if it is available, then fall back on the
    default (as determined by kubelet) DNS settings.
     - `"ClusterFirstWithHostNet"` indicates that the pod should use cluster DNS
    first, if it is available, then fall back on the default (as determined by
    kubelet) DNS settings.
     - `"Default"` indicates that the pod should use the default (as determined
    by kubelet) DNS settings.
     - `"None"` indicates that the pod should use empty DNS settings. DNS
    parameters such as nameservers and search paths should be defined via
    DNSConfig.

  enableServiceLinks	<boolean>
    EnableServiceLinks indicates whether information about services should be
    injected into pod's environment variables, matching the syntax of Docker
    links. Optional: Defaults to true.

  ephemeralContainers	<[]EphemeralContainer>
    List of ephemeral containers run in this pod. Ephemeral containers may be
    run in an existing pod to perform user-initiated actions such as debugging.
    This list cannot be specified when creating a pod, and it cannot be modified
    by updating the pod spec. In order to add an ephemeral container to an
    existing pod, use the pod's ephemeralcontainers subresource.

  hostAliases	<[]HostAlias>
    HostAliases is an optional list of hosts and IPs that will be injected into
    the pod's hosts file if specified. This is only valid for non-hostNetwork
    pods.

  hostIPC	<boolean>
    Use the host's ipc namespace. Optional: Default to false.

  hostNetwork	<boolean>
    Host networking requested for this pod. Use the host's network namespace. If
    this option is set, the ports that will be used must be specified. Default
    to false.

  hostPID	<boolean>
    Use the host's pid namespace. Optional: Default to false.

  hostUsers	<boolean>
    Use the host's user namespace. Optional: Default to true. If set to true or
    not present, the pod will be run in the host user namespace, useful for when
    the pod needs a feature only available to the host user namespace, such as
    loading a kernel module with CAP_SYS_MODULE. When set to false, a new userns
    is created for the pod. Setting false is useful for mitigating container
    breakout vulnerabilities even allowing users to run their containers as root
    without actually having root privileges on the host. This field is
    alpha-level and is only honored by servers that enable the
    UserNamespacesSupport feature.

  hostname	<string>
    Specifies the hostname of the Pod If not specified, the pod's hostname will
    be set to a system-defined value.

  imagePullSecrets	<[]LocalObjectReference>
    ImagePullSecrets is an optional list of references to secrets in the same
    namespace to use for pulling any of the images used by this PodSpec. If
    specified, these secrets will be passed to individual puller implementations
    for them to use. More info:
    https://kubernetes.io/docs/concepts/containers/images#specifying-imagepullsecrets-on-a-pod

  initContainers	<[]Container>
    List of initialization containers belonging to the pod. Init containers are
    executed in order prior to containers being started. If any init container
    fails, the pod is considered to have failed and is handled according to its
    restartPolicy. The name for an init container or normal container must be
    unique among all containers. Init containers may not have Lifecycle actions,
    Readiness probes, Liveness probes, or Startup probes. The
    resourceRequirements of an init container are taken into account during
    scheduling by finding the highest request/limit for each resource type, and
    then using the max of of that value or the sum of the normal containers.
    Limits are applied to init containers in a similar fashion. Init containers
    cannot currently be added or removed. Cannot be updated. More info:
    https://kubernetes.io/docs/concepts/workloads/pods/init-containers/

  nodeName	<string>
    NodeName is a request to schedule this pod onto a specific node. If it is
    non-empty, the scheduler simply schedules this pod onto that node, assuming
    that it fits resource requirements.

  nodeSelector	<map[string]string>
    NodeSelector is a selector which must be true for the pod to fit on a node.
    Selector which must match a node's labels for the pod to be scheduled on
    that node. More info:
    https://kubernetes.io/docs/concepts/configuration/assign-pod-node/

  os	<PodOS>
    Specifies the OS of the containers in the pod. Some pod and container fields
    are restricted if this is set.

    If the OS field is set to linux, the following fields must be unset:
    -securityContext.windowsOptions

    If the OS field is set to windows, following fields must be unset: -
    spec.hostPID - spec.hostIPC - spec.hostUsers -
    spec.securityContext.seLinuxOptions - spec.securityContext.seccompProfile -
    spec.securityContext.fsGroup - spec.securityContext.fsGroupChangePolicy -
    spec.securityContext.sysctls - spec.shareProcessNamespace -
    spec.securityContext.runAsUser - spec.securityContext.runAsGroup -
    spec.securityContext.supplementalGroups -
    spec.containers[*].securityContext.seLinuxOptions -
    spec.containers[*].securityContext.seccompProfile -
    spec.containers[*].securityContext.capabilities -
    spec.containers[*].securityContext.readOnlyRootFilesystem -
    spec.containers[*].securityContext.privileged -
    spec.containers[*].securityContext.allowPrivilegeEscalation -
    spec.containers[*].securityContext.procMount -
    spec.containers[*].securityContext.runAsUser -
    spec.containers[*].securityContext.runAsGroup

  overhead	<map[string]Quantity>
    Overhead represents the resource overhead associated with running a pod for
    a given RuntimeClass. This field will be autopopulated at admission time by
    the RuntimeClass admission controller. If the RuntimeClass admission
    controller is enabled, overhead must not be set in Pod create requests. The
    RuntimeClass admission controller will reject Pod create requests which have
    the overhead already set. If RuntimeClass is configured and selected in the
    PodSpec, Overhead will be set to the value defined in the corresponding
    RuntimeClass, otherwise it will remain unset and treated as zero. More info:
    https://git.k8s.io/enhancements/keps/sig-node/688-pod-overhead/README.md

  preemptionPolicy	<string>
    PreemptionPolicy is the Policy for preempting pods with lower priority. One
    of Never, PreemptLowerPriority. Defaults to PreemptLowerPriority if unset.

    Possible enum values:
     - `"Never"` means that pod never preempts other pods with lower priority.
     - `"PreemptLowerPriority"` means that pod can preempt other pods with lower
    priority.

  priority	<integer>
    The priority value. Various system components use this field to find the
    priority of the pod. When Priority Admission Controller is enabled, it
    prevents users from setting this field. The admission controller populates
    this field from PriorityClassName. The higher the value, the higher the
    priority.

  priorityClassName	<string>
    If specified, indicates the pod's priority. "system-node-critical" and
    "system-cluster-critical" are two special keywords which indicate the
    highest priorities with the former being the highest priority. Any other
    name must be defined by creating a PriorityClass object with that name. If
    not specified, the pod priority will be default or zero if there is no
    default.

  readinessGates	<[]PodReadinessGate>
    If specified, all readiness gates will be evaluated for pod readiness. A pod
    is ready when all its containers are ready AND all conditions specified in
    the readiness gates have status equal to "True" More info:
    https://git.k8s.io/enhancements/keps/sig-network/580-pod-readiness-gates

  resourceClaims	<[]PodResourceClaim>
    ResourceClaims defines which ResourceClaims must be allocated and reserved
    before the Pod is allowed to start. The resources will be made available to
    those containers which consume them by name.

    This is an alpha field and requires enabling the DynamicResourceAllocation
    feature gate.

    This field is immutable.

  restartPolicy	<string>
    Restart policy for all containers within the pod. One of Always, OnFailure,
    Never. In some contexts, only a subset of those values may be permitted.
    Default to Always. More info:
    https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy

    Possible enum values:
     - `"Always"`
     - `"Never"`
     - `"OnFailure"`

  runtimeClassName	<string>
    RuntimeClassName refers to a RuntimeClass object in the node.k8s.io group,
    which should be used to run this pod.  If no RuntimeClass resource matches
    the named class, the pod will not be run. If unset or empty, the "legacy"
    RuntimeClass will be used, which is an implicit class with an empty
    definition that uses the default runtime handler. More info:
    https://git.k8s.io/enhancements/keps/sig-node/585-runtime-class

  schedulerName	<string>
    If specified, the pod will be dispatched by specified scheduler. If not
    specified, the pod will be dispatched by default scheduler.


  schedulingGates	<[]PodSchedulingGate>
    SchedulingGates is an opaque list of values that if specified will block
    scheduling the pod. If schedulingGates is not empty, the pod will stay in
    the SchedulingGated state and the scheduler will not attempt to schedule the
    pod.

    SchedulingGates can only be set at pod creation time, and be removed only
    afterwards.

    This is a beta feature enabled by the PodSchedulingReadiness feature gate.

  securityContext	<PodSecurityContext>
    SecurityContext holds pod-level security attributes and common container
    settings. Optional: Defaults to empty.  See type description for default
    values of each field.

  serviceAccount	<string>
    DeprecatedServiceAccount is a depreciated alias for ServiceAccountName.
    Deprecated: Use serviceAccountName instead.

  serviceAccountName	<string>
    ServiceAccountName is the name of the ServiceAccount to use to run this pod.
    More info:
    https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

  setHostnameAsFQDN	<boolean>
    If true the pod's hostname will be configured as the pod's FQDN, rather than
    the leaf name (the default). In Linux containers, this means setting the
    FQDN in the hostname field of the kernel (the nodename field of struct
    utsname). In Windows containers, this means setting the registry value of
    hostname for the registry key
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters to
    FQDN. If a pod does not have FQDN, this has no effect. Default to false.

  shareProcessNamespace	<boolean>
    Share a single process namespace between all of the containers in a pod.
    When this is set containers will be able to view and signal processes from
    other containers in the same pod, and the first process in each container
    will not be assigned PID 1. HostPID and ShareProcessNamespace cannot both be
    set. Optional: Default to false.

  subdomain	<string>
    If specified, the fully qualified Pod hostname will be
    "<hostname>.<subdomain>.<pod namespace>.svc.<cluster domain>". If not
    specified, the pod will not have a domainname at all.

  terminationGracePeriodSeconds	<integer>
    Optional duration in seconds the pod needs to terminate gracefully. May be
    decreased in delete request. Value must be non-negative integer. The value
    zero indicates stop immediately via the kill signal (no opportunity to shut
    down). If this value is nil, the default grace period will be used instead.
    The grace period is the duration in seconds after the processes running in
    the pod are sent a termination signal and the time when the processes are
    forcibly halted with a kill signal. Set this value longer than the expected
    cleanup time for your process. Defaults to 30 seconds.

  tolerations	<[]Toleration>
    If specified, the pod's tolerations.

  topologySpreadConstraints	<[]TopologySpreadConstraint>
    TopologySpreadConstraints describes how a group of pods ought to spread
    across topology domains. Scheduler will schedule pods in a way which abides
    by the constraints. All topologySpreadConstraints are ANDed.

  volumes	<[]Volume>
    List of volumes that can be mounted by containers belonging to the pod. More
    info: https://kubernetes.io/docs/concepts/storage/volumes
root@Ubunty-22:/media/sf_FolderWinLin#
```

