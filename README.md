# Docker

[**Kubernetes**](https://www.notion.so/Kubernetes-6a22a6af60814853aa3e38d98041a8da?pvs=21)

[GitHub - codeedu/wsl2-docker-quickstart: Guia rápido do WSL2 + Docker](https://github.com/codeedu/wsl2-docker-quickstart)

[https://www.youtube.com/watch?v=wpdcGgRY5kk](https://www.youtube.com/watch?v=wpdcGgRY5kk)

## Install docker in WSL2:

### Instale os pré-requisitos:

```bash
sudo apt update && sudo apt upgrade
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

### Adicione o repositório do Docker na lista de sources do Ubuntu:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Instale o Docker Engine:

```bash
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Configs:

```bash
#Dê permissão para rodar o Docker com seu usuário corrente:

sudo usermod -aG docker $USER
```

### Inicie o serviço do Docker:

```bash
sudo service docker start

#starting docker with linux
sudo nano /etc/wsl.conf
[boot]
command="service docker start"

```

## Comandos:

```bash
#Cria e executa um container
docker container run nome-da-imagem
docker container run --name "nome-para-o-container" nome-da-image

#Lista os containers rodando-ativos
docker container ls
docker container ls -a #lista todos os containers

#Remover um container
docker container rm nome-do-container

#Modo interativo (acessando o terminal do container)
docker container run -it nome-da-image 

#Instanciar um container com variaveis de ambeinte
#exemplo mysql
docker container run -e MYSQL_ROOT_PASSWORD=senha -e MYSQL_DATABASE=nome -e MYSQL_USER=user -e MYSQL_PASSWORD=senha -d -p "portal-local:porta-container" mysql 
docker container run -e MYSQL_ROOT_PASSWORD=k8888-e MYSQL_DATABASE=aula_docker -e MYSQL_USER=klysman08-e MYSQL_PASSWORD=k8888-d -p 3307:3306 --name aula_docker_mysql mysql 

#Acessando o terminal do container
docker exec -it id-container /bin/bash

#listar as imagens locais
docker image ls
#iniciar a imagem criada 
docker container run -d -p "porta:porta" "nome-da-imagem"
```

## Parametros:

```bash
-e #variaveis 
-d #deamon para não travar o terminal
-it #modo terminal do container
-p #crindo pontes das portas de acesso - "portal-local:porta-container"
```

## Criando imagens:

```bash
#Crie um arquivo chamado Dockerfile sem extensão na raiz do projeto
#exemplo ubuntu
FROM ubuntu #imagem base
RUM apt update && apt install curl -y #comandos para executar no termanal do container quando for criado
WORKDIR /app #Cria um diretorio no container e entra nele
```

```bash
#Crie um arquivo chamado Dockerfile sem extensão
#exemplo node
FROM node:versão ou lastest
WORKDIR /app 
COPY package*.json ./ # Copy package.json and package-lock.json to the working directory
RUN npm install # Install dependencies
COPY . . # Copy all files to the working directory
EXPOSE 8080 # Expose port 8080
CMD ["node", "server.js"] # Run the server.js file
```

```bash
#Crie um aruivo .dockerignore no mesmo diretório para desconsiderar pastas ou files na contrução de imagens
node_modules/
```

```bash
#Cria a imagem com base todos os arquivo no diretorio (.)
#acesse o diretorio onde está o dockerfile
docker build -t "nome-para-imagem" -f Dockerfile .
```

## Docker Hub:

```bash
#Com a imagem criada localmente
docker tag "nome-da-imagem" klysman08/nome-repositorio:versão ou latest

#atribuir versão para latest
docker tag klysman08/conversao-temperatura:v1 klysman08/conversao-temperatura:latest

#faça login
docker login

#push
docker push klysman08/nome-da-image:latest

#iniciando imagem do docker-hub
#exemplo:
docker container run -d -p 8081:8080 klysman08/conversao-temperatura:latest
```

## Portainer:

[https://docs.portainer.io/start/install/server/docker/ws](https://docs.portainer.io/start/install/server/docker/wsl)l 

```bash
docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

#https://docs.portainer.io/advanced/reset-admin
docker stop "id-portainer-container"
docker run --rm -v portainer_data:/data portainer/helper-reset-password > comando para resetar a senha de login

#abrir o portainer em:
https://localhost:9443

2023/06/24 16:24:03 Password successfully updated for user: klysman08
2023/06/24 16:24:03 Use the following password to login: k8888
```

## Home Assistant:

```bash
docker run -d -p 8123:8123 --privileged --volume "C:\Users\klysm\Downloads\homeassistant:/config" --name homeassistant -e "TZ=America/New_York" [ghcr.io/home-assistant/home-assistant:stable](http://ghcr.io/home-assistant/home-assistant:stable)
```

## Wordpress:

```bash
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser 
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

```bash
#abrir o wordpress em:
http://localhost:8080/
```
