# **DOCKERFILE** 

O arquivo _Dockerfile_ é utilizado para definir os comandos para executar instalações complexas com características específicas a serem utilizadas pelo containers.

Os principais comandos são `FROM`, `COPY`, `WORKDIR`, `RUN`, `EXPOSE` e `ENTRYPOINT`...

Ao subir uma imagem criada através de um Dockerfile para o Docker Hub disponibilizamos para os todos os desenvolvedores.



## CONSTRUINDO NOSSA IMAGEM

Uma imagem é como se fosse uma receita de bolo. Então, precisamos criar a nossa própria receita de bolo, para criar a nossa imagem.

### Montando o Dockerfile

Criar dentro do nosso pojeto um arquivo Dockerfile

O nome do arquivo pode ser `Dockerfile` ou `id.dockerfile`, essa segunda opção é mais usada quando temos mais de um `Dockerfile` por projeto.

Geralmente, montamos as nossas imagens a partir de uma imagem já existente. 
Para informar qual é a imagem-base que iremos usar utilizamos a instrução `FROM` seguida do nome da imagem.
    
   `FROM node`

Podemos indicar a versão da imagem que queremos utilizar ou indicar latest, que faz referência à versão mais recente da imagem. 
Se não passarmos versão nenhuma, o Docker irá assumir que queremos o latest.

   `FROM node:latest`

Outra instrução que é comum colocarmos é quem cuida, quem criou a imagem, através do comando `MAINTAINER`

Agora, especificamos o que queremos na imagem. 

Queremos colocar o nosso código dentro da imagem, então utilizarmos o comando `COPY`.

Como queremos copiar tudo o que está dentro da pasta, dentro do nosso host, vamos utilizar o `.` para copiar tudo que está na pasta do arquivo Dockerfile, e vamos copiar para /var/www.

   `COPY <origem> <destino>`

   `COPY . /var/www`

No projeto, já temos as dependências dentro da pasta `node_modules`, mas não queremos copiar essa pasta para dentro do container, pois elas podem estar desatualizadas,quebradas, etc. então queremos que a própria imagem instale as dependências para nós, rodando o comando `npm install`. Para executar um comando, utilizamos o `RUN`

   `RUN npm install`

Agora, deletamos a pasta node_modules, para ela não ser copiada para o container. 

Toda imagem possui um comando que é executado quando a mesma inicia, e o comando que utilizamos na aula anterior foi o `npm start`. 
Para isso, utilizamos o comando `ENTRYPOINT`, que executará o comando que quisermos assim que o container for carregado:

   `ENTRYPOINT npm start`

Também podemos passar o comando como se fosse em um array, por exemplo `["npm", "start"]`, ambos funcionam.

Falta colocarmos a porta em que a aplicação executará, a porta em que ela ficará exposta. Para isso, utilizamos o `EXPOSE`:

   `EXPOSE 3000`

Por fim, falta dizermos onde os comandos rodarão, pois eles devem ser executados dentro da pasta `/var/www`. Então, através do comando `WORKDIR`, assim que copiarmos o projeto, dizemos em qual diretório iremos trabalhar;

...
   `WORKDIR /var/www`
...

Com isso, finalizamos o Dockerfile, baseado no comando que fizemos na aula anterior:

    `docker run -p 8080:3000 -v "$(pwd):/var/www" -w "/var/www" node npm start`

```
    FROM node:latest
    MAINTAINER Andressa Idalgo
    COPY . /var/www
    WORKDIR /var/www
    RUN npm install
    ENTRYPOINT  ["npm", "start"], 
    EXPOSE 3000
``` 

### Criando a imagem

Para criar a imagem, precisamos fazer o seu build através do comando `docker build`, comando utilizado para buildar uma imagem a partir de um Dockerfile.
Para configurar esse comando, passamos o nome do Dockerfile através da flag -f

   `docker build -f Dockerfile`

Como o nome do nosso Dockerfile é o padrão, poderíamos omitir esse parâmetro, mas se o nome for diferente, é preciso especificar.

Além disso, passamos a tag da imagem, o seu nome, através da flag `-t`.

Já vimos que para imagens não-oficiais, colocamos o nome no padrão `NOME_DO_USUARIO/NOME_DA_IMAGEM`, então é isso que faremos

   `docker build -f Dockerfile -t douglasq/node`

E agora dizemos onde está o Dockerfile. Como já estamos rodando o comando dentro da pasta volume-exemplo, vamos utilizar o ponto (.);

   `docker build -f Dockerfile -t xxxx/node .`

Ao executar o comando, podemos perceber que cada instrução executada do nosso Dockerfile possui um id. 

Isso ocorre pois para cada passo o Docker cria um container intermediário, para se aproveitar do seu sistema de camadas. Ou seja, cada instrução gera uma nova camada, que fará parte da imagem final, que nada mais é do que a imagem-base com vários containers intermediários em cima, sendo que cada um desses containers representa um comando do Dockerfile.

   `docker run -d -p 8080:3000 xxxx/node`

Ao acessar o endereço da porta no navegador, vemos a página da nossa aplicação. No Dockerfile, também podemos criar variáveis de ambiente, utilizando o ENV. Por exemplo, para criar a variável PORT, para dizer em que porta a nossa aplicação irá rodar, fazemos:

```
    FROM node:latest
    MAINTAINER
    ENV PORT=3000
    COPY . /var/www
    WORKDIR /var/www
    RUN npm install
    ENTRYPOINT npm start
    EXPOSE 3000
```

E no próprio Dockerfile, podemos utilizar essa variável:

    FROM node:latest
    MAINTAINER Douglas Quintanilha
    ENV PORT=3000
    COPY . /var/www
    WORKDIR /var/www
    RUN npm install
    ENTRYPOINT npm start
    EXPOSE $PORT

E como modificamos o Dockerfile, precisamos construir a nossa imagem novamente e podemos perceber que dessa vez o comando é bem mais rápido, já que quase todas as camadas estão cacheadas pelo Docker.

Agora que criamos a imagem, vamos disponibilizá-la para outras pessoas. E é isso que veremos no próximo vídeo.

## Quando usar ADD ou COPY
A aplicção do _ADD_ ou do  _COPY_ irá depender do que se quer transferir para o container. De acordo com essa resposta no [SOen](https://stackoverflow.com/questions/24958140/what-is-the-difference-between-the-copy-and-add-commands-in-a-dockerfile/24958548#24958548), a maior diferença do método _ADD_ para o _COPY_ é:
* Método _ADD_ permite que o atributo SRC seja uma URL;
* Se o arquivo informado no atributo SRC do método _ADD_ possuir um formato de compressão reconhecido, ele será descoprimido. [2]

## As melhores práticas para escrever um _Dockerfile_
O Docker cria imagens automaticamente, lendo as instruções de um Dockerfilearquivo de texto que contém todos os comandos, em ordem, necessários para criar uma determinada imagem.

Cada instrução cria uma camada:

~~~yml
   FROM ubuntu:18.04
   COPY . /app
   RUN make /app
   CMD python /app/app.py
~~~

* **FROM** cria uma camada da ubuntu:18.04 imagem do Docker.
* **COPY** adiciona arquivos do diretório atual do seu cliente Docker.
* **RUN** constrói seu aplicativo com make.
* **CMD** especifica qual comando executar dentro do contêiner.

Ao executar uma imagem e gerar um contêiner, você adiciona uma nova camada gravável (a "camada contêiner") na parte superior das camadas subjacentes.


# Referencias
[1] Alura
[2] https://pt.stackoverflow.com/questions/202406/quando-usar-add-ou-copy-para-copiar-arquivos-em-um-dockerfile 
[3] [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#add-or-copy)