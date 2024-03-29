# Продвинутое использование СLI docker

Создание контейнера - `docker run -itd --name alpine nginx:alpine`
Остановка контейнера - `docker pause alpine`
```sh
[node1] (local) root@192.168.0.18 ~
$ docker run -itd --name alpine nginx:alpine

Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
c926b61bad3b: Pull complete
eb2797aa8e79: Pull complete
47df6ca4b6bc: Pull complete
5ea1ba8ab969: Pull complete
6a4b140a5e7c: Pull complete
c99555e79d52: Pull complete
f9302969eafd: Pull complete
d7fb62c2e1cc: Pull complete
Digest: sha256:3923f8de8d2214b9490e68fd6ae63ea604deddd166df2755b788bef04848b9bc
Status: Downloaded newer image for nginx:alpine
eb10d1c66c63b59d6cda3613dc812b56d0479edb0c4b195919c331c6e1bfca6e

[node1] (local) root@192.168.0.18 ~
$ docker pause alpine
alpine
[node1] (local) root@192.168.0.18 ~
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
eb10d1c66c63   nginx:alpine   "/docker-entrypoint.…"   56 seconds ago   Up 55 seconds   80/tcp    alpine
```
Отменить приостановку контейнера.
```sh
[node1] (local) root@192.168.0.18 ~
$ docker unpause alpine
alpine
[node1] (local) root@192.168.0.18 ~
```
Команда `docker info`
```sh
[node1] (local) root@192.168.0.18 ~
$ docker info
Client:
 Version:    24.0.7
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.11.2
    Path:     /usr/local/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.23.0
    Path:     /usr/local/libexec/docker/cli-plugins/docker-compose
  scout: Docker Scout (Docker Inc.)
    Version:  v1.0.9
    Path:     /usr/lib/docker/cli-plugins/docker-scout

Server:
 Containers: 1
  Running: 1
  Paused: 0
  Stopped: 0
 Images: 1
 Server Version: 24.0.7
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 091922f03c2762540fd057fba91260237ff86acb
 runc version: v1.1.9-0-gccaecfc
 init version: de40ad0
 Security Options:
  apparmor
  seccomp
   Profile: builtin
 Kernel Version: 4.4.0-210-generic
 Operating System: Alpine Linux v3.18 (containerized)
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 31.42GiB
 Name: node1
 ID: dc40ba1c-b0dd-4dec-944f-0e82d3c3962a
 Docker Root Dir: /var/lib/docker
 Debug Mode: true
  File Descriptors: 32
  Goroutines: 46
  System Time: 2023-12-02T13:51:28.797797597Z
  EventsListeners: 0
 Experimental: true
 Insecure Registries:
  127.0.0.1
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
```

Команда `docker logs alpine` выводит все логи, которые можно фильтровать.
```sh
[node1] (local) root@192.168.0.18 ~
$ docker logs alpine
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/12/14 11:59:24 [notice] 1#1: using the "epoll" event method
2023/12/14 11:59:24 [notice] 1#1: nginx/1.25.3
2023/12/14 11:59:24 [notice] 1#1: built by gcc 12.2.1 20220924 (Alpine 12.2.1_git20220924-r10)
2023/12/14 11:59:24 [notice] 1#1: OS: Linux 5.10.16.3-microsoft-standard-WSL2
2023/12/14 11:59:24 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/12/14 11:59:24 [notice] 1#1: start worker processes
2023/12/14 11:59:24 [notice] 1#1: start worker process 30
2023/12/14 11:59:24 [notice] 1#1: start worker process 31
2023/12/14 11:59:24 [notice] 1#1: start worker process 32
2023/12/14 11:59:24 [notice] 1#1: start worker process 33
2023/12/14 11:59:24 [notice] 1#1: start worker process 34
2023/12/14 11:59:24 [notice] 1#1: start worker process 35
2023/12/14 11:59:24 [notice] 1#1: start worker process 36
2023/12/14 11:59:24 [notice] 1#1: start worker process 37
2023/12/14 11:59:24 [notice] 1#1: start worker process 38
2023/12/14 11:59:24 [notice] 1#1: start worker process 39
2023/12/14 11:59:24 [notice] 1#1: start worker process 40
2023/12/14 11:59:24 [notice] 1#1: start worker process 41
2023/12/14 11:59:24 [notice] 1#1: start worker process 42
2023/12/14 11:59:24 [notice] 1#1: start worker process 43
2023/12/14 11:59:24 [notice] 1#1: start worker process 44
2023/12/14 11:59:24 [notice] 1#1: start worker process 45
```

```sh
[node1] (local) root@192.168.0.18 ~
$ docker attach web
2023/12/14 12:01:45 [notice] 45#45: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 44#44: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 42#42: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 43#43: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 41#41: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 40#40: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 38#38: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 39#39: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 37#37: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 36#36: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 35#35: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 34#34: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 33#33: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 32#32: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 31#31: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 1#1: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 30#30: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 45#45: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 44#44: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 43#43: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 42#42: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 41#41: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 40#40: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 39#39: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 38#38: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 37#37: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 36#36: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 35#35: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 34#34: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 33#33: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 32#32: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 30#30: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 31#31: signal 28 (SIGWINCH) received
2023/12/14 12:01:45 [notice] 1#1: signal 28 (SIGWINCH) received
```

Команда docker stats позволяет отслеживать статистику использования ресурсов контейнерами в реальном времени.
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                     PORTS     NAMES
eb10d1c66c63   nginx:alpine   "/docker-entrypoint.…"   2 minutes ago   Exited (0) 7 seconds ago             alpine