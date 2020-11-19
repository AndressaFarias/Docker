# Docker-Compose

Passoas realizados conforme tutorial do video #2 - Docker, Swarm e Compose | ContainerWeek, Canal Linux Tips - https://www.youtube.com/watch?v=T3lpAjOcbU8&t=3629s

## Comandos realizados

### Docke

`docker container ls` -- listagem dos containers que estão em execução

`docker container run hello-world` -- execução de uma imagem teste

`docker conatiner ls -a` -- lista tos os containersmesmo os parados

`docker container run -ti debian` -- desta forma o container é criado e temos interatividade com ele, porem ao sair o container morre

saindo do container usando o `ctrl + p + q` o container continua no ar

`docker container attach <ID-Conatiner>` -- acesar um container, de modo que se conecta diretamente no processo

`docker container exec -ti <ID> bash` - acessa o container 

`docker container run -d -p 8080:80 nginx`

`docker image ls`

`docker container rm -f <ID>`-- remove inclusive os container em execução

### Docker-compose