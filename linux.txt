## Comandos Servidor Linux

- Comando para exibir os números de IP, o IP estará na propriedade "enp0s3" no "inet"
```
$ ip a
```

- Para acessar uma maquina virtual do via Linux/MacSO seguir da seguinte forma:
```
$ ssh <usuário da máquina>@<número de IP>

# Máquina Virtual Pública AWS/Azure, baixar chave SSH extensão .pem
$ ssh -i <nome da chave.pem> <usuário da máquina>@<númedo de IP>
```
No Windowns usar o programa puTTY, é uma opção

- Comando para instalar serviço "OPEN SSH"
```
$ sudo apt-get install openssh-server
```

### Usuário Administrador e Grupos

- Verificação de grupos e usuários
```
$ cat /etc/group # Lista conteúdo do arquivo de grupos dos sistema
$ cat /etc/sudoers # Contém as especificações para utilizar o comando "sudo"
```
Para ser admin é necessário estar nos grupos "admin" e "sudo"

- Administração de usuários
```
$ sudo passwd <nome do usuário> # Atribui senha ao usuário existente
```

- Arquivo SSHD 
É o portal de acesso ao sistema via SSH, guarda o log de todas as tentativas e acessos realizados no sistema

-- Para permitir relizar login via usuário root é necessário alterar o arquivo sshd_config na linha **PermitRootLogin** descomentar 
essa linha e trocar **prohibit-password** por **yes**, depois ou restart o sistema ou o servido "sshd". Para alteração necessário 
utilizar editor de texto vim ou nano
***Não é a melhor prática por questões de segurança***
```
$ cat /etc/ssh/sshd_config
$ systemctl status sshd # Verifica status do serviço SSHD
$ systemctl restart sshd
```

## Editor de Texto VIM e NANO




## Comandos para o dia-a-dia

 - Comando para listar conteúdo de um diretório
```
$ ls
$ ls | more # lista o arquivos e permite correr a lista com "enter"
$ ls p* # comando para listar arquivos iniciado com "p" apresenta tbm o seu conteúdo
$ ls m?g* # comando para filtar o que tiver a 1 letra "m" e a 3 "g" as demais não importa
$ ls arquivo[1-3].txt ou * # comando para listar um filtro de arquivos no exemplo de 1 ao 3 no padrão de nome arquivos1.txt
$ ls -d */ # Lista apenas diretorios, necessário está dentro da pasta que deseja realizar a listagem
$ ls */ # Lista o conteúdo de cada diretorio a partir do diretorio que estiver logado
$ ls -R /<nome de um diretório>/ # Irá listar o conteúdo de todos os diretórios a partir do que estiver logado.
```
# variáveis do comando (exemplo de arquivo nomeado como arquivo1.txt, arquivo2.txt ...)
	[1-3] - lista do 1 ao 3
	[2,5] - lista o 2 e o 5
	[^3-5] - não lista do 3 ao 5
 - Essas variáveis podem ser utlizadas para excluir, copiar, filtrar arquivos entre outros, teste para saber!!

- Comandos de busca de arquivos "find" é utilizado em conjunto com parâmetros, irá listar os arquivos e caminhos encontrados a 
partir do diretório que estiver logado.
```
$ find -name <nome do arquivo, pode usar o inicio + * para busca>
```

- Comando para verificar menu de ajuda **--help** e/ou **man**
```
$ ls --help
$ man ls
```

- Comandos para criar arquivos
Cria arquivo vazio
```
touch <nome do arquivo com extensão>
```
Cria e escreve no arquivo
```
echo <texto do arquivo> > <nome do arquivo>
```

- Comando history, server para verificar o historico de comandos realizados por um determinado usuários, também permite o 
reaproveitamento do comando passando para o sistema o número dele na lista
```
$ history
$ !7 # 7 é no exemplo, o númedo do indice do comando que desejo reaproveitar
$ !! # reaproveita o último comando
$ !?dat? # verifica na lista se algum comando já realizado atente o parâmetro e o executa
$ history | grep "Aula" # Irá listar todos os comandos que usou a palavra "Aula"
$ history -c # limpa o historico de comandos
```
- Para alterar a variável de ambiente do comando history e configurar para exibir a data e hora que o comando foi executado
```
$ export HISTTIMEFORMAT="%c"
```

- Editando o comando HISTORY, necessáiro realizar por usuário
```
$ set +o history # desativa armazenamento
$ set -o history # ativa armazenamento
```

O history armazena por padrão os 1.000 últimos comandos, para altera é necessário realizar alteraça dentro do arquivo **.bashrc** 
de cada usuário, no parâmetro **HISTSIZE**.

Para cria um arquivo com todos os comandos que foram executados pelo usuário logado, usar o comando ```$ history -w``` irá criar um 
arquivo **.bash_history** na pasta do usuário com as informações listadas, é possível determinar o seu tamanho pelo parâmetro 
**HISTFILESIZE** no arquivo **.bashrc**.


### Comandos para atualização do sistema ###

1. sudo apt update - Comando que lista todos os pacotes que precisam ser instalados

1.2 apt list --upgradeable - Comando para exibir os pacotes a serem atualizados

2. sudo apt upgrade - Comando para realizar as atualizações listadas

3. lsb_release -rs - Comando para verificar a versão do Linux.

4. sudo apt install gcc make perl curl - Instalação de pacote padr

## Teclas de atalho no terminal

 - ctrl + shift + C //Copiar
 - ctrl + shift + V //Colar

### Comandos para instalação de pacotes e programas

1. Virtual Box
 sudo apt-get install virtualbox

1.2 Comando para aumentar a dimensão da tela
sudo apt install build-essential dkms
sudo apt-get install virtualbox-guest-additions-iso

 - Ir no Menu Divices/Insert Guest additiions

 - Abrir terminal e rodar o comando
./autorun.sh

### Comandos para instalação do MySQL ###

1. # Instalando o MySQl e verificando status do sistema #

1.1. sudo apt install mysql-server - Comando para realizar a instação do MySQL

1.2. service mysql status - Comando para verificar o status do servidor MySQL

1.2.1 sudo service mysql stop - Comando para parar o servidor MySQL. "start" para iniciar e "restart" para reiniciar

1.3. mysql --version - Comando para verificar a versão do mysql

2. # Configurando o MySQL #

2.1 sudo mysql_secure_installation - Altera configurações de segurança

2.3 ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password'; - Comando para altar o usário root, faz com que solicite uma senha de autenticação. No 'password' é o local onde ESCREVEMOS a SENHA DESEJADA.

2.4 CREATE USER <nome usuario>@'<acesso ex.: localhost>' IDENTIFIED BY '<senha>'; - Comando para criar um usuário no banco de dados MySQL.

2.5 GRANT ALL PRIVILEGES ON <nome do banco de dados>.* TO <nome do usuário>@'%'; - Comando para liberar acesso ao usuário, no caso "*" acesso completo e "%" qualquer host.

3 # Comando de execução #

3.1 sudo mysql - Para executar o MySQL em linhas de código, caso a senho do usuário já estiver cadastrada trabalhar com os parametros -u<nome do usuário> -p ENTER para digitar a senha do usuário.


## Instalando a MySQL Workbench ##

1. # Instalação por linhas de comandos #

1.1 Roda UPDATE para verificar os pacotes do Linux.

1.2 wget <url> - Para baixar o programa da internet por linhas de código. É necessário saber o url completa

1.3 sudo dpkg -i <nome do arquivo a ser instalado>.

1.3.1 sudo apt-get -f install - Para instalar a dependencias.

1.4 /usr/bin/mysql-workbench - Para abrir o sistema

1.5 Para o WorkBench conectar no MySQL server é necessário rodar os dois comandos abaixo:

	sudo snap connect mysql-workbench-community:ssh-keys
	sudo snap connect mysql-workbench-community:password-manager-service


## Comandos do Python ##

1. Necessário instalar o gerenciador de pacotes PIP comando: sudo apt install python3-pip
2. Necessário instalar Python3-venv comando: apt install python3.10-venv e sudo pip3 install virtualenv
3. Rodar comando de criação da pasta venv: python3 -m venv ./venv #nome dado ao arquivo
4. Rodar comando para ativar o ambiente virtual: surce <caminho da pasta (opcional)> venv/bin/activate
5. Para desativar o ambiente virtual: deactivate


## Gera chave ssh
 Comando 
```
ssh -keygen -f ~/.ssh/<nome desejado> -t <OPICIONAL especificação da chave> -b 4096 //Verificar parametros no menu help "ssh-keygen --help"
```
