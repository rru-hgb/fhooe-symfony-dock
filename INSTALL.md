# Install LAMPP  Environment with Docker

## Not familiar with Docker?

[Start without install on Play-With-Docker](https://labs.play-with-docker.com/)

## Install Docker

- [Download Docker Desktop for Windows or MAC](https://www.docker.com/products/docker-desktop)
- [Install Docker for Windows](https://docs.docker.com/desktop/windows/install/)
- [Install Docker for MAC](https://docs.docker.com/desktop/mac/install/)
- [Install Docker for Linux](https://docs.docker.com/engine/install/)

## Install fhooe-web-dock and start Containers
Open Powershell (PS) or other Terminal (prompt my be different then).

`PS ???> ` 
```shell
cd <path-where-your-Docker-should-reside>
```
`PS path-where-your-Docker-should-reside> `
```shell
mkdir Docker # if not already exists
```
```shell
cd Docker
```
`PS path-to-Docker> `
Replace *docker-git-repository* with the appropriate Docker Repository.
```shell
git clone *docker-git-repository*
```
```shell
cd *docker-git-repository*
```

## Starting Containers

```shell
docker compose up -d
```

## Refreshing containers after bug fixing
```shell
docker compose up --force-recreate --build -d
```
## Cleaning Up and Re-Install
You can clean up using Docker Desktop
- Stop and remove Containers
- Remove Images 
- Remove Volumes 

### Clean Re-Install with Script
- Go to directory, where docker-compose.yml is installed.
- Type .\CleanReinstall.sh for Linux or MAC or in a Windows Powershell
- or doubleclick on CleanReinstall.bat for Windows 

See [Starting Container](#starting-containers) to start again.

manually deleting only images for fhooe-web-dock
```shell
docker image rm fhooe-web-dock_php-apache
docker image rm fhooe-web-dock_db
docker image rm phpmyadmin/phpmyadmin
```

# Toubleshooting chrome + hsts
- [Follow Link for Instructions](https://superuser.com/questions/1400200/chrome-persistently-redirecting-to-https-for-http-site)
- enter `chrome://net-internals/#hsts` in Chrome Browser
- goto end of site.
- enter "localhost" and press "Delete"
```
# Delete domain security policies
Input a domain name to delete its dynamic domain security policies (HSTS and Expect-CT).(You cannot delete preloaded entries.):
Domain:[example.com -> localhost][Delete]
```

# More Commands to work with Containers and Images

## Stopping the Containers
```shell
docker compose stop
```
Result
```shell
 - Container pma      Stopped                                                                                  4.2s 
 - Container webapp   Stopped                                                                                  4.1s 
 - Container mariadb  Stopped
```
## Starting the Containers without rebuilding
```shell
docker compose start
```

## See which images exist
```shell
docker image ls
```
Result \
besides others there should be the following entries
```shell
REPOSITORY                    TAG         IMAGE ID       CREATED        SIZE
fhooe-web-dock_db             latest      96a53a828586   2 days ago     565MB
fhooe-web-dock_php-apache     latest      5605e6a73edc   2 days ago     561MB
phpmyadmin/phpmyadmin         latest      2e5141bbcbfb   7 months ago   474MB
```

## List Containers
```
docker container ls
```
## List Volumes
```
docker volume ls
```
## Debugging:
```shell
docker exec -it webapp /bin/bash
docker exec -it mariadb /bin/bash
docker exec -it pma /bin/bash
```
## See processes running in Container webshop
`PS path-to-Docker/Docker/fhooe-web-dock> ` 
```shell
docker exec -it webapp /bin/bash
```
`root@<image-id>:/var/www/html# `
```shell
ps -aux
```
Result
```shell
 USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
 root         1  0.0  0.2  78988 26568 ?        Ss   08:30   0:00 apache2 -DFOREGROUND
 www-data    17  0.0  0.0  79256 11952 ?        S    08:30   0:00 apache2 -DFOREGROUND
 www-data    18  0.0  0.0  79256 11952 ?        S    08:30   0:00 apache2 -DFOREGROUND
 www-data    19  0.0  0.0  79256 11952 ?        S    08:30   0:00 apache2 -DFOREGROUND
 www-data    20  0.0  0.0  79256 11952 ?        S    08:30   0:00 apache2 -DFOREGROUND
 www-data    21  0.0  0.0  79256 11952 ?        S    08:30   0:00 apache2 -DFOREGROUND
 root        22  0.1  0.0   4092  3336 pts/0    Ss   08:40   0:00 /bin/bash
 root        29  0.0  0.0   6928  2908 pts/0    R+   08:40   0:00 ps -aux
```
`root@<image-id>:/var/www/html# `
```shell
ps -ef
```
Result
```shell
 UID        PID  PPID  C STIME TTY          TIME CMD
 root         1     0  0 08:30 ?        00:00:00 apache2 -DFOREGROUND
 www-data    17     1  0 08:30 ?        00:00:00 apache2 -DFOREGROUND
 www-data    18     1  0 08:30 ?        00:00:00 apache2 -DFOREGROUND
 www-data    19     1  0 08:30 ?        00:00:00 apache2 -DFOREGROUND
 www-data    20     1  0 08:30 ?        00:00:00 apache2 -DFOREGROUND
 www-data    21     1  0 08:30 ?        00:00:00 apache2 -DFOREGROUND
 root        22     0  0 08:40 pts/0    00:00:00 /bin/bash
 root        30    22  0 08:40 pts/0    00:00:00 ps -ef
```
`PS path-to-Docker/Docker/fhooe-web-dock> `
```shell
docker exec -it webapp /bin/bash -c "ps -aux"
```

# pushing pre-built images to registry-1.docker.io

## building and tagging
```shell
docker build -f Dockerfile-mariadb -t mhteaching/fhooe-web-dock:mariadb .
```
```shell
docker build -f Dockerfile-php -t mhteaching/fhooe-web-dock:php .
```
```shell
docker build -f Dockerfile-phpmyadmin -t mhteaching/fhooe-web-dock:phpmyadmin .
```
## pushing
```shell
docker push mhteaching/fhooe-web-dock:mariadb
```
```shell
docker push mhteaching/fhooe-web-dock:php
```
```shell
docker push mhteaching/fhooe-web-dock:phpmyadmin
```
## pulling
```shell
docker compose up -f docker-compose.tagged.yml -d
```

## Troublshooting Docker Desktop and Containers

See the [Wiki](https://github.com/Digital-Media/fhooe-web-dock/wiki)