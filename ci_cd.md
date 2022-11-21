# CI/CD com Gitlab, Docker e Kubernetes


**Deploy** implementação envolve mover o software de um ambiente controlado para outro. Um ambiente é um subconjunto de infraestrutura de TI usado para uma finalidade específica.

**Integração Contínua (CI)** é uma prática de desevnvolvimento de software em que os desenvolvedores, com frequência, juntam suas alterações de código em um repositório central. Depois disso, criações e teste são executados. Os principais objetivos da integração contínua são encontrar e investigar erros mais rapidamente, melhorar a qualidade do software e reduriz o tempo necessário para validar e lançar novas atualizações de software.

**Entrega contínua (CD)** é uma prática de desenvolvimento de software em que alterações de código são criadas, testadas e preparadas automaticamente para liberação para produção. Ela expande com basse na integração contínua, pela implantação de todas as alterações de código em um ambiente de teste e/ou ambiente de produção, após o estágio de criação. Quando a integração contínua for implementada adequandamente, os desenvolvedores sempre terão um artefado de criação pronto para ser implantado, e que passou por um processo de teste padronizado.

- Para o processo de **CI no GitLab** é necessário criar arquivo de nome **.gitlab-ci.yml**.
Com este arquivo, será automatizado o processo de criação de uma imagem docker da aplicação, sempre que sofre alguma alteração.
O GitLab trabalhar por estágios (stages e/ou palco), podemos no mesmo arquivo criar a imagem docker e executar o pod no cluster kubernetes, basta especificar os processo de cada estágio.

**Exemplo de configuração para criação de imagem Docker**

```
stages:                                # Estágio (palcos) que o GitLab trabalha
  - build                              # Aqui são especificados os estágios que serão trabalhados

build_images:
  stage: build
  image: docker:20.10.16               # É necessário utilizar uma imagem do Docker para gerar a imagem docker da aplicação (Docker in Docker pesquisar no Hub.Docker.com)


  services:
    - docker:20.10.16-dind             # É obrigatório informar esta imagem, pois o linux necessita do serviço Daemon

  variables:
    DOCKER_TLS_CERTDIR: "/certs"       # Caminho obrigatório para onde serão gerados os certificados do Docker

  before_script:                       # Listar aqui os procedimentos necessário antes de gerar o script, exemplo logar na conta do Docker
    - docker login -u $REGISTRY_USER -p $REGISTRY_PASS   # Utilizar variáveis de ambiente para proteção dos dados de login

  script:
    - docker build -t <NOME DE USUARIO DOCKER>/app-cicd-dio:1.0 app/.
    - docker push <NOME DE USUARIO DOCKER>/app-cicd-dio:1.0
```

**Exemplo de configuração para deploy no GCP**

```
deploy_gcp:

  stage: deploy_gcp

  before_script:
    - chmod 400 $SSH_KEY

  script:
    - ssh -o StrictHostKeyChecking=no -i $SSH_KEY rlvenancio@$SSH_SERVER_IP "sudo rm -rf ./app-cicd-dio/ && sudo git clone https://gitlab.com/rlvenancio/app-cicd-dio.git && cd app-cicd-dio && sudo chmod +x ./script.sh && ./script.sh"
```
No ssh o parâmetro "-o" é para não realizar o questionamento visto que será executado por escrite, e caso o mesmo gere o "ScriptHostKeyChecking=no" irá para o processo. Consta também alguns comandos a serem executados.

Para o git clone, dentro das linhas de comandos do código acima, é necessário **clonar o caminho via HTTPS**

O arquivo **./script** que irá executar um comando **kubectl**, tem que ser executado pelo usuário comum que realizou a instalação do **Kubectl**

Para utilização das variável de ambiente para realização de login, necessário configurar no repositório do GitLab caminho **RepositórioDesejado/"CI/CD"/Variables**.

## Acessar o cluster via Máquina Virtual Bastion Host

- Para configurar acessar um cluster Kubernetes por meio de um processo CI/CD, no caso de serviço da Google GCP, é necessário criar uma máquina virtual chamada **Bastion Host**, que sempre estará online.

Por ser uma máquina apenas para gerenciar o acesso ao cluster, não precisa de muito memória e nem capacidade de processamento.
O ideal é que a maquina estaja na mesma região que o cluster ou na mesma rede de acesso.
Após criação da máquina, será necessário ir na opção **EDITAR**, ir em **Chave SSH** inserir a chave pública para acesso.
Realizar acesso a máquina via ssh
```
$ ssh <NOME USUÁRIO>@<NÚMERO IP PÚBLICO>
```
**Configurações iniciais**
 - gcloud instalado
 
 - Configurar o usurário
 ```
 $ gcloud config set account <NOME DO USUÁRIO - referente o usuário do GCP>
 ```
 
 - Iniciar o gcloud - Caso não tenha necessidade de realizar qualquer alteração, pode aperta "Control+C"
 ```
 $ gcloud init
 ```
 - Verificar se a Máquina Virtual tem acesso ao cluster. Para isso, deve ir até o cluster e copiar o acesso de conexão via linha, igualmente seria para um acesso via máquina local e colar na terminal da VM.
 
 - Caso solicite para realizar o Login, seguir o procedimento
  1. Rodar comando **gcloud auth login**
  2. Colar no navegador e link disponibilizado
  3. Copiar do navegador o código de autenticação.
  4. Necessário instalar os pacotes
 ```
 $ sudo apt-get install kubectl
 $ sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin
 ```
  5. Verificar novamente se a VM tem acesso ao cluster, rodar os comandos do **Kubectl** 
  6. Necessário instalar o **GIT**.

 - Necessário informar a **chave ssh privada** na variável de ambiente do **GitLab**
  1. Ir em **settings/"CI/CD"/Variable** do repositório, e colar a chave privada, adicionar um espaço presionando "Enter" no final da linha
  2. Alterar **Type** para **File**
  3. Adicionar outra variável do **tipo variaval** com o número de IP da **VM Bastion Host**.
  
 - Após executar o scrit o acesso ao cluster será por meio da VM Bastion Host
  1. Primeiro loga na VM
  2. Acesse o cluster.
