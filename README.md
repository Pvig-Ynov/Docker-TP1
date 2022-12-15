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


### 


```
docker run --name some-nginx -v /home/pierre/html:/usr/share/nginx/html:ro -d nginx
97d7432a5e2ec8b880ba50a831caf4be1e854d712e1fbd56b5fffec4c24dc612
```