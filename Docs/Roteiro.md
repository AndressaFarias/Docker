# Requisitos
1 VM com 1 core e 2gb de ram

# Passos
1. Instalar o Docker : [About Docker CE](https://docs.docker.com/install)
    * ` curl -fsSL https://get.docker.com | bash  `

2. Adicionar o user ao grupo docker, para que o memso possa gerenciar os usuários
    * `  sudo usermod -aG docker usuario   `

3. Execute os comandos inciiais 
    * ` docker version `
    * ` docker container ls `


4. Criando o primeiro container
    * ` docker container run -it hello-world`


    ` `