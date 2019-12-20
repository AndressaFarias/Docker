# DOCKER

## Introdução ao Docker

**ANTIGAMENTE**

Várias aplicações , vários servidores

Era comum que cada aplicação necessáriapara possibilidar a execução de nossa aplicação ficasse em um servidor físico diferente. 

Para que essas aplicações posam ser instaladas é preciso antes, instalar em cada um dos servidores o respectivo SO (Sistema Operacional).

Também é preciso que haja a comunicação entre esse servidores, o que demanda a criaão de uma rede.

Pensando também que para o funcionamento dos servidores é precio energia elétrica, quanto mais serviores físicos mais gasto com energia.

O processo de provisionamento era lento, pois cadanova aplicação era necessário comprar / montar o servidor físico, instalar o SO e configurá-lo e subir a aplicação.

**Capacidade Pouco Aproveitada**
Esse modelo de arquitetura tornava comum termo servidores parrudos, com uma única aplicação sndo executada, para normalmente ficarem funcionando abaixo da sua capacidade, para que quando fosse necessário, o servidor suportar uma grande quantidade de acessos. 

Isso resultava em muita capacidade ociosa nos servidores, com muitos recursos desperdiçados.


**VIRTUALIZAÇÃO**
Após o advento da tecnologia chamada de _Hypervisor_, foam possiveis as máquinas virtuais. 

As máquinas virtuais tinham o intuito de reduzir o alto tempo e custo de executar aplicações em servidores físicos.

![Estrutura Máquin Virtual](https://i2.wp.com/opensourceforu.com/wp-content/uploads/2016/02/Figure-2.png)

O _Hypervisor_ funciona em cima do sistema operacional, permitindo a virtualização dos recursos físicos do nosso sistema. Assim, criamos uma máquina virtual que tem acesso a uma parte do nosso HD, memória RAM e CPU, criando um computador dentro de outro:

![VM](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/maquina-virtual.png)

Se temos uma VM acessando apenas uma parte do nosso hardware é possivel colocar dentro dela uma das nossas aplicações. 

Desta forma, reduzimos a quantidade de servidores e consequentemente os custos com luz e rede. Além disso, dividimos melhor o nosso hardware, aproveitando melhor os seus recursos e diminuindo a ociosidade.

**Problemas das Máquias Virtuais**

O uso de máquina virtuais resolve os problemas alto indice de ociosidade nos servidores e redução n quantidade de servidores necessários para disponibilização do noo ambiente. Porém as VMs também possuem problemas. Para podermos hospedar a nossa aplicação em uma máquina virtual, também precisamos instalar um sistema operacional nela:

![VMs com Aplicações](https://www.researchgate.net/profile/Umar_Farooq_Minhas/publication/242077512/figure/fig2/AS:282710602993666@1444414868359/Hosted-Virtual-Machine-Architecture.png)

Cada aplicação necessita de um SO para poder ser executada e esses sistemas podem ser diferentes do SO que está instaldo no servidor físico. 

Os SOs possuem custo de memória RAM, disco e processamento. Ou seja, precisamos de uma capacidade mínima para manter as funcionalidades de um SO, aumentando o seu custo de manutenção a cada sistema que tivermos.

Além disso, há um custo de configuração, isto é, liberar portas, instalar alguma biblioteca específica, etc.

**CONTAINER**

Um container funcionará junto do nosso SO base e conterá a nossa aplicação. Criamos um container para cada aplicação, e esses containers vão dividir as funcionalidades do sistema operacional:

![Docker Contaner Architecture](https://www.researchgate.net/profile/Babak_Bashari_Rad/publication/318816158/figure/fig3/AS:522470562201600@1501578097383/Docker-Container-architecture-11.png)

Não temos mais um sistema operacional para cada aplicação, já que agora as aplicações estão dividindo o mesmo sistema operacional. 

**Vantagens de um container**

O Contaier é muito mais leve que a VM por compartilhar o SO do host. Por ser mais leve o container é mais rápido de inicializar. 

**O QUE É O DOCKER?**

*Docker, Inc.*
A Docker, Inc. no início era chamada de _dotCloud_, ela era uma empresa de PaaS (**P**latform **a**s **a** **Se**rvice). Inicialmente, para prover a parte de infraestrutura, a _dotCloud_ utilizava o Amazon Web Services (AWS), serviço que nos disponibiliza máquinas virtuais e físicas para trabalharmo, a _dotCloud_ introduziu o conceito de containers na hora de subir uma aplicação, dando origem ao Docker.

**As tecnologias do Docker**
O Docker é uma coleção de tecnologias para facilitar o deploy e a execução das nossas aplicações. A sua principal tecnologia é a Docker Engine, a plataforma que segura os containers, fazendo o intermédio entre o sistema operacional.

Outras tecnologias do Docker que facilitam a nossa vida:
- **Docker Compose**: é um jeito fácil de definir e orquestrar múltiplos containers;
- **Docker Swarm**: é uma ferramenta para colocar múltiplos docker engines para trabalharem juntos em um cluster;
- **Docker Hub**: é um repositório com mais de 250 mil imagens diferentes para os nossos containers;
- **Docker Machine**: é uma ferramenta que nos permite gerenciar o Docker em um host virtual.

**INSTALANDO O DOCKER**

**Docker For Windows**

Pre-Requisitos:
- Arquietura 64 bits;
- Versão Pro, Enterprise ou Education;
- Virtuaização habilitada;

Instalação:
**WINDDOWS**
- Acesso à página do Docker : https://www.docker.com/products/docker-desktop

**UBUNTU**
* Desitalar versões anteriores: 

    `sudo apt-get remove docker docker-engine docker.io containerd runc`

* Instale usando o repositório

    `sudo apt-get update`

    * Instale pacotes para permitir que o apt use um repositório sobre HTTPS:
    
    
    sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

    ``` 

**HELLO WORLD**

Com o Docker instalado no nosso sistema operacional, podemos testá-lo para ver o seu funcionamento.

Se o nosso Docker foi instalado pelo Docker for Mac ou Docker for Windows, conseguimos executar os seus comandos através do terminal nativo do Mac ou do Windows (Prompt de Comando). Mas se o nosso Docker foi instalado pelo Docker Toolbox, devemos executar os seus comandos através do Docker Quickstart Terminal, terminal que foi instalado pelo próprio Docker Toolbox.

Então, vamos abrir um terminal que consiga se comunicar com o nosso Docker, e executar o seguinte comando para verificar a sua versão:

`docker version` 

Também podemos executar o clássico Hello World:

    docker run hello-world

Baixada a imagem, ela é executada.

Na mensagem retornada detalha o que foi feito para a execução da imagem. 

Nosso _Docker local_ entra em contato com a _Docker Engine_, que por sua vez baixa a imagem _hello-world_ do _Docker Hub_, cria um container com ela e a executa. Após isso, a saída é impressa para nós e a imagem é encerrada.

\-------------------------------
 ## Trabalhando com imagens
\-------------------------------
**COMANDOS BÁSICOS COM CONTAINERS**

| Flag |  Descrição | 
| ----------- | ----------- |
| docker run <nomeDaImgem> | O Docker verifica e temo  imagems indicada, e caso não tenha o Docker buc a imagem do _Docker HUB/Docker Store_|
| docker ps | Retorna o(s) container(s) em execução |
| docker ps -a| Retorna o(s) container(s) em execução **e** os que estão parados se houver.  |


**Imagem**

A imagem é como se fosse uma receita de bolo, uma série de instruções que o Docker seguirá para criar um container. Criado o container, o Docker executa-o.

Quando efetuamos o ownload de uma imagem é possivel verificar que ela é composta por várias camadas (_layers_)

**Container**
- Quando a imagem não poui nenhu comando a ser executado- e utiliamos apenas o comando docker  run <imagem> - o container fica parado ap´so a finlizaçõ de sua criação.


**Exercicio**
Executar os comandos:
    
    docker run ubuntu echo "Ola Mundo"

Com isso, o Docker irá executar um container com Ubuntu, executar o comando echo "Ola Mundo" dentro dele e nos retornar a saída.

**Trabalhando dentro de um Container**

Podemos fazer com que o terminal da nossa máquina seja integrado ao terminal de dentro do container, para ficar um terminal interativo. Podemos fazer isso adicionando a flag -it ao comando, atrelando assim o terminal que estamos utilizando ao terminal do container:

    docker run -it ubuntu

Assim que executamos o comando, podemos perceber que o terminal muda.

Com isso, estamos trabalhando dentro do container. E dentro dele, podemos trabalhar como se estivéssemos trabalhando dentro do terminal de um Ubuntu.

Agora, se abrirmos outro terminal e executar o comando docker ps, veremos o container que estamos executando. Podemos parar a sua execução, digitando no container o comando exit ou através do atalho CTRL + D .


**Executando novamente um container**
Fazemos isso pegando id do container a ser iniciado, e passando-o ao comando    

    docker start <ID>

Esse comando roda um container já criado, mas não atrela o nosso terminal ao terminal dele. 

Para atrelar os terminais, primeiramente devemos parar o container:

    docker stop <id>

E rodamos novamente o container, passando duas flags: 
-  \-a, de attach, para integrar os terminais, 
- \-i, de interactive, para interagirmos com o terminal, para podermos escrever nele.

    docker start -a -i <id>

Com isso, conseguimos ver um pouco de como subir um container, pará-lo e executá-lo novamente, além de trabalhar dentro dele.


**LAYERED FILE SYSTEM**

As camadas de uma imagem podem ser reaproveitadas em outras imagens. 

As camadas de uma imagem são somente para leitura. O que acontece é que não escrevemos na imagem, já que quando criamos um container, ele cria uma nova camada acima da imagem, e nessa camada podemos ler e escrever.

Então, quando criamos um container, ele é criado em cima de uma imagem base e nele nós conseguimos escrever. E com uma imagem base, podemos reaproveitá-la para diversos containers.

Reaproveitando uma imagem para vários containers

Isso nos traz economia de espaço, já que não precisamos ter uma imagem por container.

**PRINCIPAIS ESTADOS DE UM CONTAINER**
- running: quando criamos um ou iniciamos;
- stopped: quanto a sua execução encerra ou paramos;

**Doceker run**

Então, vamos criar um container que segurará um site estático: 

    docker run dockersamples/static-site

Terminado o download da imagem, o container é executado, pois sabemos que os containers ficam no estado de running quando são criados. 

No caso dessa imagem, o container está executando um processo de um servidor web, que está disponibilizando o site estático para nós, então esse processo trava o terminal. 

Para tal, paramos o container que acabamos de criar. Para impedir o travamento, nós o executamos sem atrelar o nosso terminal ao terminal do container, fazendo isso através da flag -d:

    docker run -d dockersamples/static-site

Assim, o container fica executando em segundo plano. 

Podemos verificar que o container realmente está rodando executando o comando 
    
    docker ps

**Acessando o site**

Precisamos linkar a porta interna do container a uma porta do nosso computador. 

Para fazer isso, precisamos adicionar mais uma flag, a -P, que fará com que o Docker atribua uma porta aleatória do mundo externo, que no caso é a nossa máquina, para poder se comunicar com o que está dentro do container:

    docker run -d -P dockersamples/static-site

Agora, ao executar novamente o comando docker ps, na coluna PORTS, veremos  o mapemento portaHost:PortaContainer

ma outra maneira de ver as portas é utilizar o comando:
    docker port <idDoContainer>

Caso você esteja utilizando o Docker Toolbox, você tem de acessar a porta através do IP da máquina virtual. Para descobrir o IP dessa máquina virtual, basta executar o comando

    docker-machine ip. 

Com o IP em mãos, basta acessá-lo no navegador, utilizando a porta que o Docker atribuiu, por exemplo http://192.168.0.38:9001/.

**Nomeando um container**

Uma outra coisa interessante que é possível fazer quando estamos criando um container é que podemos dar um nome para o container. Para dar um nome para o container, utilizamos a flag --name:

    docker run -d -P --name meu-site dockersamples/static-site

Assim o nome do nosso container será meu-site. 

**Definindo uma porta específica**

Quando estamos criando um container e queremos linkar uma porta interna sua a uma porta do nosso computador, utilizamos a flag -P, para o Docker atribuir uma porta aleatória da nossa máquina, assim podemos nos comunicar com o que está dentro do container. 

Mas podemos definir essa porta, utilizando a flag -p, nesse modelo: 
    
    -p PORTA-MUNDO-EXTERNO:PORTA-CONTAINER, por exemplo:

    docker run -d -p 12345:80 dockersamples/static-site

Nesse exemplo, através da porta 12345 do nosso computador podemos acessar a porta 80 do container.

**Atribuindo uma variável de ambiente**

Além disso, podemos atribuir uma variável de ambiente no container. 

Por exemplo, a página do site estático pega o valor da variável de ambiente AUTHOR e o exibe junto à mensagem de Hello, então podemos modificar o valor dessa variável, através da flag -e:

    docker run -d -P -e AUTHOR="Douglas Q" dockersamples/static-site

**Parando todos os containers de uma só vez**
Podemos ver apenas os ids dos containers que estão rodando, executando o comando:
    
    docker ps -q. 
    
Com esse comando, podemos parar todos os containers de uma só vez. Para isso, podemos utilizar a interpolação de comandos, no padrão $(comando), que executa o comando, captura sua saída e insere isso na linha de comando:

    docker stop $(docker ps -q)

Então, o comando docker ps -q será executado e a sua saída, os ids dos containers que estão rodando, será inserida no comando docker stop, parando assim todos os containers.

O comando docker stop demora um pouco para ser executado pois ele espera 10 segundos para parar o container. 

Podemos diminuir esse tempo através da flag -t, passando o tempo a ser aguardado, por exemplo:

    docker stop -t 0 $(docker ps -q)


\-------------------------------
 ##  Usando volumes
\-------------------------------

**O que são os volumes?**

- Continers são volateis, então quando ele é removido os dados associados a ele são perdidos.
    - Então onde alvar Logs? Dados? 
    - Resposta: Podemos criar dentro dos containers um local epecial: *Os Volumes*
    Especificamos que esse local será o nosso volume de dados.

Ao criar um volume de dados estamos apontando para uma pequena pasta no Docker Host. Ou seja, quando criamos um volume, criamos uma pasta dentro do container, e o que escrevermos dentro dessa pasta na verdade estaremos escrevendo do Docker Host.

Isso faz com que não percamos os nossos dados, pois o container até pode ser removido, mas a pasta no Docker Host ficará intacta.

**Trabalhando com volumes**

Vamos criar um container com o docker run, mas dessa vez utilizando a flag -v para criar um volume: 
    
    docker run -v "/var/www" ubuntu

No exemplo acima, criamos o volume "/var/www"
Mas a que pasta no Docker Host ele faz referência? 
Para descobrir, podemos inspecionar o container, executando o comando docker inspect, passando o id :

    docker ps -a
    docker inspect <id> 

Temos uma saída com diversas informações, mas a que nos interessa é o "Mounts":

"Mounts": [
    {
        "Type": "volume",
        "Name": "bfbebbe2613fc5d0922d60fe7e33a293afdcb2bf9134c29daa2575b42f7571a6",
        "Source": "/var/lib/docker/volumes/bfbebbe2613fc5d0922d60fe7e33a293afdcb2bf9134c29daa2575b42f7571a6/_data",
        "Destination": "C:/Users/andressa.farias/AppData/Local/Programs/Git/var/www",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
    }
]

Nele, podemos ver que o /var/www será escrito na nossa máquina no diretório /var/lib/docker/volumes/5e1cbfd48d07284680552e56087c9d5196659600ccd6874bfa3831b51ddd0576/_data, esse endereço é gerado automaticamente pelo Docker.

Ao remover o container, a pasta continuará na nossa máquina. 

Essa pasta gerada pelo Docker pode ser configurada, podemos dizer a pasta que será referenciada pela pasta /var/www do container. 

Por exemplo, se quisermos escrever dentro do Desktop da nossa máquina, devemos passá-lo antes do volume, separando-os com dois pontos. Além disso, vamos executar o container no modo interativo:

    docker run -it -v "C:\Users\Alura\Desktop:/var/www" ubuntu

Ou seja, quando escrevermos na pasta /var/www do container, estaremos escrevendo no Desktop da nossa máquina. 

**Exercicio**
Dentro do container:

root@abd0286c0083:/# cd /var/www/
root@abd0286c0083:/var/www# touch novo-arquivo.txt
root@abd0286c0083:/var/www# echo "Este arquivo foi criado dentro de um volume" > novo-arquivo.txt

Ao acessarmos o nosso Desktop, o arquivo estará lá, também com a mensagem escrita. E ao remover o container, a sua camada de escrita é removida, mas os arquivos continuam no nosso Desktop.

**Importancia dos Volumes**

Então, o uso de volumes é importante para salvarmos os nossos dados fora do container, e esses volumes sempre estarão atrelados ao Docker Host. No caso acima, atrelamos o volume com o Desktop, mas podemos atrelar com um lugar mais seguro, salvando os dados do banco de dados nele, logs, e até mesmo o código fonte, coisa que faremos no próximo vídeo.

### **Rodando código em um container**
O que escrevemos no volume (pasta /var/www do container) aparece na pasta configurada da nossa máquina local. Mas podemos pensar o contrário, ou seja, tudo o que escrevemos no Desktop será acessível na pasta /var/www do container.

Isso nos dá a possibilidade de implementar localmente um código de uma linguagem que não está instalada na nossa máquina, e colocá-lo para compilar e rodar dentro do container. 

**Rodando código em um container**

Vamos usar um exemplo escrito Node.js.

Agora, como fazemos para criar um container, que irá pegar e rodar esse código Node que está na nossa máquina? Vamos utilizar os volumes. 

Primeiramente, como vamos rodar um código em Node.js, precisamos utilizar a sua imagem:

    docker run node

Além disso, precisamos criar um volume, que faça referência à pasta do código no nosso Desktop:

    docker run -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" node

Agora, para iniciar o seu servidor, executamos o comando npm start. 
Para executar um comando dentro do container, podemos iniciá-lo no modo interativo ou passá-lo no final do docker run:

    docker run -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" node npm start   

Por fim, esse servidor roda na porta 3000, então precisamos linkar essa porta a uma porta do nosso computador, no caso a 8080. O comando ficará assim:

    docker run -p 8080:3000 -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" node npm start

Executado o comando, recebemos um erro. 
Nele podemos verificar a seguinte linha:

> npm ERR! enoent ENOENT: no such file or directory, open '/package.json'

Isto é, o package.json não foi encontrado, mas ele está dentro da pasta do código. 
O que acontece é que o container não inicia já dentro da pasta /var/www, ele iniciar em uma pasta determinada pelo próprio container. 

Então devemos especificar que o comando npm start deve ser executado dentro da pasta /var/www. 
Para isso, vamos passar a flag -w (Working Directory), para dizer em qual diretório o comando deve ser executado, a pasta /var/www:

    docker run -p 8080:3000 -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" -w "/var/www" node npm start

Agora, ao acessar a porta 8080 no navegador, vemos uma página exibindo a mensagem Eu amo Docker!. 


Melhorando o comando

    docker run -d -p 8080:3000 -v "$(pwd):/var/www" -w "/var/www" node npm start