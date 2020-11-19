# define a imagem base 
#FROM php:5.6-apache
ARG IMAGE_TAG
FROM php:$IMAGE_TAG
# instalação do mysqli para permitir a comunicaão com o banco
RUN docker-php-ext-install mysqli

# cópia dos arquivos do projeto para dentro do container
COPY . /var/wwww/html

EXPOSE 80



-----------
