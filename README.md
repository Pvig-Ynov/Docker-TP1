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
docker run --name some-nginx -v /home/pierre/pierre/VSCODE/Docker-TP1/html:/usr/share/nginx/html:ro -d nginx
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