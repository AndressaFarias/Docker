# Material de Apoio
Segue o link do material de apoio completo: https://github.com/badtuxx/DescomplicandoDocker

# ANTIGAMENTE
Era comum que cada aplicação ficasse em um servidor físico diferente. 
Para que essas aplicações pudessem ser instaladas era preciso: 
   - Primeiro, instalar em cada um dos servidores o respectivo SO (Sistema Operacional);
   - Também era preciso que hauvesse a comunicação entre esse servidores- o que demanda a criação de uma rede;
   - Pensando também que para o funcionamento dos servidores era preciso energia elétrica, quanto mais serviores físicos mais gasto com energia;
O processo de provisionamento era lento, pois para cada nova aplicação era necessário comprar/montar uma servidor físico, instalar o SO, configurá-lo e subir a aplicação.

## Capacidade Pouco Aproveitada
Esse modelo de arquitetura tornava comum haver servidores parrudos com uma única aplicação sendo executada. Normalmente ficavam funcionando abaixo da sua capacidade, para que quando fosse necessário o servidor pudesse suportar uma grande quantidade de acessos. 

Isso resultava em muita capacidade ociosa nos servidores, com muitos recursos desperdiçados.


# VIRTUALIZAÇÃO
Após o advento da tecnologia chamada de _Hypervisor_, foram possiveis as máquinas virtuais.
As máquinas virtuais tinham o intuito de reduzir o alto tempo e custo de executar aplicações em servidores físicos.

![Estrutura Máquin Virtual](https://i2.wp.com/opensourceforu.com/wp-content/uploads/2016/02/Figure-2.png)

O _Hypervisor_ funciona em cima do sistema operacional, permitindo a virtualização dos recursos físicos do nosso sistema. Assim, criamos uma máquina virtual que tem acesso a uma parte do nosso HD, memória RAM e CPU, criando um computador dentro de outro:

![VM](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/maquina-virtual.png)

Se temos uma VM acessando apenas uma parte do nosso hardware é possivel colocar dentro dela uma das nossas aplicações. 

Desta forma, reduzimos a quantidade de servidores e consequentemente os custos com luz e rede. Além disso, dividimos melhor o nosso hardware, aproveitando melhor os seus recursos e diminuindo a ociosidade.

## Problemas das Máquias Virtuais

O uso de máquina virtuais resolve os problemas alto indice de ociosidade nos servidores e redução na quantidade de servidores necessários para disponibilização do nosso ambiente. 

Porém as VMs também possuem problemas. Para podermos hospedar a nossa aplicação em uma máquina virtual também precisamos instalar um sistema operacional nela:

![VMs com Aplicações](https://www.researchgate.net/profile/Umar_Farooq_Minhas/publication/242077512/figure/fig2/AS:282710602993666@1444414868359/Hosted-Virtual-Machine-Architecture.png)

Cada aplicação necessita de um SO para poder ser executada e esses sistemas podem ser diferentes do SO que está instaldo no servidor físico.

Os SOs possuem custo de memória RAM, disco e processamento, ou seja, precisamos de uma capacidade mínima para manter as funcionalidades de um SO, aumentando o seu custo de manutenção a cada sistema que tivermos.

Além disso, há um custo de configuração, isto é, liberar portas, instalar alguma biblioteca específica, etc.