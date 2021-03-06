# Docker-compose

``` bash
[node1] (local) root@192.168.0.23 ~
$ mkdir compose-example-1
[node1] (local) root@192.168.0.23 ~
$ cd compose-example-1/
[node1] (local) root@192.168.0.23 ~/compose-example-1
```
### My first compose file 
``` bash
vi docker-compose.yaml
```
### docker-compose.yaml file
``` yaml
version: '3' 
  
# same as 
# docker run -dit --name my-apache-app -p 8080:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:latest

services:
  httpd:
    image: httpd:latest
    volumes:
      - .:/usr/local/apache2/htdocs/
    ports:
      - '8080:80'
```
### Running my first docker compose file
``` bash
$ docker-compose up 
Creating network "test1_default" with the default driver
Pulling httpd (httpd:latest)...latest: Pulling from library/httpdbf5952930446: Pull complete3d3fecf6569b: Pull completeb5fc3125d912: Pull complete679d69c01e90: Pull complete
76291586768e: Pull complete
Digest: sha256:3cbdff4bc16681541885ccf1524a532afa28d2a6578ab7c2d5154a7abc182379
Status: Downloaded newer image for httpd:latest
Creating test1_httpd_1 ... done
Attaching to test1_httpd_1
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | [Mon Aug 24 04:19:32.161521 2020] [mpm_event:notice] [pid 1:tid 140199202600064] AH00489: Apache/2.4.46 (Unix) configured -- resuming normal operations
httpd_1  | [Mon Aug 24 04:19:32.161642 2020] [core:notice] [pid 1:tid 140199202600064] AH00094: Command line: 'httpd -D FOREGROUND'
httpd_1  | 172.18.0.1 - - [24/Aug/2020:04:19:35 +0000] "GET / HTTP/1.1" 200 225
httpd_1  | 172.18.0.1 - - [24/Aug/2020:04:19:36 +0000] "GET /favicon.ico HTTP/1.1" 404 196
httpd_1  | 172.18.0.1 - - [24/Aug/2020:04:19:46 +0000] "GET /docker-compose.yaml HTTP/1.1" 200 251
```
### list the container
``` bash
$ docker-compose ps
    Name            Command        State    Ports
-------------------------------------------------
test1_httpd_1   httpd-foreground   Exit 0       
```
### delete a container
``` bash
$ docker-compose rm
Going to remove compose-example-1_httpd_1
Are you sure? [yN] y
Removing compose-example-1_httpd_1 ... done
```
### mount a local directory that contains custom webapage
``` bash
$ mkdir myweb; cd myweb
vi index.html
```
# paste the following html content 
``` html
<!DOCTYPE html>
<html>
<title>docker-compose with your site </title>
<body>

<h1>Welcome to RK's home page</h1>
<p>Devops is cool</p>

</body>
</html>
```
### come out of myweb directory
```
cd ..
```
### modify the docker-compose.yaml file
```version: '3' 
  
# same as 
# docker run -dit --name my-apache-app -p 8080:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:latest

services:
  httpd:
    image: httpd:latest
    volumes:
      - $PWD/myweb/:/usr/local/apache2/htdocs/
    ports:
      - '8080:80'
```
### Run it in the background
``` bash
$ docker-compose up -d
Creating compose-example-1_httpd_1 ... done
```
### stop the container
``` bash
$ docker-compose stop
Stopping compose-example-1_httpd_1 ... done
```
### start the stoped service
``` bash
$ docker-compose restart
Restarting compose-example-1_httpd_1 ... done
```
### Get the logs
``` bash
$ docker-compose logs
Attaching to compose-example-1_httpd_1
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | [Mon Aug 24 15:42:14.496867 2020] [mpm_event:notice] [pid 1:tid 140490688894080] AH00489: Apache/2.4.46 (Unix) configured -- resuming normal operations
httpd_1  | [Mon Aug 24 15:42:14.497304 2020] [core:notice] [pid 1:tid 140490688894080] AH00094: Command line: 'httpd -D FOREGROUND'
httpd_1  | 172.18.0.1 - - [24/Aug/2020:15:44:51 +0000] "GET / HTTP/1.1" 304 -
httpd_1  | [Mon Aug 24 15:45:55.943507 2020] [mpm_event:notice] [pid 1:tid 140490688894080] AH00492: caught SIGWINCH, shutting down gracefully
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.2. Set the 'ServerName' directive globally to suppress this message
httpd_1  | [Mon Aug 24 15:47:40.992874 2020] [mpm_event:notice] [pid 1:tid 139815783609472] AH00489: Apache/2.4.46 (Unix) configured -- resuming normal operations
httpd_1  | [Mon Aug 24 15:47:40.993045 2020] [core:notice] [pid 1:tid 139815783609472] AH00094: Command line: 'httpd -D FOREGROUND'
httpd_1  | 172.18.0.1 - - [24/Aug/2020:15:51:14 +0000] "GET / HTTP/1.1" 304 -
```
[Back to Main page](https://github.com/blrk/learn-docker.io/wiki)



