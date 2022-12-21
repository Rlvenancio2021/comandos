# Jenkins

## Índice

- [Instalar no GCP](#instalar-no-gcp)
- [Configurando Jenkins](#configurando-jenkins)
- [Jobs](#jobs)

Por se tratar de um servidor, tudo que desejar utilizar precisa ser instalado, a responsabilidade de manutenção é do usuário, inclusive realização de Backups, diferente do *Cloud Build* que todo gerenciamento é realizado pela Google e o usuário apenas utiliza.

Identificar erros mais cedo, builds em diversos ambientes, testes automatizados, fácil operação e configuração, análise de código.

**Documentação**
site: https://www.jenkins.io/doc/

Ferramento Open Source escrito em JAVA. Realiza as mesmas coisas que o Cloud Build, porém é necessário **instalar um servidor Jenkins** na máquina, a partir daí, é possível utilizar, roda em núvem e local.

## Instalar no GCP

Pesquisar por **Jenkins** no Marketplace
- No caso de máquinas virtuais escolher a versão *Google Click to Deploy - Máquinas Virtuais*.
- Em **Allow HTTP traffic from the internet e Allow  HTTPS traffic from the interne** habilitar "0.0.0.0/0" (full) para que possa ser acessado de qualquer endereço de IP ou se tiver um IP conhecido pode mencionar aqui. Habilitação Full não é ideal na produção

Após conclusão, navegar no jenkins (em **Deployment Manager**) e localizar o link de acesso ao servidor, usuário e senha, ou verificar a Instância VM criada para verificar o IP da máquina.

Jenkins funciona com **Plugins (Extensões)** na opção **Gerenciamento Jenkins** terá acesso ao gerenciamento de plugins

## Configurando Jenkins

- Instalar plugin do **Git** para iniciar os processo a cada commit
- Instalar os plugins que forem necessários para executar a operação desejada


## Jobs

Um exemplo de criação de um Job, é um para executar um comando "ls", onde em **build / Executar Shell** podemos escrever o comando "ls". Aqui também estarão as **variáveis de ambiente** disponíveis do sistema.

Para usar Docker.

Instalar os plugins no Jenkins
- Docker API Plugin
- Docker Commons Plugin
- Docker Pipeline
- Docker-build-step

Acessar o servicor via SSH e instalar o Docker
- https://docs.docker.com/engine/install/debian/

Configurar no Jenkins a conexão com Docker
 - Gerenciar Jenkins / Configuração do Sistema
 - Na opção **Gerenciar Jenkins / Gerenciar nós / Configurar nuvens** em **Docker Host URI** escrever "unix:///var/run/docker.sock" e testar conexão.
 - Em Configuração de Sistema **Docker Builder** escrever "unix:///var/run/docker.sock" e testar conexão.

Necessário incluir da permissão de execução ao Docker para o usuário
```
$ sudo usemod -aG docker $USER
$ sudo systemctl start docker
```
Testar com comando *docker ps* caso ainda não funcionar executar
```
$ sudo chmod a+rwx /var/run/docker.sock
$ sudo chmod a+rwx /var/run/docker.pid
```

Repositório para Deploy da Aplicação
https://github.com/digitalinnovationone/python-flask

No Jenkins
 - Novo Job, nomear e selecionar uma das opções desejadas, no exemplo *Construindo um projeto de software de estile livre*
 - Escrever *Descrição*
 - **Gerenciamento de código fonte** escolher *git*, e colar o link do repositório .git, informar a **Branch** desejada.
 - **Build** para o exemplo escoler *Execute Docker command* e *Create/build image* 
 - Escolher diretório onde será criada a image, **manter** a variável **$WORKSPACE/** indicar o diretório na frente, no exemplo será utilizado o diretório Raiz
 - Clicar em **Construir Agora**
*Obs* podemos adicionar quantos passos forem necessários em **Build**, basta indica-los na sequência que será executado.
