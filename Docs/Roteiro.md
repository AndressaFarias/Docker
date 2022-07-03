# Módulo = 02 Os primeiros comandos
## Aula = 07 Mapeamento portas
```
docker run -d -P dockersamples/static-site
docker ps 

verificar a porta que está mapeado  container 

docker port <container-id>

```

# Módul = 0 Criando e compreendendod imagens
## Aula = 05. Intruções Dockerfile

```
Baixar zip :: https://github.com/danielartine/alura-docker/blob/aula-3/app-exemplo.zip?raw=true
```

descompactar unzip <nomeArquivo>


crie umDockerfil com as seguintes instruções

```
FROM node:14
WORKDIR /app-node
COPY . .
RUN npm install
ENTRYPOINT npm start
```

`docker build -t <nome-user-docker0hub>/app-node:1.0`

`docker images`