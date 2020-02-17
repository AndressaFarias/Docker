# Docker Compose

Nos auxilia a lidar com múliplos _containers_.

## Entendendo o Docker Compose

Ao invés de subir todos containers na mão, o que vamos fazer é utilizar uma tecnologia aliada do Docker, chamada **Docker Compose**, feito para nos auxiliar a orquestrar melhor múltiplos containers. 

Ele funciona seguindo um arquivo de texto YAML (extensão .yml), e nele nós descrevemos tudo o que queremos que aconteça para subir a nossa aplicação, todo o nosso processo de build, isto é, subir o banco, os containers das aplicações, etc.s

## Entendendo a aplicação

Para começarmos a entender como funciona o Docker Compose, primeiramente vamos entender como funciona a aplicação que utilizaremos como base. 

Utilizamos a imagem nginx, ou criamos a nossa própria. Como vamos configurar o NGINX para algumas coisas específicas, como lidar com os arquivos estáticos, vamos criar a nossa própria imagem, por isso que na aplicação há o nginx.dockerfile:

```
FROM nginx:latest
MAINTAINER Douglas Quintanilha
COPY /public /var/www/public
COPY /docker/config/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80 443
ENTRYPOINT ["nginx"]
# Parametros extras para o entrypoint
CMD ["-g", "daemon off;"]
```

Nesse arquivo, nós utilizamos a última versão disponível da imagem do nginx como base, e copiamos o conteúdo da pasta public, que contém os arquivos estáticos, e um arquivo de configuração do NGINX para dentro do container. Além disso, abrimos as portas 80 e 443 e executa o NGINX através do comando nginx, passando os parâmetros extras -g e daemon off.

Por fim, vamos ver um pouco sobre o arquivo de configuração do NGINX, para entendermos um pouco como o load balancer está funcionando.

No arquivo nginx.conf, dentro server, está a parte que trata de servir os arquivos estáticos. Na porta 80, no localhost, em /var/www/public, ele será responsável por servir as pastas css, img e js. E todo resto, que não for esses três locais, ele irá jogar para o node_upstream.

No node_upstream, é onde ficam as configurações para o NGINX redirecionar as conexões que ele receber para um dos três containers da nossa aplicação. O redirecionamento acontecerá de forma circular, ou seja, a primeira conexão irá para o primeiro container, a segunda irá para o segundo container, a terceira irá para o terceiro container, na quarta, começa tudo de novo, e ela vai para o primeiro container e assim por diante.

Isso já está tudo pronto, basta baixarmos o código da imagem e da aplicação aqui.

Agora, no próximo vídeo, escreveremos o responsável por orquestrar a subida de cada uma dessas partes da nossa aplicação, o docker-compose.yml.


## Criando o docker-compose.yml

## Subindo os serviços

## Instalando o Docker Compose no Linux

## Docker e Microserviços



