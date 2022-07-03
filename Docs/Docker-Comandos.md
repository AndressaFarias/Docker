# Comandos Docker

## Comandos Iniciais 
| COMANDO           | DESCRIÇÃO |
| ---------------   | -----------------------------------------     |
| docker version    | Exibe a versão do docker que está instalada.  |


## FLAGS
| Flag |  Name       | Descrição                                                 |
|------|-------------|---------------------------------------------------------- |
| -d   | detach      | Executa o container em background                         |
| -P   | publish-all | Atribui portas aleatória para o mapeamento host:container |
| -p   |             | Faz o mapeaento para uma porta especifica                 |
|-it   |             | Faz com que o terminal seja iterativo.                    |
| -t=time   | | tempor para que o comando seja executado |
| -s | size | informa o tamanho da imagem |



## CRIAR UM CONTAINER
| COMANDO                    | DESCRIÇÃO                      |
| ---------------------------|--------------------------------|
| docker run <nomeImage>     | (Sintaxe antiga) Cria um container com a respectiva imagem passada como parâmetro. |
| docker run -it <nomeImage> <cmd> | (Sintaxe antiga) Cria um container com a imagem passada por parâmetro e conecta o terminal que estamos utilizando ao terminal do container.|
| docker container run -ti <nomeImage>  | (Sintaxe nova) Verifca se há containers em execução          |
| docker run --name NOME dockersamples/static-site | Dá um nome ao container. |
| docker run -it --name NOME_CONTAINER --network NOME_DA_REDE NOME_IMAGEM | Cria um container especificando seu nome e qual rede deverá ser usada |  

**docker run** : verifica se a imagem já existe, se não exister faz o pull da imagem a partir do Docker Hub



## **INICIAR/INTERROMPER UM  CONTAINER**
| Comando                         | Descrição                                                                                            |
| ------------------------------- |----------------------------------------------------------------------------------------------------- |
| docker stop <ID_CONTAINER>        | Interrompe o container com id em questão.                                                            |
| docker start <ID_CONTAINER>       | Inicia o container com id em questão.                                                                |
| docker pause <ID_CONTAINER>       | Pausa um container  - o fluxo de execução é mantido|
| docker unpause <ID_CONTAINER>     | Despause um container - o fluxo de execução é mantido| 


## ACESSAR um CONTAINER
| Comando                         | Descrição                                                                                            |
| ------------------------------- |----------------------------------------------------------------------------------------------------- |
| docker exec -it <Nome|ID> sh    | Acessar container e acessar o shell do container                                                        |



## VERIFICANDO containers EM EXECUÇÃO
| COMANDO                | DESCRIÇÃO |
| ---------------        | -----------------------------------------     |
| docker container ls    | (Sintaxe nova) Verifca se há containers em execução          |
| docker container ls -a | (Sintaxe nova) Exibe todos os containers, independentemente de estarem em execução ou não. |
| docker ps              | (Sintaxe antiga) Exibe todos os containers em execução no momento.  | 
| docker ps -a           | (Sintaxe antiga) Exibe todos os containers, independentemente de estarem em execução ou não. |



## VERIFICANDO infos do CONTAINER
| COMANDO                | DESCRIÇÃO |
| ---------------        | -----------------------------------------         |
| docker port <ID>       | para verificar as portas mapeadas do container    |
| docker inspect <id-container> 



## **REMOVER CONTAINER**
| Comando                   | Descrição                                     |
| ------------------------- |---------------------------------------------- |
| docker rm ID_CONTAINER    | Remove o container com id em questão.         |
| docker container prune    | Remove todos os containers que estão parados. |
| docker container rm $(docker container ls -aq)  | remove todos os containers |



## PERSISTINDO DADOS
| Comando | Descrição | 
|  docker run -it -v <host:container> <imagem> | Cria um volume no respectivo caminho do container. |
|  docker run -it --mount type=bind,source=/home/andressa/estudos/Docker/Projetos/Dockerfile/volume-docker,target=/app ubuntu bash |  cria um bind mount para o container , liga uma pasta do host a uma pasta do container, dependem da estrutura de pastas do host|
| docker volume ls  | verifica quais os volumes existem | 
| docker volume create meu-volume | cria um volume que é totalmnete gerenciado pelo docker |
| docker run -it -v meu-volume:/app ubuntu bash |
| docker run -it --mount source=meu-volume,target=/app ubuntu bash | cria um contaner usando o volume criado, se for indicado um volme que não esteja previamente criado, o volume é criado |
| docker run -it --tmpfs=/app ubuntu bash| é escrita na memória do host, é uma pasta temporária, quando o container parar de funcionar os dados serão perdidos - esse tipo de armazenamento armazena os dados em memória, mas não são escritos na camada de RW, pode ser util para arquivos sensiveis - só funciona no Linux  |
| docker run -it --munt type=tmpfs, destination=/app ubuntu bash | possui o mesmo comportamento do comando acima, Tmpfs armazenam dados em memória volátil|



## REDE
| Comando | Descrição |
| docker networkl ls | retorna todas as redes docker que existem |
| docker network create --driver <bridgre|host|...> <name> | Cria uma rede ex: `docker network --driver bridge minha-bridge`


| hostname -i                                        | Mostra o ip atribuído ao container pelo docker (funciona apenas dentro do container). |
| docker network create --driver bridge NOME_DA_REDE | Cria uma rede especificando o driver desejado.




## VERIFICAR IMAGEM
| COMANDO                       | DESCRIÇÃO |
| ----------------------------  | --------------------------------------------- |
| docker inspect <id-image>     | Retorna diversas informações sobre a imagem.  |
| docker history <id_image>     | Retorna as camdas que compõe a imagem         |

## CRIAR IMAGEM
| COMANDO | DRESCRIÇÃO |
| docker build -t <userDockerHub/NomeImage:versao> <localDockerfile>| faz o build da imagem descrita no Dockerfile |
| docker push <imagem> |
| docker tag <nomeAtual> <novoNome> | ALtera o nome da imagem  porém manter o Id da imagem | 


## REMOVER IMAGEM
| COMMANDO | DESCRIÇÃO |
| docker rmi NOME_DA_IMAGEM | Remove a imagem passada como parâmetro.       |
| docker rmi $(docker image ls -aq) <--force> | remove todas as imagens |




---


## **Comandos relacionados às informações**

| docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.Names}}" | Para consultar e formatar a aída das informações | 

## **Comandos relacionados à construção de Dockerfile**
| Comando                                                                                       | Descrição                                                                        |
| --------------------------------------------------------------------------------------------- |--------------------------------------------------------------------------------- |
| docker build -f Dockerfile - .                                                                | Cria uma imagem a partir de um Dockerfile                                        |
| docker build -f CAMINHO_DOCKERFILE/Dockerfile -t NOME_USUARIO/NOME_IMAGEM  CAMINHO_DOCKERFILE | Constrói e nomeia uma imagem não-oficial informando o caminho para o Dockerfile. |
| docker login                                                                                  | Inicia o processo de login no Docker Hub.                                        | 
| docker push NOME_USUARIO/NOME_IMAGEM                                                          | Envia a imagem criada para o Docker Hub.                                         |






## **Comandos relacionados ao Docker Hub**
| Comando                              | Descrição                                 |
| ------------------------------------ |-------------------------------------------|
| docker login                         | Inicia o processo de login no Docker Hub. | 
| docker push NOME_USUARIO/NOME_IMAGEM | envia a imagem criada para o Docker Hub.  |
| docker pull NOME_USUARIO/NOME_IMAGEM | baixa a imagem desejada do Docker Hub.    |


                                        |


## **Comandos dentro do Container**
| Comando                      | Descrição                                                          |
| ---------------------------- |------------------------------------------------------------------- |
| exit                         | digitando no container o comando _exit_ para encerrar o container. | 
| CTRL + D                     | para encerrr o container                                           |
| docker images                | exibe as imagens que temos na nossa máquina                        |
| docker-machine ip            | descobrir o ip da máquina virtual                                  |


## **Comandos COMPOSE**
| Comando              | Descrição                                              |
| -------------------- | ------------------------------------------------------ |
| docker-compose up -d | Para subir os containers descritos no _docker compose_ | 