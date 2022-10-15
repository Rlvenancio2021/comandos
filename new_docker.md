# Docker

## Instalação

1. Passo
Podemos verificar no site várias formas de instalação, e podemos seguir com o processo abaixo:
site: docs.docker.com
ou 
```
$ curl -fsSl https://get.docker.com -o get-docker.sh
$ sh get-docker.sh
```
Para confirmar se o docker foi instalado com sucesso
```
$ docker version
$ sistemctl status docker
```

2. Passo
Pesquisar imagem desejada no site **hub.docker.com** e utilizar o comando indicado para cópia
Ex.:
```
$ docker pull hello-word
$ docker pull hello-world:<TAG>
```

Para verificar se a imagem foi criar o comando é
```
$ docker images
```

Para executar a imagem
```
$ docker run hello-word
```


## Comandos dos dia-a-dia

- Para docker existem duas sintáxes antiga e atual, porém as duas funcionam
```
 ## Antiga ##
$ docker ps
$ docker run
...
 ## Nova ##
$ docker container ls 
$ docker container run
```

- Comandos para verificar containers estão rodando
```
$ docker ps
	# Parametros 
	-a # para mostrar o que ocorreu recentemente
```

Comando para verificar os serviços que estão rodando
```
$ docker service ls
```

- Comando para executar uma imagem docker, com o comando abaixo irá entrar na imagem, caso a imagem possua uma TAG específica, informar o nome da Tag no comando **IMAGEM:TAG**
```
$ docker run -it <NOME DA IMAGEM/REPOSITORY>
	     -d # para rodar em background

$ docker run -dti --name <ESCREVER UM NOME> <NOME DA IMAGEM> # cria um container com um nome específico
```

No caso de um container está rodando em **background** para entrar no terminal dele execute o comando abaixo, será necessário pegar o número do ID pode usar o comando **docker ps**.
- Comando **docker exec -it...** server para executar comando no container docker que estiver rodando, sem a necessidade de logar nele.
```
$ docker exec -it <ID do CONTAINER> /bin/bash
```

- Comando para **Excluir um container**. Não exclui os volumes.
```
$ docker rm <ID do Container>
	# Parâmetro:
	-f # Para força a exclusão do container, sem precisar "parar".
```

**Comando para excluir em massa todos os container que não estiver em uso**
```
$ docker container prune
```


- Comando para excluir uma imagem
```
$ docker rmi <NOME DA IMAGEM>
```

- Comando para copiar um arquivo para um container
```
 # PARA
$ docker cp arquivo1.txt <NOME ou ID do CONTAINER>:<CAMINHO DA PASTA DESTINO DO ARQUVIO ex.: /destino>
 # DO
$ docker cp <NOME ou ID do CONTAINER>:<CAMINHO E NOME DO ARQUIVO> <NOME DA CÓPIA DO ARQUIVO>
$ docker cp Ubuntu:/destino/Meuzip.zip Zipcopia.zip
```

- Comando para **verificar informações** do container
```
$ docker inspect <NOME ou ID DO CONTAINER>
```


## Imagem de Banco de Dados MySQL

Podemos baixar uma imagem direto do banco de dados, não é necessário baixar um servidor e instalar nele, assim ocupa menos espaço
```
$ docker pull mysql
```

No caso do MySQL existem algumas variáveis de ambiente que são necessário declarar no momento de criação do container
```
$ docker run -e MYSQL_ROOT_PASSWORD=Senha123 --name MySQL-A -d -p 3306:3306 mysql #(nome da imagem)
		# Parâmetros
		-e # seta variável de ambiente, neste caso a senha do root é obrigatório para o MySQL se tiver mais é só declarar
		-p # informar a porta do host : e qual porta está ouvindo
```

Para logar no MySQL para executar os comandos é
```
$ docker exec -it <ID ou NOME> bash
```

Para acessar o banco de dados sem precisar logar no container, é necessário instalar o mysql-client, desde que estejam na mesma faixa de IP
```
$ apt install mysql-client
$ mysql -u root -p --protocol=tcp
```
 - Protocolo tcp é o utilizado para conexão padrão com o banco de dados, é possível verificar no momento da criação do container

Para conectar o banco de dados no Workbench é necessário informar o IP da máquina que o docker está rodando e informar o usuário e senha do banco de dados, desde que o **mysql-client** esteja instalado.




## Tipos de Armazenamento de Dados

### Bind

Para verificar dados sobre o container, inclusive aonde está gravando os arquivos utilizar.
```
$ docker inspect <NOME ou ID do CONTAINER>
```
Para proteger os dados e gravar fora do container (caminho padrão) podemos seguir com as instruções abaixo, desta forma caso perca o container, ao criar um novo, será necessário apenas indicar o caminho criado conforme o procedimento 1 abaixo e os dados serão recuperados no banco de dados.	
 - Verificar a chave **Mounts** item  **Type: volume** mostra o diretório que os arquivos estão sendo gravados
	Para gravar os dados fora do container usar:
		1. Criar um diretório para gravar os dados, ex.: /data/MySQL(nome do container)
		2. Incluir na criação do container o **parâmetro --volume**
		```
		docker run -e MYSQL_ROOT_PASSWORD=senha --name MySQL -d -p 3306:3306 --volume=/data/MySQL:/var/lib/mysql mysql
		```

### Mount

Comando para criação do container, já configurando a armazenamento conforme **mount**, é necessário já ter criado a diretório no host (máquina que hospeda o docker).
```
$ docker run -dit --mount type=bind,src/data/debian-A,dst=/data debian
```
Dessa forma o arquivo será criado tanto no container quando no host em "duplicidade".
 - src = Search
 - dst = Destino

Esite a possibilidade de criar o arquivo de modo que no docker ele fique **somente para leitura**
```
$ docker run -dit --mount type=bind,src/data/debian-A,dst=/data,ro debian
```
Dessa forma o arquivo será criado tanto no container quando no host em "duplicidade".

Glossário
 - src = Search
 - dst = Destino
 - ro = Read only


## Volume

Comando para listar os **volumes criados** pelo docker
```
$ docker volume ls
```

**Comando SENSÍVEL, pois limpa todos os volumes que estiver no host que não estiver em uso**
```
$ docker volume prune
```


Para criar/excluir o próprio volume. Será criado na pasta padrão **/var/lib/docker volume**
```
$ docker volume create NOME-DO-VOLUME
$ docker volume rm NOME-DO-VOLUME
```
Neste volume podemos inclusive criar os arquivos diretamente

Para criar um container usando o volume criado, utilizamos o seguinte comando:
```
$ docker run -dti --name debian-B --mount type=volume,src=data-debian,dst=/data debian
```

*Obs* Sempre que tiver dúvida em relação ao tipo de armazenamento que está configurando no containder, podemos usar o comando **docker inspect NOME/ID-DO-CONTAINER** e verificar a informação na chave **Mounts**.


## Aplicação Web com Apache

imagem ```docker pull httpd```

1. Criar um diretório no host para os arquivos do projeto com apache
2. Criar os arquivos de html
3. Mapear o diretório no container que será criado
```
$ docker run --name apache-A -d -p 80:80 --volume=/data/apache-A:/usr/local/apache2/htdocs httpd
```
Obs. o caminho **/usr/local/apache2/htdocs** foi apresentado conforme documentação da imagem no docker hub
4. Para acessar a página é necessário utilizar o número de **ip do host**

## Aplicação com PHP

Baixar imagem do PHP tag 7.4 com Apache
```
$ docker pull php:7.4-apache
```

1. Criar um diretório no host para os arquivos do projeto com apache
2. Criar os arquivos de html (**index.php**)
3. Mapear o diretório no container que será criado
```
$ docker run --name php-A -d -p 8080:80 --volume=/data/php-A:/var/www/html -php:7.4-apache
```
## Processamento, Logs e Rede

### Processamento

Para verificar os recursos **utilizados/limite** do conteiner rodamos o comando abaixo, mostra em tempo real o uso de recursos do conteiner
```
$ docker stats <NOME/ID do CONTEINER>
```

Podemos na criação do conteiner limitar os seus recurosos protegendo assim o processamento do host e demais conteiners, passando os parâmetros
```
$ -m 128m # memmória limitando a 128 megabytes
$ --cpus 0.2 # CPU limitando no exemplo a 20% de uso
$ docker run --help # para consultar outros parâmetros
```

Para atualizar algum recurso do conteiner
```
$ docker update <NOME/ID do CONTEINER> -m 128M --cpus 0.2
	# Parâmetro
	-m # memmória
	--cpus # CPU limitando no caso a 20%
```

### Informações, logs e processos

Informações no servidor host
```
$ docker info
```

Logs de execução
```
$ docker logs <NOME do CONTAINER>
 # ou 
$ docker container logs <NOME do CONTAINER>
```

Verificar o que está em execução no conteiner
```
$ docker top <NOME do CONTAINER>
 # ou
$ docker container top <NOME do CONTAINER>
```

### Rede

Para verificar as opções
```
$ docker network --help
```

Para listar as redes disponíveis
```
$ docker network ls
```

Para veriricar os conteiner adicionados em uma rede
```
$ docker network inspect <NOME DA REDE>
```

**Criar uma rede**
O objetivo é poder isolar os conteiner que deseja nessa rede, assim nem todos terão acesso, só existe comunicação entre conteiners da mesma rede.
```
$ docker network create <NOME da REDE>
```

Para especificar a rede que deseja utilizar ao criar um conteiner, utilizao o parâmetro ```--network <NOME da REDE>```


## Extra

- Instalar programa **zip** para compactar arquivos e executar comando de compactar
```
$ apt install zip
$ zip Meuzip.zip *.txt
$ unzip Meuzip.zip
```

- Aplicação utlizada para estressar a máquina para fazer teste
```
$ apt install stress

$ stress --cpu 1 -vm-bytes 50m --vm 1 --vm-bytes 50m
	# Um exemplo de teste
```

- Aplicação **ping**
```
$ apt install iputils-ping
```

- Limpeza de arquivo baixados
```
$ apt clean
```

# Definição e Criação de um Docker File

Docker File é uma forma de criar a próprio image do sistema.

Para esse processo podemos seguir os seguintes passos:

1. Criar um diretório no host (ex.: /images/ubuntu-python)

2. Criar/escrever o programa desejado, aqui no caso um programa em Python. Nesse exemplo arquivo de nome **app.py**

3. Criar/escrever o arquivo **dockerfile** na pasta do diretório conforme item 1.
```
$ nano dockerfile

 # Dentro do arquivo
$ FROM ubuntu
$ RUN apt update && apt install -y python3 && apt clean
$ COPY app.py /opt/app.py
$ CMD python3 /opt/app.py
```

from - indica a base do conteiner (imagem original)
run - o que será executado dentro do conteiner
copy - o que será copiado. Informe o caminho da origem
crm - para executar comandos do bash

4. Construir a imagem pelo comando **docker build**
```
$ docker build . -t ubuntu-python
```
Neste caso o "." representa o caminho onde será encontrado o arquivo "dockerfile", caso não seja a pasta em que estiver logado, pasta passar o caminho

5. Verificar a criação da imagem com o comando ```docker images```

6. Para executar o imagem usamos o comando ```docker run ...```
*Obs* caso o código depende de input do usuário, não podemos utilizar o "-d" para rodar em background


## Criando uma imagem personalizada do Apache

1. Cria diretório

2. Cria os documentos do projeto (arquivos HTML, CSS, e outros)
Melhor prática cria em um diretório **provisório** específico ex.: /images/debian-apache/site

3. Compacta todos os arquivo em formado **.tar**
```
$ tar -czf site.tar ./
```
*Obs* "./" indica que são todos os arquivos da pasta atual

4. Copiar o arquivo compactado, no caso, site.tar para o diretório anterior
```
$ cp site.tar ../
```
5. Apagar o diretório provisório criado conforme item 2

6. Criar arquivo dockerfile
```
$ nano dockerfile

 # Dentro do arquivo
$ FROM debian
$ RUN apt-get update && apt-get install -y apache2 && apt-get clean

$ ENV APACHE_LOCK_DIR="/var/lock" 
$ ENV APACHE_PID_FILE="/var/run/apache2.pid"
$ ENV APACHE_RUN_USER="www-data"
$ ENV APACHE_RUN_GROUP="www-data"
$ ENV APACHE_LOG_DIR="/var/log/apache"

$ ADD site.tar /var/www/html

$ LABEL description = "Apache webserver 1.0"

$ VOLUME /var/www/html

$ EXPOSE 80

$ ENTRYPOINT ["/usr/sbin/apachectl"]
$ CMD ["-D", "FOREGROUND"]
```
*Obs*
- configurar as variáveis de ambiente **ENV APACHE_...**
- Um arquivo Pid é um arquvio que contém o número de identificação do processo (pid)
- Descrição das variáveis
ENV APACHE_LOCK_DIR= Evita que tenha mais de uma execução do apache no mesmo conteiner 
ENV APACHE_PID_FILE=
ENV APACHE_RUN_USER= Usuário que executará o apache
ENV APACHE_RUN_GROUP= 
ENV APACHE_LOG_DIR= Diretório de log

EXPOSE porta

ENTRYPOINT por ser uma aplicação a ser executada em Primeiro Plano, aqui será especificado o arquivo de execução, no caso do apache é o "apachectl".
CMD para determinar o modo de execução, no caso é em primeiro plano

7. Contruir a imagem com o comando **docker image build...**
```
$ docker image build -t debian-apache:1.0 .
```
*Obs* "debian-apache:1.0" é o nome dado a imagem que está sendo criada.


8. Criar o conteiner
```
$ docker run -dit -p 80:80 --name meu-apache debian-apache:1.0
```


## Criando uma imagem personalizada do Python

O único passo que altera é o de **criação do DockerFile** pegando como base uma **imagem Python**
```
$ docker pull python

$ FROM python
$ WORKDIR /usr/src/app
$ COPY app.py /usr/src/app
$ CMD ["python", "./app.py"]
```
*Obs* o "WORKDIR" direciona o programa para o caminho indicado, assim ao executar o "CMD" não é necessário indicar o caminho aboluto.


## Gerando uma imagem MULTISTAGE

Imagem construida em mais camadas, onde será pego o binário do **golang** e a imagem base do **alpine** para construir uma nova imagem. Assim na imagem será acrescido apenas o espaço do binário (que é apenas o desejado do golang) otimizando espaço.

Forma de gerar uma aplicativo através da imagem do golang (linguagem go), encapsulando o binário em outra imagem, no caso Alpine, gerando uma imagem pequena da aplicação, fácil de ser compartilhada.

```
$ docker pull golang
$ docker pull alpine

==================================================

$ FROM golang as exec

$ COPY app.go /go/src/app/

$ ENV GO111MODULE=auto

$ WORKDIR /go/src/app

$ RUN go build -o app.go .

$ FROM alpine

$ WORKDIR /appexec
$ COPY --from=exec /go/src/app/ /appexec
$ RUN chmod -R 755 /appexec
$ ENTRYPOINT ./app.go
```
*Obs*
 - "FROM golang **as exec**" Instrução que permite importar o executável no próximo estágio 
 - "RUN" será utilizado o run, pois a idéia é pegar o códido do **app.go** e gerar um executável, o RUN executa uma instrução e enviar o retorno dela, para a primeira camado do dockerfile. Permitindo fazer alterações.
Auxilia transforma um script em um arquivo binário.
 - ENTRYPOINT sem o uso dos conchetes, é semelhante ao uso do CMD, ou seja uma execução bash
 - Alpine é uma imagem de uma versão minima do Linux


Construção da imagem
```
$ docker run -it --name runapp2 app-go:1.0
```

## Criar imagem para carregar no Hub do Docker

É necessário criar a imagem com o nome do usuário

1. Logar na conta Hub do Docker via terminal (CLI)
```
$ docker login -u <USUÁRIO>
$ Password:

 # para deslogar
$ docker logout
```
2. Criar a imagem
```
$ docker build . -t <USUÁRIO>/my-go-app:1.0
```

3. Carregar para o Hub Docker
```
$ docker push <USUÁRIO>/my-go-app:1.0
```

4. Para baixar a imagem
```
$ docker pull <USUÁRIO>/my-go-app:1.0
```

## Criando um servidor de imagens

Para isso podemos usar um servidor específico só para este papel. A ideia é criar um servidor de imagens privado, é interessante o uso pensando no mundo corporativo, onde não seria interessante armazenar isso fora do ambiente da empresa.

Como base podemos usar uma Conteiner chamado **registry**
```
$ docker pull registry:2
```

Comando para rodar no servidor
```
$ docker run -d -p 5000:5000 --restart=always --name registry registry:2
```
 - "restart" para que o conteiner seja reiniciado caso ocorra problemas e que ele sempre carregue junto com o sistema

**Necessário pegar o IP do servidor que está rodando o conteiner**

### Carregar as imagens para o servidor de imagens

Não esquecer de **deslogar** o CLI da conta do Hub Docker

1. **Importante** Não podemos usar no servidor de imagens privado, uma imagem com a Tag (usuário) do Hub Docker, ex.: <USUÁRIO>/my-go-app:1.0, neste caso podemos criar uma cópia com o comando
```
$ docker image tag <IMAGE ID> <IP do SERVIDOR>:5000/my-go-app:1.0 
```

2. Comando para enviar para o servidor a imagem
```
$ docker push <IP do SERVIDOR>:5000/my-go-app:1.0
```
2.1 Caso apresente um erro **Get "https://10.66.192.130:5000/v2/": http: server gave HTTP response to HTTPS client** seguir com os passos
 - Configurar arquivo abaixo
```
$ nano /etc/docker/daemon.json

 # conteúdo

{ "insecure-registries":["<IP do SERVIDOR>:5000"] }
```
 - Depois reiniciar o docker
```
$ systemctl restart docker
```
 - Rodar comando push novamente

3. Para conferir as imagens que consta no servidor rodar o comando
```
$ curl <IP do SERVIDOR>:5000/v2/_catalog
```

4. Para baixar imagens do servidor
```
$ docker pull <IP do SERVIDOR>:5000/<NOME da IMAGEM e TAG>
```

## Docker Compose


Para utilizar o Compose é indicado verificar a versão do YAML que corresponde a verão do Docker, é possível verificar na página do Docker

**Instalação do Docker Compose**

```
$ apt install docker-compose
```

**Melhores práticas**

- Criar no host um **bind** para armazenar os arquivos do banco de dados (ex.: mysql-B) (podemos aproveitar a pasta DATA já criada
```
$ mkdir /data/mysql-B
```

- Criar uma pasta para os arquivos no *Compose*

```
$ mkdir /compose
$ mkdir /compose/primeiro
```

- Criar o documento **docker-compose.yml** e preencher
```
version: '3.8'

services:
  mysqlsrv:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: "Senha123"
      MYSQL_DATABASE: "testedb"
    ports:
      - "3306:3306"
    volumes:
      - /data/mysql-B:/var/lib/mysql
    networks:
      - minha-rede

  adminer:
    image: adminer
    ports:
      - 8080:8080
    networks:
      - minha-rede

networks:
  minha-rede:
    driver: bridge
```

- Desta forma iremos criar dois contêineres, para subir os dois de uma vez só usamos **docker-compose up  -d** para descer usamos **down** para os servidores.
Precisa esta na pasta que contém o arquivo docker-compose.yml
```
$ docker-compose up -d

$ docker-compose down
```

- Ao finalizar consultar os contêineres que estão rodando
```
$ docker ps
```

- Para rodar o sistema, basta pegar o IP do Host na porta definida, no caso 8080.
 - Conforme definido no arquvio **docker-compose.yml** os dados para acesso são:
	Servidor: mysqlsrv
	Usuário: root
	Senha: Senha123
	Base de dados: testedb


### Sobre o projeto 

1. Passo
O compose é a união de vários contêiner em um projeto para serem executados com um único comando. Desta forma, o primeiro passo é criar o projeto individual, cria os contêiner de exemplo: Cria um contêiner para a página HTML escrevendo o código; Cria o contêiner do banco de dados; Cria o contêiner de qualquer outra aplicação, trabalhando a idéia de microserviço, podendo até ter mais de 1 contêiner para banco de dados conforme o projeto necessitar.
Inclusive as pastas onde as informações serão gravadas ou seja toda a estrutuda do projeto individualizado para cada contêiner.

2. Passo
Criar um arquivo **docker-compose.yml** onde será escrito a forma de união dos projetos conforme o "passo 1", nesse arquivo irá constar as imagens, variáveis de ambiente, portas, redes, de cada serviço, (no exemplo acima, são dois serviços **mysqlsrv** e **adminer**).

3. Passo
Rodar os comandos de start **docker-compose up** que pode rodar em background incluindo o parâmetro -b e stop **docker-compose down**.


### Conta oficial do GitHub
github.com/docker/awesome-compose


## Docker Swarm

### Criar reder e sub-reder no AWS 

Usar o serviço **VPC**
- Criar rede (Verificar faixa de IP a ser utilizado)
- Criar sub-rede
- Criar gateway (para ter acesso a internet)
	- Associar o gateway criado a uma rede
- Criar uma tabela de roteamento / tabela de rotas
	- Configurar a rota para internet
	1. Ir na opção de tabela de rotas;
	2. Clicar no ID da tabela de rotas, da roda desejada que irá aparecer nas opções;
	3. Clicar em "Editar rotas", neste opção na **coluna "Alvo"** irá inserir o ID do gateway criado.
	**Obs**  neste caso como se trata da internet como destino será utilizado o faixa de IP 0.0.0.0/0 (todos).
- Para rodar os cluster o docker swarm necessita da liberação da porta 2377, neste caso pode ser liberada apenas para a faixa de IP conforme a rede criada.
	1. Para liberar a porta do firware da AWS, selecionar uma das máquinas do cluster e ir no item de **Segurança**;
	2. Ir na opção **Grupos de segurança** é necessário que as máquinas estejam todos no mesmo grupo, clicar no código do ID;
	3. Clicar em **Editar regras de entrada** em tipo será **TCP personalizado** e a **faixa de ip** da rede criada.


### Criar rede local

Para configuração de máquinas virtuais em massa, pode-se utilizar o software **Vagrant** site "vagrantup.com"
Como opcional pode-se utilizar uma IDE (VSCode) para escrever o script
	1. Criar um diretório;
	2. Rodar o comando ```vagrant init```
	3. Editar o arquivo criado **vagrantfile**
		Configurar a base/imagem do sistema operacional que será utilizada, (ex.: de base/imagem ubuntu 22.04) será a base **bento/ubuntu-22.04**
		Linha a ser alterada é a **config.vm.box = "bento/ubuntu-22.04"**
		Descomentar a linha "config.vm.network "public..."
		Na linha "config.vm.provision "shell..." mudar para ```$ config.vm.provision "shell", path "instalar-docker.sh"```
	4. Criar o arquivo instalar-docker.sh
	5. Rodar o comando ```$ vagrant up```
	6. Podemos rodar a máquina via virtual box ou rodando o comando no terminal ```$ vagrant ssh```
	7. IP o eth0 é o NAT e eth1 é bridge - que será usado para acesso remoto. Usuário e senha vagrant
	8. Para apagar a máquina virtual o comando é ```$ vagrant destroy -f```

 - Outra alternativa é criar as máquinas no **Multipass**

### Docker Swarm

Comando para iniciar o docker swarm. Necessário ter o docker instalado e a máquina que executar o comando será a gerenciadora dos clusters
```
$ docker swarm init
```
*Obs* 
    1. aṕos o comando será exibido o token e a porta de comunicação, anotar os dados para configuração dos nós trabalhadores;
    2. o comando **"docker swarm join token ..."**, será utilizado na configuração dos clusters trabalhadores.
    Com isso os nós serão adicionados ao clusters
    3. Entrar no terminal dos nós trabalhadores e colar o comando **docker swarm join token ...**, com isso esses serão adicionados na lista.
    4. Para conferir os nós, logar no **nó gerenciador** e rodar comando
	```
	$ docker node ls
	```

### Criando serviço em um cluster

Comando para listar os serviços
```
$ docker service ls
```

Comando para criar e parar um serviço
```
$ docker service create --name web-server --replicas 15 -p 80:80 httpd

$ docker service create --name meu-app --replicas 15 -dt -p 80:80 --mount type=volume,src=app,dst=/usr/local/apache2/htdocs/ httpd

$ docker service rm web-server
```
*Obs* "-dt" parâmetro que indica rodar em segundo plano

Comando para verificar onde os serviços foram replicados
```
$ docker service ps <NOME DO SERVIÇO (no caso "web-service")>
```

Comando para excluir os conteineres de um nó específico
```
$ docker node update --availability drain node01
```
Existem 3 opções
	- *active* - ativa o nó para receber serviços
	- *pause* - pausa o nó e impede de receber novos serviços
	- *drain* - limpa e redistribui o serviços do nó em questão e o desativa para novos serviços
*Obs* Neste caso é interessante não deixar conteiner da aplicação rodando no nó gerente, assim ele não fica sobre carregado, é interessante deixar por exemplo um Banco de Dados ou Proxy.



**Replicar arquivos entre os servidores**

Para replicar o mesmo conteúdo em todos os nós é necessário a utilização de um serviço de infraestrutura chamado **NFS**
Necessário a instalação do sistema
```
$ apt intall nfs-server
```
Indicar o diretório a ser replicado, será necessário editar o arquivo chamado **exports** adicionando o caminho do diretório que contém os dados a serem replicados
```
$ nano /etc/exports

 # Inserir a linha abaixo no arquivo
/var/lib/docker/volumes/app/_data *(rw,sync,subtree_check)
```
*Obs* 
 - "rw" - Permissões de Acesso
 - "sync" - Sincronizar demais servidores
 - "subtree_check" - Permissões para as sub-pastas

Rodar comando para exportar
```
$ exportfs -ar
```
Liberar serviço *NFS* no firewall porta padrão é **2049** liberar na faixa de IP, visto que a replicação é apenas entre os servidores
Instalar o cliente do servido nos demais nós
```
$ apt install nfs-common
```
Verificar se o acesso está OK
```
$ showmount -e <IP>
```
Caso não funcione será necessário verificar no firewall se os **acesso UDP está liberado**

Depois rodar o comando para montar a replicação na máquina
```
$ mount <IP>:/var/lib/docker/volumes/app/_data /var/lib/docker/volumes/app/_data
```
Replicar procedimentos em todos os nós trabalhadores

**FIM REPLICA**

### Criadno uma aplicação com Banco de Dados e App(replicado nos nós)

1. Criar um volume para o Banco de Dados, neste exemplo chamado "data"
```
$ docker volume create data
```

2. Rodar conteiner 
```
$ docker run -e MYSQL_ROOT_PASSWORD=senha -e MYSQL_DATABASE=meubanco --name mysql-A -d -p 3306:3306 --mount type=volume,src=data,dst=/var/lib/mysql mysql:5.7
```
*Obs* Rodando apenas em um nó

3. Criar volume e aplicação
```
$ docker volume create app

nano /var/lib/docker/volumes/app/_data/index.php
```

4. Rodar conteiner para a aplicação
```
$ docker service create --name meu-app --replicas 15 -dt -p 80:80 --mount type=volume,src=app,dst=/app/ webdevops/php-apache:alpine-php7
```


### Load Balance

É criado um **único endereço de acesso** e ele direciona para os nós, assim balanceia a carga de cada nó

**Nuvem AWS**

1. Clicar em Load Balancer;
2. Escolher a modalidade e clicar em create;
3. Escolher a rede que foi criada para os clusters;
4. Escolher grupo de segurança, utilizar o grupos criados para os clusters;
5. Confimar o arquivo do **Ping Path**;
6. Escolher as instâncias que serão utilizadas para balanceamento de acesso;
7. Será gerado um **DNS para acesso a aplicação** não será mais utilizado o IP;

*Teste de carga**

Site loader.io/tests
Teste gratis e pagos

O teste é interessante para saber se o cluster está atendendo a necessidade ou se será necessário aumentar memórias, nós, espaço, etc...

- No caminho da aplicação, criar um arquivo com o nome do "token" que é fornecido pelo site, com a própria chave como conteúdo. /var/lib/docker/volumes/app/_data/
