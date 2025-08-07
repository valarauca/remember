docker
---

Stuff I frequently forget about docker

## Run an image interactively

```shell
$ docker run -it "${yourImage}" /bin/bash
```

## Copy items from a running docker image

Fangy globbing patterns don't work

```shell
$ docker ps -a
CONTAINER ID   IMAGE                                      COMMAND                  CREATED        STATUS                      PORTS     NAMES
510a7ab9b507   nginx:latest                               "/docker-entrypoint.…"   21 hours ago   Exited (127) 21 hours ago             silly_galois
$ docker cp 510a7ab9b507:/idk/some/file /some/local/path
```

## Save a running image

When you have a container running in interactive mode and you want to save the state of it

```
$ docker ps -a
CONTAINER ID   IMAGE                                      COMMAND                  CREATED        STATUS                      PORTS     NAMES
510a7ab9b507   nginx:latest                               "/docker-entrypoint.…"   21 hours ago   Exited (127) 21 hours ago             silly_galoi
$ docker commit 510a7ab9b507 "${name}"
# then you reboot idk
$ docker run -it "${name}" /bin/bash
```

