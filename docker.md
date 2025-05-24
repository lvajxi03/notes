# Docker

Delete all containers:
```sh
$ docker rm $(docker ps -a -q)
```

Delete all images:
```sh
$ docker rmi $(docker images -q)
```