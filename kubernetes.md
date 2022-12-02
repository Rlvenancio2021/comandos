# Kubernetes

### O que é o Kubernetes

O Kubernetes (K8s) é uma ferramente (open source) de orquestração de containers originalmente desenvolvida pelo Google.

É um produto Open Source utlizados para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêiner.

Irá ajudar a organizar e administrar aplicações em containers em ambientes onde existem dezenas e até milhares de containers. As aplicações podem estar em diferentes ambientes de implementação:

- Infraestrutura local
- Máquinas virtuais
- Cloud Pública
- Cloud Híbrida

Docker Swarm é similar ao Kubernetes

**Qual a necessidade de uma ferramenta de orquestração de containers**

- Migração de aplicações monolíticas para microsserviços;
- Disponibilidade de aplicação (diminuição de downtime);
- Escalibilidade e alta performance;
Recuperação de desastre (Backup/Restore).

**Como utilizar o Kubernetes**

1. Criar um cluster Kubernetes
2. Implantar um aplicativo
  # POD
 - Menor unidade do Kubernetes;
 - Uma abstração sobre o container;
 - Normalmente é executado uma aplicação por Pod;
  - Por é na verdadde a aplicação.
3. Explore seu aplicativo
4. Exponha seu aplicativo  publicamente
5. Escale seu aplicativo
6. Atualiza seu aplicativo

### O que é um cluster Kubernetes

Um Kubernetes é um conjunto de mós que executam aplicativos em contêineres.

Kubernetes cluster é um conjunto de máquinas usadas para executar aplicações em contêiners. Quando você executa o Kubernetes, está executando um cluster.

No mínimo, um cluster contém um plano de controle e pelo menos uma máquina ou nó.

No servidor Master (Servidor de processamento) é **instalado o "Control Plane"** é uma séria de aplicações utilizadas para controlar o **cluster**
Se necessário instalar um novo nós, será por intermédio do servidor master.
É onde está a API que gerencia o cluster.
**Risco**, como é o servidor que controle todo o cluster, caso o perca, todo o cluster será perdido, neste caso, **é recomendado que tenha mais de um servidor com o "Control Plane" instalado**.

#### Componentes do Kubernetes

- O **Servidor de Processamento (worker)** hospeda os Pods que são componentes de uma aplicação;
- O **ambiente de Gerenciamento** gerencia os nós de processamento e os Pods no cluster
- Um **Ambiente de Produção**, o ambiente de gerenciamento é **geralmente executado em múltiplos computadores**, provendo tolerância a falhas e alta disponibilidade.

**Principais componentes da camada de gerenciamento**
- kube-apiserver = Porta de entrada para qualquer serviço a ser demandado pelo cluster
- etcd = É um armazenamento de valor em cluster, é o principal armazenamento, caso perdido, perde-se tudo
- kube-sheduler = É quem faz a escala do projeto
- kube-controller-manager = Loop que observa o estado de cada em dos componetes de um cluster.

**Administração da camada de gerenciamento**
Em caso de utilização de maneira manual de um servidor. São as 3 principais ferramentas a serem instalados no nó gerenciador.
*Obs* para o serviço em núvem, não é necessário se preocupar, pois já estará instalado.
 - **kubeadm** - o comando para criar o cluster
 - **kubelet** - o componente que executa em todas as máquinas no seu cluster e cuida de tarefas como a inicialização de pods e contêineres
 - **kubectl** - a ferramenta de linha de comando para interação com o cluster.

**Seriços de núvem que podem ser utlizado**
 - Amazon EKS - Necessário apenas está instalado o "kubectl" na máquina local.
    - Provisiona um cluster EKS;
    - Especifica os nós desejados (Deploy compute);
    - Conecta com o serviço EKS;
    - Roda as aplicações.
 - GCP Kubernetes Engine (GKE) - Google
 - Azure Kubernetes Serivce (AKS) - Microsoft

### O que é um Pod

É um conjunto de um ou mais contêineres, sendo a menor unidade de uma aplicação Kubernetes. Os pods são compostos por um contêiner nos casos de uso mais comuns ou por vários contêineres fortemente acoplados em cenários mais avançados. Os contêinres são agrupados nesses pods para que os recursos sejam compartilhados de modo mais inteligente.

Um Pod é um grupo de um ou mais contêineres de aplicativos (como Docker) que inclui armazenamento compartilhado (volumes), endereço IP e informações sobre como executá-los.

### Resumo

1. Cluster é um conjunto de nós
2. Em cada nó cria um Pod (para armazenar as aplicações Containerizadas)
3. Em cada Pod, roda um container com a aplicação (o ideal é que cada containder roda uma aplicação).

### Ferramenta

**Minikube** 
**Pré-requisitos para utilizar o Minikupe**

É um utilitário que pode user para executar o Kubernetes(K8s) em máquina local. Ele cria um cluster de nó único contido em uma máquina virtual . Esse cluster permite que execute e estude o Kubernetes sem exigir a instalação completa do Kunernetes.

- Necessário ter um Virtualizador instalado no máquina, como por exemplo VirtualBox.
- 2 ou mais Núcloe de processamento disponível (CPUs)
- 2GB de memória RAM
- 20GB de espaço em disco
- Conexão com a internet

**site** 

- kubernetes.io
- minikube.sigs.k8s.io/docs/start/

## Instalação no Ubuntu

**Minikube**
```
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
$ sudo dpkg -i minikube_latest_amd64.deb
```

**Kubectl**

Instalação padrão

1. Atualiza e instalar os pacotes necessários para usar o apt repositório do Kubernetes.
```
$ sudo apt-get update
$ sudo apt-get install -y ca-certificates curl
```
Verificar necessidade de instalar https
```
$ sudo apt-get install -y apt-transport-https
```
2. Download de chave de assinatura pública do Google Cloud
```
$ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```
3. Adicione o repositório do Kubernetes apt
```
$ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
4. Atualiza e instala pacotes com o novo repositório e instala o kubectl
```
$ sudo apt-get update
$ sudo apt-get install -y kubectl
```

**Arquivo padrão de configuração**
```
$ cat ~/.kube/config
```
Só apresenta dados quando o cluster está em execução.

## Comandos Minikube

Para iniciar a utilização executar o comando
```
$ minibuke start
$ minikube ssh
```

**Caso apresente erro**

Exiting due to GUEST_PROVISION: Failed to validate network: dial tcp 192.168.59.107:22: i/o timeout

Podemos proceder com o seguinte

Alterar nome do arquivo "networks.conf" para **"networks.conf.bak"**
Arquivo networks.conf, foi criado para possibilidade criar IP fixo para iniciar cluster com vagrant
```
$ mv /etc/vbox/networks.conf /etc/box/networks.conf.bak
$ minikube delete --all --purge
$ minikube start
```

- Comando para verificar os nós do cluster
```
$ kubectl get node
```

## Comandos Kubectl

- Comandos para consulta
```
$ kubectl get pods
$ kubectl get services
$ kubectl get nodes
$ kubectl get deploy
$ kubectl get pv     # Referente a Volumes
```

- Comando para verificar IP dos nós do cluster
```
$ kubectl get nodes -o wide
$ kubectl describe service <NOME DO SERVIÇO>
```
- Comando delete para **deletar** um **Serviços, Pod, Deployment**
```
$ kubectl delete <IDENTIFICAÇÃO CONFORME O KIND DO ARQUIVO .yml> <NOME>
```

- Comando para copiar arquivo para um nó
Pegar o nome, listando os Pods que estão rodando, quando replicado, cada nó terá um nome.
```
NAME                     READY   STATUS    RESTARTS   AGE    IP           NODE       NOMINATED NODE   READINESS GATES
mysql-5fd4977b45-rw7ml   1/1     Running   0          120m   172.17.0.2   minikube   <none>           <none>
```
```
$ kubectl cp <NOME DO ARQUIVO> <NOME>:<CAMINHO ONDE SALVAR O ARQUIVO>
$ kubectl cp mysql-5fd4977b45-rw7ml:/var/lib/mysql
```

## Provisão em núvem

## AWS

*Para saber o custo* de utilizar o *site* https://calculator.aws/#/
Serviços a serem utlizados:
  EKS cluster
  EC2 nós considerando os requisitos mínimos que o kubernetes precisa

## Provisionamento de servidores na **AWS**

Instalar a Interface de Linha de Comando - CLI da AWS
*site* https://aws.amazon.com/pt/cli/
```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip
$ sudo ./aws/install
```

Gerar um certificado (IAM) da AWS
 - Pesquisa na página por IAM e clicar no link **my securit credentials**;
 - Access keys (access key ID and securit access key)

 No CLI executar
```
$  aws configure
```
*OBS* não é necessário preencher **region name** e **output format**.
Com isso podemos gerenciar o servidor, via linha de comando.

### Criando cluster EKS

Via acesso ao site

**Atenção** para espeficificar a região onde será criado o cluster, informação no canto superior direito da tela.

***Principais pontos de atenção***

- Criar uma política de EKS - **Cluster Service Role**
Abrir uma nova aba, no console da AWS > IAM > Roles > Create role;
Em *"Use cases for other AWS services"* pesquisar por EKS > selecionar EKS-Cluster.
Segue passos padrões.

- Cluster endpoint access
Caso tenho acesso externo (exemplo via comandos CLI de uma máquina local) necessário escolher uma opção com acesso **Público**.

- Configure logging
O mais interessante é o **Scheduler** que informará se houve aumento ou diminuição dos nós do cluster

Via CLI
- Verificar status de criação
```
$ aws eks --region sa-east-1 describe-cluster --name <NOME DADO AO CLUSTER> --query cluster.status
```

*OBS* em caso de utilização do kubectl e o mesmo gerar conflito com o local e a núvem, renomear o arquvio "~/.kube/config" para "~/.kube/config.bak"

Baixar as especificação do servidor em nuvem para a máquina local (~/.kube/config)
```
$ aws eks --region sa-east-1 update-kubeconfig --name <NOME DADO AO CLUSTER>
```
Desta forma será possível utilizar o comando
```
$ kubectl get svc
```

### Criando nós para o Cluster

Na página do cluster ir na opção **Compute** > Add node group

***Principais pontos de atenção***
- Criar uma política do nó **Node IAM role**
Nova ABA > IAM > Create Role > **Selecionar EC2** > inserir 3 regras básicas
1. AmazonEKS_CNI_Policy
2. AmzonEKSWorkerNodePolicy
3. AmazonEC@ContainerRegistryReadOnly

- Em *Instance types* selecionar o tipo de instância desejada (para estudo pode ser a configuração minima)
Podemos acompanhar a evolução por comando CLI em tempo real.
```
$ kubectl get nodes --watch
```

### Parar os serviços na Núvem
1. Excluir os nós
2. Excluir o cluster

## FIM AWS

##  GCP (Google)

*site* https://console.cloud.google.com/

Pesquisar por Kubernetes Engine ou GKE
Existe a possibilidade de **criar um projeto** para trabalhar com a ferramenta.
- Clicar em **Criar cluster**
Para se ter melhor controle a opção é Standard

***Principais pontos de atenção***
- Prestar atenção no **POOLS DE NÓS**
Será criado um por padrão, onde pode-se gerenciar **Nós, Rede, Segurança, Metadados**

**Necessário instalar a CLI gcloud**
*site* https://cloud.google.com/sdk/docs/install#deb
Pré requisito
```
$ sudo apt-get install apt-transport-https ca-certificates gnupg
```
Instalação
```
$ echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

$ echo "deb https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```
Importar chave pública do Google Cloud
```
 # se a distribuição for compatível com o argumento "--keyring"
$ curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

 # se não for compatível
$ curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

 # Se não for compatível com o comando "apt-key"
$ curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo tee /usr/share/keyrings/cloud.google.gpg
```
Atualize e instale a CLI gcloud
```
$ sudo apt-get update && sudo apt-get install google-cloud-cli

 # se estiver instalando dentro de uma imagem docker
$ RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg  add - && apt-get update -y && apt-get install google-cloud-cli -y

 # se o comando "apt-key" não for compatível
$ RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | tee /usr/share/keyrings/cloud.google.gpg && apt-get update -y && apt-get install google-cloud-sdk -y
```
Instalar o "kubectl" ou qualquer outro pacote listado no site

Para iniciar executar
```
$ gcloud init
```

No Windows (usando Terminal ou Power Shell) é necessário é necessário
```
$ Install-Module GoogleCloud
$ Set-ExecutionPolicy RemoteSigned # para comando de script remote
$ gcloud components install gke-gcloud-auth-plugin
```
```
$ gcloud auth login
$ gcloud auth list
```

Para conectar a máquina, no site na linha do cluster criar, encontre a opção **Conectar** copie o comando e cole no terminal.
Comandos
```
$ kubectl get nodes
```

### Comandos do Gcloud GCP

- Necessário inicialmente rodar o comando
```
$ gcloud config set project PROJECT_ID
```

- Comando para verificar os IPs dos nós do cluster
```
$ gcloud compute instances list
```
- Comando para firewall
```
$ gcloud compute firewall-rules create <NOME DA REGRA> --allow tcp:<NÚMEDO DA PORTA>
$ gcloud compute firewall-rules list
```

## Fim GCP

### Local

- Comando para saber status de cluste
```
$ kubectl get svc
```

## Arquivo YAML

O **YAML** é uma linguagem de serialização de dados muito usada na escrita de arquivos de configuração.
O YAML usa um recuo no estilo Python para indicar o ainhamento. É necessário utilizar espaços em brancho porque os caractesres de tabulação não são permitidos. Não há símbolos de formato comuns, como chaves, colchetes, tags de fechamento ou aspas. Os arquivos YAML têm a **extensão .yml ou .yaml**.

**Exemplo**

```
apiVersion: v1
kind: Pod
metadata:
 name: app-html
 labels:
  app: app-html
spec:
 containers:
 - name: app-html
  image: denilsonbonatti/app-html:1.0
  ports:
  - containerPort: 80
```
1. Indica a versão da API que será utilizada
2. Indica o tipo de objeto que está sendo criado
3. Informações do objeto **metadata**
4. Configuração do(s) container(s) **spec** (tendo mais de um, relacionar todos aqui)
    - Image: referente a imagem do **Docker**

O exemplo acima é para um caso simples, para um caso com repplicadas existe um diferença. Verificar no site do **Kubernetes > Workloads**.

```
apiVersion: apps/v1 
kind: Delpoyment 
 metadata: 
  name: app-html-deployment 
  labels: 
   app: app-html 
 spec: 
  replicas: 3 
  selector: 
   matchLabels: 
    app: app-html 
  template: 
   metadata: 
    labels: 
     app: app-html 
   spec: 
    containers: 
    - name: app-html 
      image: httpd:latest 
      ports: 
      - containerPort: 80
```

**Escalar o deployment** alterando ou não o arquivo de configuração

Uma maneira de, por exemplo, **modificar a quantidade de replicas** de um cluster em execução, é alterar o valor no arquivo YAML desejado e rodar o comando **Apply** novamente.

Ou podemos rodar o comando:
```
$ kubectl scale deployment app-html-deployment --replicas=10
$ kubectl scale deployment app-html-deployment --help # para ver mais opções
```

### Definir versão

**Pod -->> "v1"
Deployment -->> "apps/v1"
Service -->> "v1"**

## Provisionando um Objeto

1. Criar o arquivo **YAML** com extensão ".yml" ou ".yaml"
2. Inicia o cluster Kubernetes
3. Executa o comando **kubectl apply -f <NOME DO ARQUIVO conforme item 1>.**
4. Veriicar o status do Objeto criado, se for um Pod o comando é **kubectl get pods**

## Comandos do dia-a-dia do KUBECTL

- Iniciar o cluster no caso de está rodando local usando um virtualizados (VirtualBox)
```
$ minikube start
```

- Comandos de verificação
Basico
```
$ kubectl get <OPÇÃO DESEJADA>
$ kubectl get <OPÇÃO DESEJADA> -o wide
$ kubectl describe <OPÇÃO DESEJADA> <NOME DO OBJETO>
```
Na prática
```
$ kubectl get services
$ kubectl get pods
$                 -o wide # Maior detalhe do pode
$ kubectl get nodes
$ kubectl describe node minikube
$ kubectl get deployment
& kubectl describe deployment app-html-deployment
```

- Carregar e apagar um Pod
```
$ kubectl apply -f <NOME DO ARQUIVO COM A EXTENSÃO .yml>
$ kubectl delete pod app-html  # Nome dado ao pod conforme arquivo .yml
```

## Exposição da aplicação

A expondo o cluster, fica acessível para consulta de qualquer usuário, assim, quando realizada uma consulta, é feita diretamente para o master e esse irá executar em qualquer um dos nós.

```
$ kubectl expose deployment app-html-deployment --type=LoadBalancer --name=app-html --port=80
```
Quando utilizado dentro do minikube, podemos executar o comando abaixo para verificar o endereço de acesso
```
$ minikube service --url app-html
```

## Criando uma imagem Docker e Expondo a Aplicação

Serão criados diretório específicos para melhor organização do projeto

***Criar a imagem Docker***

1. Para melhor organização, criar os diretórios necessários para o projeto
 - Neste exemplo ~/Kubernetes/app-2-kubernetes/dockerfile

2. Criar a Aplicação, Site, Banco de Dados, etc...

3. Criar a imagem do conteiner para rodar o aplicativo
 - Criar arquivo dockerfile
 - Consultar documentação da imagem que deseja criar no hub.docker.com, para verifciar as expecificações necessárias.

4. Dendro do diretório que está **gravado o dockerfile** executar o comando
```
$ docker bluid . -t <NOME DO USUÁRIO DO HUB.DOCKER>/<NOME DA APLICAÇÃO>:<TAG>
```

5. Consultar se a imagem foi gerada com sucesso
```
$ docker images
```

*Para enviar a imagem para o repositório oficial*

6. Logar do Docker via CLI
```
$ docker login
```

7. Realizar Upload da imagem para o repositório
```
$ docker push <NOME DA IMAGEM CONFORME ITEM 4 incluindo a TAG>
```

***Expondo a aplicação***

8. Cria o **arquivo YAML** no diretório específico
 - Neste caso ~/Kubernetes/app-2-kubernetes/app1
 - Nome dado ao arquivo "app-deployment.yml"

9. Rodar o **comando apply**
```
$ kubectl apply -f ./app-deployment.yml
```
 - Desta forma a aplicação já está **rodando no Cluster**

10. Expor o deplyment
```
$ kubectl expose deployment html-deployment --type=LoadBalancer --name=app2-html --port=80
```
 - Para pegar o endereço da URL para consultar ao serviço rodar o comando
 ```
 $ kubectl service
 ```

 - Se estiver rodando em máquina local via virtualizador, para pegar o endereço rodar comando
 ```
 $ minikube service --url app2-html
 ```

 ***Atualizando a Aplicação***

1. Realiza a atualização na aplicação conforme desejado
  - Seja site, API, etc...

2. Cria um novo diretório e copia o arquivo YAML
 - Neste caso .../app1.2
3. Roda o comando de construção de uma imagem Docker alterando a sua **TAG**

4. Carrega a nova imagem para o Hub Docker

5. No arquivo YAML conforme o item 2.
 - Atualiza a variávem **image** para a nova versão da aplicação.

6. Rodar comando para realizar **Apply**
 - Dentro dentro do diretório ./app1.2, conforme estrutuda desenhada


### LoadBalancer

Existem duas maneiras de realizar o LoadBalancer.

1. No momento de expor a aplicação por meio do comando
```
$ kubectl expose deployment html-deployment --type=LoadBalancer --name=app2-html --port=80
```
Podemos excluir o LoadBalancer excluindo o serviço pelo comando
```
$ kubectl delete service app2-html
```

2. Por meio do arquivo YAML

 2.1. Cria um arquivo YAML
 ```
apiVersion: v1
kind: Service
metadata:
 name: app-html-lb
spec:
 selector:
  app: app-html
 ports:
  - port: 80
    targetPort: 80
 type: LoadBalancer
 ```
*OBS* 
 - em "selector>app" indica o nome dado ao APP conforme consta no arquivo criado "app.deployment.yml"
 - "targetPort" é a porta do Load Balancer

 2.2. Roda comando Apply, porém indica o arquivo YAML, criado com o serviço do tipo Load Balancer.


## Criando um NodePort

### Na Núvem

Serve para casos em que não se deseja criar um Load Balancer, mas deixar um acesso direcionado como por exemplo no caso de um banco de dados

Exemplo de uma aplicação diretório "app-3-kubernetes"


Arquivo **nodePort.yml** a ser criado
```
apiVersion: v1
kind: Service
metadata:
  name: myapp-php-service
spec:
  type: NodePort
  selector:
    app: myapp-php
  ports:
    - port: 80
      targetPort: 80
      #nodePort: 30007
```
*OBS*
 - targetPort é a porta alvo do conteiner, ou seja precisa ser a mesma portar específicado no arquivo do serviço.
 - nodePort é a porta padrão de acesso a esse serviço, deve ser acima de 30000. Se esse parâmetro não for adicionado, o kubernetes irá gerar uma porta aleatória

1. Rodar o **Apply do arquivo pod.yml** para criar o conteiner


### Local

Usando o mesmo arquivo YAML criado para o serviço em núvem

Rodar o comando Apply
```
$ kubectl apply -f <NOME DO ARQUIVO QUE CRIA O CLUSTER>
```

Expor para acesso externo
```
$ kubectl apply -f <NOME DO ARQUIVO QUE CRIA O NODEPORT>
```


## Executando aplicações no Pod

### Acessando POD diretamente

- Acesso ao container, para o caso em que deseja realizar uma alteração direto no arquivo, sem a necessidade de gerar um deploy, porém caso realize o deploy novamente, as alterações realizadas aqui serão perdidas
```
$ kubectl exec --stdin --tty myapp-php -- /bin/bash
```

### Criaando um POD de Banco de dados e criando porta de acesso

Para criar um POD de banco de dados o arquivo YAML fica preenchido
```
apiVersion: v1
kind: Pod
metadata:
 name: mysql-pod
 labels:
  name: mysql-pod
spec:
 containers:
  - name: mysql
    image: mysql:latest
    env:
      - name: "MYSQL_DATABASE"
        value: "meuBanco"
      - name: "MYSQL_ROOT_PASSWORD"
        value: "Senha123" # Não é recomendável difinir a senha desta forma
    ports:
      - containerPort: 3306
```

**Criando porta de acesso**
Caso não tenha sido criado um serviço para esse Pod
```
$ kubectl port-forward pod/mysql-pod 3306:3306
```

### Criando Estrutura completa 
Arquivos base ./app-5-kubernetes

**Database**

Dentro de **DockerFile** na linha **ADD sql.sql /docker-entrypoint-initdb.d**, cria um entrypoint executável para um arquivo externo, onde existem os comandos para criar uma tabela.

Estando no diretório onde está gravador o arquivo dockerfile, rodar comando para criar a imagem docker
```
$ docker build . -t <NOMEUSUÁRIO>/<NOME DA IMAGEM>:<TAG>
```
Carregar a imagem para o diretório
```
$ docker push <NOMEUSUÁRIO>/<NOME DA IMAGEM>:<TAG>
```

Dentro do **db-deployment.yml** 
Neste exemplo, ao criar o Pod do Banco de Dados, não está sendo específicado um volume, ou seja, ao excluir o Pod irá perder os dados do Banco, por esse motivo não a réplicas para o Banco de Dados
Na linha **imagePullPolicy: Always**, quer dizer que sempre irá checar se a imagens que está no Docker Hub é igual a que está no Cluster, caso a imagem do Docker Hub for mais nova. está será baixada.
O banco de dados ficará apenas no cluster, não será possível acesso externo, então será criado um serviço, para acessar apenas pelo BackEnd.
Carregar o deployment
```
$ kubectl apply -f <NOME DO ARQUIVO.yml>
```

**Backend**

Arquivo **conexao.php** é o arquivo que gera a conexão com o banco de dados
1. Criar e carrega a imagem Docker
2. Cria o Pod e/ou Serviço no cluster

**FronEnd**

Para criar a conexão com BackEnd, é necessário pegar o IP gerado para o nó e informa-lo no arquivo **js.js** na linha de **url**

## Persistência de Dados

O gerenciamento de armazenamento é uma questão bem diferente do gerenciamento de instâncias computacionais. O subsistema PersistentVolume provê uma API para usuários e administradores que mostra de forma detalhada como o armazenamento é provido e como ele é consumido. Para isso, o Kubernetes possui duas novas APIs: **PersistentVolume** e **PersistentVolumeClaim**.

A vantagem de criar um PersistentVolumeClain é que podemos usá-lo para vincular a outros Pods, sem a necessidade de criar um para cada, assim, todos estão vinculados ao mesmo banco de dados.

- PersistentVolume, irá especificar onde estará o volume de dados, ou seja, o ambiente físico que irá salvar as informações. Está no cluster, em alguns dos nós, está em um servidor externo, em um disco virtual, etc.
PVs são plugins de **volume**, porém, eles têm um ciclo de vida independente de qualquer Pod que utilize um PV. Essa API tem por objetivo mostrar os detalhes da implementação do armazenamento, seja ele NFS, iSCSI, ou um armazenamento específico de um provedor de cloud pública.

 - PersistenteVolumeClaim, é aqui que **vincula um Pod ou Deployment** com o PersistentVolume
 O PVC é uma requisição para armazenamento por um usuário. Claims podem solicitar ao PV tamanho e modos de acesso específicos. Uma reivindicação de volume persistente (PVC) é a solicitação de armazenamento, que é atendida vinculando a PVC a um volume persistente (PV).
 Neste caso **Usário** é um **Pod ou Deployment**

 - O Administrador do Cluster cria o PV
 - O Desenvolvedor cria o PVC

 *OBS* em um ambiente real, os dados devem está fora do ambiente do cluster. O cluster deve apenas gerenciar a aplicação.

Arquivo exemplo criado no diretório "~/Kubernetes/app-6-kubernetes"

Arquivo YAML
```
  1 apiVersion: apps/v1
  2 kind: Deployment
  3 metadata:
  4   name: mysql
  5 spec:
  6   selector:
  7     matchLabels:
  8       app: mysql
  9   template:
 10     metadata:
 11       labels:
 12         app: mysql
 13     spec:
 14       containers:
 15       - image: mysql:5.6
 16         name: mysql
 17         env:
 18           # Use secret in real usage
 19         - name: MYSQL_ROOT_PASSWORD
 20           value: Senha123
 21         - name: MYSQL_DATABASE
 22           value: meubanco
 23         ports:
 24         - containerPort: 3306
 25           name: mysql
 26
 27         volumeMounts:
 28         - name: local
 29           mountPath: /var/lib/mysql
 30       volumes:
 31       - name: local
 32         hostPath:
 33           path: /meubanco/
```
Para utilização com o PVC para realizar a **vinculação**. Após criado e carregado os arquivo YML do PV e PVC utilziar o **volumes** da seguinte forma:
```
 30       volumes:
 31       - name: local
 32         persistentVolumeCalim:
 33          claimName: local    # Aqui será inserido o mesmo nome dado ao PVC  
```

Em clusters em Núvem não é possível criar desta forma pois não tem permissão para criar diretórios no Cluster

**Sobre Volumes**

- ReadWriteOnce
Com está opção, por mais que se tenha mais de um nó no cluster e o deployment da aplicação for replicado, apenas nas réplicas que estiverem no nó que está executando o PVC irá ter acesso ao banco de dados, pois essa configuração é de leitura e escrita de apenas um nó. Não será possível escalar para mais nós, usando uma única base de dados.
- ReadOnlyMany
Neste opção o acesso será somente para leitura
- ReadWriteMany
Não compatível para GCP, neste caso é necessário criar um servidor NFS ou um disco que aceite esse recurso.

**Arquivos criados para PV e PVC**
No exemplo criamos os arquivo com nome de **pv.yml** e **pvc.yml** 
Carregamos ambos os arquivos com o comando **Apply** e para consultar
```
$ kubectl get pv
$ kubectl get pvc
```

### Persistência em núvem GCP

**Provisionar PersistentVolume dinamicamente**

*site* https://cloud.google.com/kubernetes-engine/docs/concepts/persistent-volumes

Procedimento é o mesmo, criar os arquivos yml para realizar deploy na núvem
Exemplo do arquivo na pasta app6 arquivo "mysql-deployment.yml"

Neste caso cria apenas PersistentVolumeClaim assim que automáticamente o Kubernentes faz o PersistentVolume

Após criar o arquivo o comando é o **APPLY**

**Para criação de um disco ou servidor NFS** que irá possibilitar escalar o banco de dados horizontal e verticalmente.
No caso de **servidor**, o acesso será por IP e nome de compartilhamento, que será configurado no PV.

No caso de **disco**, precisa se atentar ao local onde foi criado o cluster e a rede, para esteja acessível a rede.
O serviço a ser utilizado é o **filestore** no GCP
A região e rede tem que ser a mesma que o cluster foi criado
Pegar o número de IP gerado para acesso ao disco.
**Arquivo de referência "http-nfs.yml".**


## Gerenciamento de Deploy e Roolback em Clusters Kubernetes

Utilizando o comando **remove** mata todo a aplicação, se estiver em produção, isso fará com que fique fora do ar até que faça um novo **apply**.


Na procedimento padrão - para nova versão que será carregada, o sistema irá nó por nó, derrubar a versão que estiver rodando e carregar a nova versão, até que percorra todos os nós, assim, garantindo que a aplicação estaja no ar durante o processo.

Neste caso a **prática é informar no nome do arquivo .yml a versão dele**, assim com o comando **rollout history** será possível identificar na lista, qual o deploy desejado.

- Comando para criar um deploy criando um histórico
```
$ kubectl apply -f <NOME DO ARQUIVO .yml> --record
```

- Comando para voltar uma versão
```
$ kubectl rollout undo deployment httpd
$ kubectl rollout undo deployment httpd --to-revision=<NÚMERO DESEJADO>
```
***Obs*** para verificar os número de indice do **Revision** dor o comando para verificar o histórico

- Comando para verificar o histórico
```
$ kubectl rollout history deployment <NOME>
```

## Senhas dentro de um Cluster

**Secrets** é um objeto que contém uma pequena quantidade de informação sensível, como senhas, tokens ou chaves. Este tipo de informação poderia, em outras circunstâncias, ser colocada diretamente em uma configuração de Pod ou em uma imagem de contêiner. O uso de Secrets evita que você tenha de incluir dados confidenciais no seu código.
Podem ser criados de forma idenpendente dos Pods que os consomem. Isto reduz o risco de que o Secret e seus dados sejam expostos durante o processo de criação, visualização e edição ou atualização de Pods.

***Exemplo***
```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  ROOT_PASSWORD: Senha123
```
*OBS* 
 - O tipo **Opaque** significa que o usuário estará passando os valores.
 - Após "apply" deste arquivo, o mesmo pode ser excluído (ou armazenado em local segura, **não subir no GitHub**), desta forma não será exposto para o usuário. Para o desenvolvedor será passado a **chave** que ele precisa para realiza o acesso, no exemplo acima **ROOT_PASSWORD**, para utilizar a senha será necessário chamar pelo nome do sercret, neste caso **my-secret**.

 A mudança na utilização do secret é que no **".yml" do Banco de dados, nas variáveis de ambiente (ENV)** será apresentada da seguinte forma.
```
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: my-secret
                  key: ROOT_PASSWORD

            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: my-secret
                  key: MYSQL_DATABASE
```

- Comandos para secrets
```
$ kubectl get secrets                                           # Exibe os secrets criado
$ kubectl describe secrets <NOME DO SECRET>                     # Exibe a descrição do secret criado
$ kubectl edit secret <NOME DO SECRET>                          # Edita o secret criado
$ kubectl get secret <NOME DO SECRET> -o yaml                   # Exibe o conteúdo do secret criado
$ kubectl get secret <NOME DO SECRET> -o jsonpath='{.data}'     # Exibe o conteúdo do secret criado
```

- Para verificar os informações criadas via "Variável de Ambiente", após acesso o Pod desejado, rodar o comando.
```
$ printenv
```


# Google Cloud Deploy

É uma opção a utilização de ferramentas de CI/CD, isso ajudará a ter um controle melhor sobre o cluster e gerenciamento do Deploy.
Utilizando a ferramenta Skaffold por trás. Assim tem-se a garantia de que aquilo que foi homologado será exatamente o que será entregue no ambiente de produção.

**É um serviço gerenciado que automatiza a entrega de aplicações em uma séria de ambientes.**

site: https://cloud.google.com/deploy/docs/deploy-app-gke

Entregue continuamente ao Google Kubernetes Engine e ao Anthos.

Usado para situações que o **Cloud Build não atende**

**Faz a entrega da applicação no Kubernetes**


Ferramentas **Skaffold**
site: https://skaffold.dev/docs/

Ferramenta de linha de comando que facilita o processo de CI/CD dos conteiner, pode ser usado em núvem ou local

É responsável pela execução e implantação dos artefatos do cluster, identificando o **manifesto do Kubernetes** a ser usado para implantar a aplicação em questão.
O **skaffold lida com o fluxo de trabalho** para criar, enviar e implantar sua aplicação, **fornecendo blocos de construção para criar pipelines de CI/CD**.
Skaffold é uma **ferramento de linha de comando** que facilita o desenvolvimento contínuo de **aplicativos nativos do kubernetes**.

Tendo o gcloud instaldo, rodar o comando para instalar
```
$ gcloud components install skaffold
```
### Uso

- Entrar no console do GCP pesquisar por **Cloud Deploy** e ativar a API caso necessário

Adiciona o papel clouddeploy.jobRunner, para adicionar a permissão para uso do **Cloud Deploy**
```
$ gcloud projects add-iam-policy-binding PROJECT_ID \
    --member=serviceAccount:$(gcloud projects describe PROJECT_ID \
    --format="value(projectNumber)")-compute@developer.gserviceaccount.com \
    --role="roles/clouddeploy.jobRunner"
```

Adiciona o papel clouddeploy.jobRunner, para adicionar a permissão para uso do **Kubernetes**
```
$ gcloud projects add-iam-policy-binding PROJECT_ID \
    --member=serviceAccount:$(gcloud projects describe PROJECT_ID \
    --format="value(projectNumber)")-compute@developer.gserviceaccount.com \
    --role="roles/container.developer"
```

**Cria um cluster**
```
$ gcloud container clusters create-auto quickstart-cluster-qsdev --project=authentic-door-368219 --region=us-central1 && gcloud container clusters create-auto quickstart-cluster-qsprod --project=authentic-door-368219 --region=us-central1
```

**Excluir um cluster e pipeline**
```
$ gcloud container clusters delete quickstart-cluster-qsdev --region=us-central1 --project=PROJECT_ID
$ gcloud container clusters delete quickstart-cluster-qsprod --region=us-central1 --project=PROJECT_ID
$ gcloud deploy delivery-pipelines delete my-gke-demo-app-1 --force --region=us-central1 --project=PROJECT_ID
```


Conforme a documentação, é necessário a criação de um diretório.
```
$ mkdir deploy-gke-quickstart
$ cd deploy-gke-quickstart
```

Criar arquivo **skaffold.yaml**
```
apiVersion: skaffold/v2beta16
kind: Config
deploy:
  kubectl:
    manifests:
      - k8s-*
```

Verificar na documentação a criação dos arquivos necessários para o deploy são eles:
- clouddeploy.yaml
- k8s-pod.yaml
- skaffold.yaml

Após a criaçãod dos arquivos, executar o comando
```
$ gcloud deploy apply --file clouddeploy.yaml --region=us-central1 --<PROJECT_ID>
```

Após seguir todos os passos, é possível realizar alterações na aplicação e iniciar o processo de alteração via comando:
```
$ gcloud deploy releases create test-release-001 \
  --project=<PROJECT_ID> \
  --region=us-central1 \
  --delivery-pipeline=my-gke-demo-app-1 \
  --images=my-app-image=k8s.gcr.io/echoserver:1.4
```

Para verificar o status, entrar em **Cloud Deploy** e clicar no pipeline desejado. Para **subir** a aplicação para o **Ambiente de Produção**, após selecionar o pipeline desejado, na página que irá abrir será apresentada dois icones, um de Dev outro de Prod (conforme especificado na criação do projeto), neste ponto, clicar em **Promover** no icone de Dev.

Existe a possibilidade de criar um procedimento de aprovação do Delivery, assim que alguém aprovar o delivery para Produção acontecerá de forma transparente.
