# Comandos Docker


## **FLAGS**
| Flag |             | Descrição                                                 |
|------|-------------|---------------------------------------------------------- |
| -d   | detach      | Executa o container em background                         |
| -P   | publish-all | Atribui portas aleatória para o mapeamento host:container |
|-it   |             | Faz om que o terminal seja iterativo.                     |


## **Comandos relacionados à construção de Dockerfile**
| Comando                                                                                       | Descrição                                                                        |
| --------------------------------------------------------------------------------------------- |--------------------------------------------------------------------------------- |
| docker build -f Dockerfile - .                                                                | Cria uma imagem a partir de um Dockerfile                                        |
| docker build -f CAMINHO_DOCKERFILE/Dockerfile -t NOME_USUARIO/NOME_IMAGEM  CAMINHO_DOCKERFILE | Constrói e nomeia uma imagem não-oficial informando o caminho para o Dockerfile. |
| docker login                                                                                  | Inicia o processo de login no Docker Hub.                                        | 
| docker push NOME_USUARIO/NOME_IMAGEM                                                          | Envia a imagem criada para o Docker Hub.                                         |


## **Comandos relacionados às informações**
| Comando                      | Descrição                                                                   |
| ---------------------------- |---------------------------------------------------------------------------- |
| docker version               | Exibe a versão do docker que está instalada.                                |
| docker inspect ID_CONTAINER  | Retorna diversas informações sobre o container.                             |
| docker ps                    | Exibe todos os containers em execução no momento.                           |
| docker ps -a                 | Exibe todos os containers, independentemente de estarem em execução ou não. |


## **Comandos relacionados à execução**
| Comando                                                                 | Descrição                                                                         
| ------------------------------------------------------------------------|--------------------------------------------------------------------------------- |
| docker run NOME_DA_IMAGEM                                               | Cria um container com a respectiva imagem passada como parâmetro.                |
| docker run -it NOME_DA_IMAGEM                                           | Conecta o terminal que estamos utilizando com o do container.                    |
| docker run -d -P --name NOME dockersamples/static-site                  | Dá um nome ao container.                                                         |
| docker run -d -p 12345:80 dockersamples/static-site                     | Define uma porta específica para ser atribuída à porta 80 do container, neste caso 12345. |
| docker run -v "CAMINHO_VOLUME" NOME_DA_IMAGEM                           | Cria um volume no respectivo caminho do container.                               |
| docker run -it --name NOME_CONTAINER --network NOME_DA_REDE NOME_IMAGEM | Cria um container especificando seu nome e qual rede deverá ser usada            | 

    
## **Comandos relacionados à inicialização/interrupção**
| Comando                         | Descrição                                                                                            |
| ------------------------------- |----------------------------------------------------------------------------------------------------- |
| docker start ID_CONTAINER       | Inicia o container com id em questão.                                                                |
| docker start -a -i ID_CONTAINER | Inicia o container com id em questão e integra os terminais, além de permitir interação entre ambos. | 
| docker stop ID_CONTAINER        | Interrompe o container com id em questão.                                                            |


## **Comandos relacionados à remoção**
| Comando                   | Descrição                                     |
| ------------------------- |---------------------------------------------- |
| docker rm ID_CONTAINER    | Remove o container com id em questão.         |
| docker container prune    | Remove todos os containers que estão parados. |
| docker rmi NOME_DA_IMAGEM | Remove a imagem passada como parâmetro.       |


## **Comandos relacionados ao Docker Hub**
| Comando                              | Descrição                                 |
| ------------------------------------ |-------------------------------------------|
| docker login                         | Inicia o processo de login no Docker Hub. | 
| docker push NOME_USUARIO/NOME_IMAGEM | envia a imagem criada para o Docker Hub.  |
| docker pull NOME_USUARIO/NOME_IMAGEM | baixa a imagem desejada do Docker Hub.    |


## **Comandos relacionados à rede**
| Comando                                            | Descrição                                                                             |
| -------------------------------------------------- |---------------------------------------------------------------------------------------|
| hostname -i                                        | Mostra o ip atribuído ao container pelo docker (funciona apenas dentro do container). |
| docker network create --driver bridge NOME_DA_REDE | Cria uma rede especificando o driver desejado.                                        |


## **Comandos dentro do Container**
| Comando                      | Descrição                                                          |
| ---------------------------- |------------------------------------------------------------------- |
| exit                         | digitando no container o comando _exit_ para encerrar o container. | 
| CTRL + D                     | para encerrr o container                                           |
| docker images                | exibe as imagens que temos na nossa máquina                        |
| docker port <ID>             | para verificar s portas mapeadas do container                      |
| docker-machine ip            | descobrir o ip da máquina virtual                                  |
| docker exec -it <Nome|ID> sh | Acessar container e acessar o shell do container                   |


## **Comandos COMPOSE**
| Comando              | Descrição                                              |
| -------------------- | ------------------------------------------------------ |
| docker-compose up -d | Para subir os containers descritos no _docker compose_ | 