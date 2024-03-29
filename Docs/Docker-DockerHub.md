# **DOCKER HUB** 


Para disponibilizar a imagem para outras pessoas, precisamos enviá-la para o Docker Hub.

* O primeiro passo é criar a nossa conta. Com ela criada, no terminal nós executamos o comando:

    docker login
    
digitamos o nosso login e senha que acabamos de criar.

Após isso, basta executar o comando docker push, passando para ele a imagem que queremos subir, por exemplo:

    docker push <nomeUserDOckerHub>/<NomeImagemQueVaoSubir>

Esse comando pode demorar um pouco, mas terminada a sua execução, podemos ver que várias mensagens Mounted from library/node, ou seja, o Docker já sabe que essas camadas podem ser reaproveitadas da imagem do node, então não tem o porquê dessas camadas subirem também, então só as camadas diferentes são enviadas para o Docker Hub.

Mais uma vantagem em se trabalhar com camadas, o Layered File System, pois até na hora de fazer o upload, só é feito das camadas diferentes, as outras são referenciadas da imagem-base que estamos utilizando, no caso a do node.

Por fim, ao acessar a nossa conta do Docker Hub, podemos ver que a imagem está lá. Para baixá-la, podemos utilizar o comando docker pull:

    docker pull andressaidalgof/node

Esse comando somente baixa a imagem, sem criar nenhum container acima dela.

Então, esse é um jeito de simples de compartilharmos uma imagem com outras pessoas, através do Docker Hub. 

A imagem é disponibilizada em um repositório público, mas também podemos disponibilizar em repositórios privados, que no momento da criação do curso, cada usuário pode criar um repositório privado gratuitamente.