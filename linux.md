# Comandos do Terminal Linux

## Comandos Servidor Linux

- Comando para exibir os números de IP, o IP estará na propriedade "enp0s3" no "inet"
```
$ ip a
```

- Para acessar uma maquina virtual do via Linux/MacSO seguir da seguinte forma:
```
$ ssh <usuário da máquina>@<número de IP>
```
-  Máquina Virtual Pública AWS/Azure, baixar chave SSH extensão .pem
```
$ ssh -i <nome da chave.pem> <usuário da máquina>@<númedo de IP>
```
<sub>No Windowns uma das opções é usar o programa puTTY (necessário baixar e instalar).</sub>

- Comando para instalar serviço "OPEN SSH"
```
$ sudo apt-get install openssh-server
```

## Usuário Administrador e Grupos

- Verificação de grupos e usuários
```
$ cat /etc/group # Lista conteúdo do arquivo de grupos dos sistema
$ cat /etc/sudoers # Contém as especificações para utilizar o comando "sudo"
```
Para ser admin é necessário estar nos grupos "admin" e "sudo"

- **Administração de usuários**
```
$ sudo passwd <nome do usuário> # Atribui senha ao usuário existente
```

- Arquivo SSHD 
É o portal de acesso ao sistema via SSH, guarda o log de todas as tentativas e acessos realizados no sistema

-- Para permitir relizar login via usuário root é necessário alterar o arquivo sshd_config na linha **PermitRootLogin** descomentar 
essa linha e trocar **prohibit-password** por **yes**, depois restart o sistema ou o serviço "sshd". Para realizar a alteração será necessário 
utilizar um editor de texto VIM ou NANO.
***Não é a melhor prática por questões de segurança***
```
$ cat /etc/ssh/sshd_config
$ systemctl status sshd # Verifica status do serviço SSHD
$ systemctl restart sshd
```

**Gestão de Usuários**

Esses comando funcionam também para o Desktop
Comando "useradd" não cria pasta HOME
- Comando para criar ou deletar usuário
***Consultar usuários cadastrado no sistema***
```
$ cat /etc/passwd
```

***Cria usuários***
Quando criamos o usuário, ao passar uma senha neste momento é necessário que estaja encripitada, caso contrário é melhor primeiro criar o usuário e depois criar a senha, para realiza a encripitação no mesmo comando de criação seguir conforme parâmetro "-p" abaixo
```
$ useradd <nome do usuário>
$ useradd <nome do usuário> -m -c "nome completo do usuário" -s /bin/bash -e 22/09/2022
	Parâmetros: (qualquer parâmetro pode ser alterado/incluido com uso do comando "usermod")
	-m # Para criar pasta home
	-c # Para incluir mensagem para o usuário, pode ser o nome completo por exemplo (-c "mensagem")
	-s # Para informar qual a shell que o usuário irá logar, o mais usuál é o BASH (-s /bin/bash)
	-e # Para determinar uma data limite de acesso (-e dd/mm/aaaa), quando não informado será necessário nova senha no momento do login
	-p $(openssl passwd -password senha123)
	-G <nome do grupo>
```
***Realiza alterações no usuário***
```
$ usermod <nome do usuário> <parâmetro que deseja alterar> # Exemplo: $ usermod guest -e 30/09/2022 # Comando para realizar alterações nos parâmetros de um usuário 

$ passwd <nome do usuário> # Comando para criar ou alterar senha de usuário, pode ser usado com os parâmtros:
	-e # podemos passar data para expirar a senha ou, se não informar a data será necessário uma nova senha nesse momento
```
***Exclui usuários***
```
$ userdel -f <nome do usuário> # -f é para forçar a exclusão
	Parâmetro:
	-f # Para forçar a exclusão
	-r # Exclui o usuário e seus mail e diretórios
```

- Comando para criar ou alterar senha, serve para resetar senha de ususário
```
$ passwd <nome do usuário>
```
- Para os usuário novos é necessário informa a shell ao qual terá acesso, caso não informado não terá acesso as pastas do sistema, pode ser informada no momento da criação utilizando o parâmetro "-s" ou pelo comando
Bash é a shell padrão
```
$ chsh -s /bin/bash <nome do usuário>
```

***Criar script de execução para criação de usuário***
Criamos na raiz do sistema uma pasta chamado **scripts** e dentro dela um arquivo, no exemplo, de nome "**create_user.sh**", importante ser com a extensão ".sh" e na primeira linha do arquivo o comando ```#!/bin/bash``` e no corpo do arquivo escrevemos os comandos da 
mesma maneira que seria escrito no terminal
Necessário conceder permissão de execução para o arquivo, com o comando, poderá observar que o arquivo irá trocar de cor
```
$ chmod +x <nome do arquivo>
```
Para executar o arquivo:
```
./<nome do arquivo>
```

***Grupo de usuários***

Para consultar os grupos existente
```
cat /etc/group
```
- Comando para adicionar o usuário em um grupo existente
```
$ usermod -g <nome do grupo> <nome do usuário>
	  -G <nome do grupo>,<nome do grupo> <nome do usuário> 
```
Usando -g ou -G o usuário é migrado de grupo, ou seja, sai dos grupos que já estavam e passa para o grupo indicado no comando, para 
manter no mesmo grupos anteriores é necessário indicar os grupos que ela deve pertencer
Agora para excluir de apenas um grupo o comando é
```
gpasswd -d <nome do usuário> <nome do grupo que será removido>
```

- Criar Grupo
Grupos servem como uma forma de criar permissão de acesso, assim todos os usuários constante no grupo herdam as permissões que o grupo tem, é uma forma mais fácil de gerenciar permissões.
Comando para alterar os grupos dos usuário ```usermod -G ou -g``` é necessário **listar todos os grupos que o usuário deva pertencer**, 
comando para excluir um grupo do usuário é o ```gpasswd -d```.
```
groupadd <nome do grupo>
groupdel <nome do grupo>
usermod -G <nome do grupo> <nome do usuário>
gpasswd -d <nome do usuário> <nome do grupo que será removido>
```

### Permissões

Cada "elemento" dentro do linux quando observador de forma detalhada ```ls -l```  possuí uns caracteres iniciais que podem iniciar com **d** - 
quando se 
trata de um diretório, ou "-" - quando se trata de um arquivo, os demais caratéres são divididos em 3 colunas (dividido em grupos de 3 
caracteres) que diz respeito sobre as permissões de **"Dono", "Grupo","Outros"**, respectivamente, onde:
 - r read/leitura
 - w write/gravação
 - x execute/execução
Os valores de leitura, gravação e execução possuem um valor que os representa de forma numérica sendo:
 - Leitura(R) => 4
 - Gravação(W) => 2
 - Execução(X) => 1
 - Nenhum => 0
A utilização desses valores é por meio de soma, exemplo para dar permissão de leitura/gravação/execução informa 
o valor "7"

A representação de uma pasta e/ou diretório no Linux é da seguinte forma:
permissões	qtde arquivos	user dono	grupo	tamanho		data		diretório/arquivo
drwxr-xr-x	1		root     	root    7 		set 23 14:42 	adm

root é o usuário super usuário do sistema. Podemos altera o dono e/ou grupo de um arquivo/diretório com o comando ```chown``` change owner
```
chown <nome do novo usuário dono>:<nome do grupo> /adm/ #Caminho do diretório/arquivo no caso /adm/
```
Com essa ação a nova representação fica:
	DE
	drwxr-xr-x   2 root root       4096 set 23 15:10 adm
	PARA
	drwxr-xr-x   2 venancio GRP_ADM       4096 set 23 15:10 adm
Assim, neste exemplo, o usuário **venancio** dono do diretório **adm**, pode ler/gravar/exeturar ```rwx```, e os usuário pertecentes ao grupo **GRP_ADM** podem ler/executar ```r_x``` e os outros podem ler/executar também ```r_x```

Comando para modificar permissões do grupo ```chmod``` na sequência informamos os valores de permissão para o 
"dono", "grupo" e "outros", respectivamente. Pode alterar as permissões tanto de arquivos como de diretórios
```
chmod 750 /adm/ # No exemplo o diretório foi o /adm/ criado anteriormente para esse testeA
```
Caso crie um arquivo de execução e queira das permissão de execução **apenas para o dono** do arquivo, o comando é
```
chomd +x <nome do arquivo>
chomd -x <nome do arquivo>
```

## Editor de Texto VIM e NANO
**Vim**
Para entrar no modo de inserção uma das alternativas é aperta a leta "i", quando finalizar aperta "esc" os comandos abaixo
```
$ :wq # Salva e fecha o arquivo
$ :q! # Fecha sem salvar
```

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
### variáveis do comando ls
<sub>
	(exemplo de arquivo nomeado como arquivo1.txt, arquivo2.txt ...)
	conforme exemplo acima os parâmetros abaixo são usados para
</sub>

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

### Demais comandos interessantes ###
```
- Comando para realizar Downloads ```wget```
- Comando para descompactar arquivo ```unzip``` ou ```unrar```


## Gerenciamento de Pacotes

* Gerenciador de pacote do Debian / Ubuntu
- **apt-get** maiores detalhes ```apt-get --help```
- **apt** - Mais amigável ao usuário, maiores detalhes ```apt --help```

* Gerenciador de pacote do Red Hat / Fedora / CentOS
- **yum**
- **dnf**

**Alguns exemplos de comandos**
```
$ apt list # Lista todos os pacotes disponíveis para baixar
$ apt list --installed # Lista todos os pacotes instalados na máquina
$ apt list --upgradeable # Lista se existem versões a serem atualizadas dos pacotes já instalados.
$ apt search <nome do pacote> # Busca de pacotes no gerenciador
$ apt remove <nome do pacote> # Para remover pacotes
	-y # Parâmetro para sem pedir confirmação, ideal para uso dentro de "script"
$ apt update
$ apt upgrade
```
**Curiosidade** Com o uso da virtualização pelo Oracle Virtual Box, é possível criar uma 
scrinshots antes de realização de qualquer modificação, exemplo um script de configuração, 
uma atualização do sistema, isso irá funcionar como um backup, caso ocorra alguma 
problemas, basta restaurar a versão desejada.


**Adicionar repositório de terceiros**
Comando para verificar os links de repositórios do Ubuntu, caso queira adicionar repositório de terceiros, basta incluir o link aqui
```
$ apt edit-sources
```

**Instalação de programas .deb**
- Pode utilizar o **apt** para instalação ```apt install ./<nome do arquivo com 
extensão>```, necessário navegar até a pasta onde o arquivo está guardado, para 
para baixar o arquivo utiliza-se o comando ```wget <endereço para download>```
- Comando para instalação de arquivos ```dpkg -i <nome do arquivo>```


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
