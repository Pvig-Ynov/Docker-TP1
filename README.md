# Docker-TP1
## Exécuter un serveur web (apache, nginx, ...) dans un conteneur docker
### Récupérer l’image sur le Docker Hub

```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
025c56f98b67: Pull complete 
ec0f5d052824: Pull complete 
cc9fb8360807: Pull complete 
defc9ba04d7c: Pull complete 
885556963dad: Pull complete 
f12443e5c9f7: Pull complete 
Digest: sha256:75263be7e5846fc69cb6c42553ff9c93d653d769b94917dbda71d42d3f3c00d3
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

### Vérifier que cette image est présente en local

```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker image list
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    3964ce7b8458   32 hours ago   142MB
```

### Créer un fichier index.html simple

```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ touch index.html
pierre@pierre-pc:~/VSCODE/Docker-TP1$ nano index.html
```


### Démarrer un conteneur et servir la page html créée précédemment à l’aide d’un volume (option -v de docker run)


```
docker run --name some-nginx -v /home/pierre/pierre/VSCODE/Docker-TP1/html:/usr/share/nginx/html:ro -p 80:80 -d nginx
97d7432a5e2ec8b880ba50a831caf4be1e854d712e1fbd56b5fffec4c24dc612
```

### Supprimer le conteneur précédent et arriver au même résultat que précédemment à l’aide de la commande docker cp


```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker run --name some-nginx -v /opt/nginx:/usr/share/nginx/html -d nginx
docker: Error response from daemon: Conflict. The container name "/some-nginx" is already in use by container "92017dc379278d1ff679adde70ee399b01c3b8a3530208f48d3a2e41a9d9c3b4". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker kill some-nginx 
some-nginx
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker rm some-nginx 
some-nginx
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker run --name some-nginx -v /opt/nginx:/usr/share/nginx/html -d nginx
57daa6a37d69323d94d5b4d97c0a0b1d90e133a6a0b075b9478fef48b8c0fde1
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker cp /home/pierre/VSCODE/Docker-TP1/html/index.html some-nginx:/usr/share/nginx/html
pierre@pierre-pc:~/VSCODE/Docker-TP1$ 

```
## Builder une image
### A l’aide d’un Dockerfile, créer une image (commande docker build)
```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker build .
[+] Building 0.2s (7/7) FINISHED                                                                                                       
 => [internal] load build definition from Dockerfile                                                                              0.0s
 => => transferring dockerfile: 97B                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                 0.1s
 => => transferring context: 2B                                                                                                   0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                                   0.0s
 => [internal] load build context                                                                                                 0.0s
 => => transferring context: 965B                                                                                                 0.0s
 => [1/2] FROM docker.io/library/nginx                                                                                            0.0s
 => CACHED [2/2] COPY /html/index.html /usr/share/nginx/html                                                                      0.0s
 => exporting to image                                                                                                            0.0s
 => => exporting layers                                                                                                           0.0s
 => => writing image sha256:9e17f0cd598b0fb30fe89640936d14b343abf8fce9408fefc98398c040fe917f                                      0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
```

### Exécuter cette nouvelle image de manière à servir la page html (commande docker run)

```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker run --name nginx-pvig -p 80:80 -d 9e17f0cd598b
4b443d69523a4a1ff3a333d66116906d48c290e728542b2484d331694f594eab
```

## Utiliser une base de données dans un conteneur docker

### Récupérer les images mysql:5.7 et phpmyadmin/phpmyadmin depuis le Docker Hub

```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker pull mysql:5.7
5.7: Pulling from library/mysql
d26998a7c52d: Pull complete 
4a9d8a3567e3: Pull complete 
bfee1f0f349e: Pull complete 
71ff8dfb9b12: Pull complete 
bf56cbebc916: Pull complete 
2e747e5e37d7: Pull complete 
711a06e512da: Pull complete 
3288d68e4e9e: Pull complete 
49271f2d6d15: Pull complete 
f782f6cac69c: Pull complete 
701dea355691: Pull complete 
Digest: sha256:6306f106a056e24b3a2582a59a4c84cd199907f826eff27df36406f227cd9a7d
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker pull phpmyadmin/phpmyadmin
Using default tag: latest
latest: Pulling from phpmyadmin/phpmyadmin
214ca5fb9032: Pull complete 
cd813a1b2cb8: Pull complete 
63cf7574573d: Pull complete 
54c27146d16e: Pull complete 
078f4450f949: Pull complete 
5f145e355bc4: Pull complete 
fdc797cb9eea: Pull complete 
af45e7153a31: Pull complete 
b546fbaf263b: Pull complete 
16dd2cabbcd2: Pull complete 
30a426b49280: Pull complete 
c94e73d5f13e: Pull complete 
2f5a3464a278: Pull complete 
a4f9f723c297: Pull complete 
5b04d16a8086: Pull complete 
2a3d1fa22772: Pull complete 
ef56affc4552: Pull complete 
9b9b44731108: Pull complete 
Digest: sha256:ae6dadd9cf3c158e42937788f7255fa820ea3daef0349226d8d43f32e76535e1
Status: Downloaded newer image for phpmyad
```

```
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=Motdepassetùrèssecret -d mysql:5.7
c3d798d5c56be46dddc8cfdc18840445becf165e84f4c5a771502c1b965e0658
pierre@pierre-pc:~/VSCODE/Docker-TP1$ docker run --name php-myadmin --link mysql:db -p 8080:80 -d phpmyadmin/phpmyadmin
02f79e9c76804571fcf2261423c78001a46be135bd00ee427100c2c75a840e0b
```

## Faire la même chose que précédemment en utilisant un fichier docker-compose.yml


cf fichier docker-compose.yml

### A l’aide de docker-compose et de l’image praqma/network-multitool disponible sur le Docker Hub créer 3 services (web, app et db) et 2 réseaux (frontend et backend). es services web et db ne devront pas pouvoir effectuer de ping de l’un vers l’autre

cf ntier-segmented/docker-compose.yml