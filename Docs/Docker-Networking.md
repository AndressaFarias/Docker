
 # COMUNICAÇÃO ENTRE CONTAINERS
\-------------------------------

# NETWOKING DOCKER

Normalmente uma aplicação é composta por diversas partes. Quando estamos trabalhando com containers, é comum separarmos cada uma dessas partes em um container específico, para que cada container tenha apenas uma única responsabilidade.

## **Redes com Docker**
Por padrão, já existe uma default network. 

Isso significa que, quando criamos os nossos containers, por padrão eles funcionam na mesma rede.

redes padrão criados 
* none : quando um container é criado nessa rede é indicado que esse container não terá nenhuma interface de rede. Ele ficará isolado.

* bridge :

* host : quando um container é criado nessa rede, é retirado todo o isolamento que existe entre a interface de rede do host e do container. utilizando o driver host usamos a mesma interface do host no container.

## **Para verificar**

1. Vamos subir um container com Ubuntu:

    `docker run -it ubuntu`

2. Abrir outro terminal e verificar o id desse container

    `docker ps`

3. Vamos inspecionar o container

    `docker inspect <ID>`

4.  Na saída desse comando, em _NetworkSettings_, vemos que o container está na rede padrão _bridge_, rede em que ficam todos os containers que criamos.

5. Voltando ao terminal do container, e executamos o comando 

    `hostname -i`    vemos o IP atribuído a ele pela rede local do Docker

    Dentro dessa rede local, os containers podem se comunicar através desses IPs. 

6. Para comprovar isso, vamos deixar esse container rodando e criar um novo:
    `docker run -it ubuntu`

7. Vamos verificar o seu IP:

    `hostname -i`

8. Voltamos ao primeiro container e instalamos o pacote `iputils-ping` para podermos executar o comando ping 

    `apt-get update && apt-get install iputils-ping`

9. Executamos o comando ping, passando para ele o IP do segundo container
   Assim, podemos ver que os containers estão conseguindo se comunicar entre eles.

## **Comunicação entre containers utilizando os seus nomes**

O Docker cria uma rede virtual _default_, em que todos os containers fazem parte dela, com os IPs automaticamente atribuídos. 

Como a atribuição de IPs é feita pelo Docker cada vez que subimos uma container ele irá receber um IP novo/diferente. Logo não saberemos qual o IP que será atribuido. 

Quando queremos fazer a comunicação entre os containers isso não é muito útil. Pois podemos colocar na aplicação a referencia exata para nosso banco de dados, para isso é preciso configurar um nome para o container.

Nomear um container nós já sabemos, basta adicionar a flag --name, passando o nome que queremos na hora da criação do container.

Apesar de conseguirmos dar um nome a um container, a rede _default_ do Docker não permite com que atribuamos um hostname a um container.

Na rede padrão do Docker, só podemos realizar a comunicação utilizando IPs, mas se criarmos a nossa própria rede, podemos "batizar" os nossos containers, e realizar a comunicação entre eles utilizando os seus nomes.

## **Criando a nossa própria rede do Docker**

Vamos criar a nossa própria rede:
* Para esse comando também precisamos dizer qual driver vamos utilizar. Para o padrão de ter uma nuvem e os containers compartilhando a rede, devemos utilizar o driver de bridge.
    * Especificamos o driver através da flag --driver 

* Dizemos o nome da rede. 

_Um exemplo do comando:_

    docker network create --driver bridge minha-rede

Agora, quando criamos um container, ao invés de deixarmos ele ser associado à rede padrão do Docker, atrelamos à rede que acabamos de criar, através da flag --network. 

Vamos aproveitar e nomear o container:

    docker run -it --name meu-container-de-ubuntu --network minha-rede ubuntu

Se executarmos o comando docker inspect meu-container-de-ubuntu, podemos ver em NetworkSettings o container está na rede minha-rede. 

Para testar a comunicação entre os containers na nossa rede, vamos abrir outro terminal e criar um segundo container:

    docker run -it --name segundo-ubuntu --network minha-rede ubuntu

No segundo-ubuntu, instalamos o ping e testamos a comunicação com o meu-container-de-ubuntu:

    ping meu-container-de-ubuntu

Conseguimos realizar a comunicação entre os containers utilizando somente os seus nomes. 

É como se o Docker Host, o ambiente que está rodando os containers, criasse uma rede local chamada minha-rede, e o nome do container será utilizado como se fosse um hostname.

**Mas lembrando que só conseguimos fazer isso em redes próprias, redes que criamos, isso não é possível na rede padrão dos containers.**

# **Pegando dados de um banco**

Para praticar o que vimos sobre redes no Docker, vamos criar uma pequena aplicação que se conectará ao banco de dados.

O que vamos fazer é utilizar a aplicação alura-books, que irá pegar os dados de um banco de dados de livros e exibi-los em uma página web. É uma aplicação feita em Node.js e o banco de dados é o MongoDB.

## **Pegando dados de um banco em um outro container**

Primeiramente vamos baixar as duas imagens, a imagem douglasq/alura-books na versão cap05 e a imagem mongo:

    docker pull douglasq/alura-books:cap05
    docker pull mongo

### A imagem douglasq/alura-books

Possui o arquivo server.js, que carrega algumas dependências e módulos que são instalados no momento em que rodamos a imagem. Esse arquivo carrega também as configurações do banco, que diz onde o banco de dados estará em execução, no caso o seu host será meu-mongo, e o database, com nome de alura-books. 

Então, quando formos rodar o container de MongoDB, seu nome deverá ser meu-mongo. 

Além disso, o arquivo realiza a conexão com o banco, configura a _porta_ que será utilizada (3000) e levanta o servidor.

Por fim, temos as rotas, que são duas: 
* a rota /, que carrega os livros e os exibe na página, 
* e a rota /seed, que salva os livros no banco de dados.

Visto isso, já podemos subir a imagem:

    docker run -d -p 8080:3000 douglasq/alura-books:cap05

Ao acessar a página http://localhost:8080/, nenhum livro nos é exibido, pois além de não termos levantado o banco de dados, nós não salvamos nenhum dado nele. Então, vamos excluir esse container e subir o container do MongoDB, lembrando que o seu nome deve ser meu-mongo, e vamos colocá-lo na rede que criamos no vídeo anterior:

    docker run -d --name meu-mongo --network minha-rede mongo

Com o banco de dados rodando, podemos subir a aplicação do mesmo jeito que fizemos  anteriormente, mas não podemos nos esquecer que ele deve estar na mesma rede do banco de dados, logo vamos configurar isso também:

    docker run --network minha-rede -d -p 8080:3000 douglasq/alura-books:cap05

Agora, acessamos a página http://localhost:8080/seed/ para salvar os livros no banco de dados. 

Após isso, acessamos a página http://localhost:8080/ e vemos os dados livros são extraídos do banco e são exibidos na página. 

Para provar isso, podemos parar a execução do meu-mongo e atualizar a página, veremos que nenhum livro mais será exibido.

# **DOCKER PULL**

Faz o download de uma imagem, sintaxe

    docker pull NOME_USUARIO/NOME_IMAGEM

Uma imagem poder ter uma _tag_ que serve para pegar uma determinada versão dessa imagem, sintaxe

    docker pull NOME_USUARIO/NOME_IMAGEM:TAG

Isso baixará a verão _TAG_ da imagem _NOME\_IMAGEM_ do uuário _NOME\_USUARIO_



