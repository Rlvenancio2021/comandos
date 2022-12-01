# Google Cloud SDK

site: https://cloud.google.com/sdk/docs

Todo recurso em GCP tem uma apresentação de **Linha de comando do gcloud** que demonstrar a forma de realizar a mesma atividade usando linha de comando.

site: https://cloud.google.com/sdk/docs/install

## Comandos

site: https://cloud.google.com/sdk/gcloud/reference/compute

Para obter informações do gcloud
```
$ gcloud info
```

Para iniciar o **processo de autenticação**
```
$ gcloud init
```
**Sempre ler as mensagens apresentadas na tela com atenção, muitas dúvidas podem ser esclarecida**.

Listar projeto que temos acesso
```
$ gcloud projects list
$ gcloud projects list --format="json"
$ gcloud projects list --format="table[box, title='Meus Projetos'] (createTime:sort=1,name,projectNumber,projectId:label=ProjetctID,parent.id:label=Parent)"
```

Listar Zonas
```
$ gcloud compute zones list
$ gcloud compute zones list --format="json"
```

# Cloud Build

site: https://cloud.google.com/build

Plataforma CI/CD Server para criação de pipeline 

Necessário ativar a API no console.google.com

Para conectar com algum versionador de códio clicar em **CONFIGURAR ACIONADORES DE VERSÕES** no Painel do Cloud Build

Faz
  - Compilação do código
  - Geração de artefatos
  - Delivery da aplicação

github: https://github.com/googlecloudplatform/cloud-build-samples
github: https://github.com/googlecloudplatform/cloud-builders

O cloud build para roda faz a leiura do arquivo **.yml ou .json** e executa por **steps**, pegando como base o repositório **cloud-build-samples** a pasta **basic-config** cada **name** do arquivo **cloudbuild.yaml** se refere a um conteiner em execução

**Registry** no GCP é uma recurso de armazenamento.
gcr.io é o caminho para o Conteiner Registry do GCP.

## Executando um exemplo 

Basic-config do repositório cloud-build-samples

- Trocar caminho da imagem para o caminho do Registry do GCP
```
DE   $ us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage
PARA $ gcr.io/${PROJECT_ID}/myimage
```
PROJETCT_ID é uma variável padrão do GCP.

- Para subir a aplicação
```
$ gcloud builds submit --config <NOME DO ARQUIVO yaml ou json>
```
Verificar se o Cloud Build tem as permissões necessárias para criação de máquinas no GCP compute, ir no item **Configurações** do cloud build.

**Uso de Variáveis no Cloud Build** 
site: https://cloud.google.com/build/docs/configuring-builds/substitute-variable-values

**Variáveis definidas pelo usuário**

- Entrar no **Cloud Storage** do console;
- Criar um **Bucket** dar um nome para o bucket que sejá único.

rodar comando, pegando como exemplo o "python-example-flask" do repositório "cloud-build-samples" 
```
$ gcloud builds submit --config cloudbuild.yaml --substitutions=_BUCKET_NAME=rodrigolvenanciopy
```
Neste exemplo, por não esta sendo executado no Git e neste caso não ter chave hash gerado, ao invés de usar a variável **SHORT_SHA** usar **BUILD_ID**.
A variável "SHORT_SHA" pega os 7 primeiro caracteres da chave hash.

**Cloud Source Repositories**

- Criar repositório no GCP, para versionamento de código. É possível conectar a um repositório externo.
- Após criar, clicar no botão **Console Do Cloud** no canto superior direito, escolher forma de autenticação, no caso SDK, e executar o comando no terminal.
- Rodar os comandos do Git (init, add e commit).
- Configurar repositório remoto (conforme instruções do Console do Cloud.

Após carregar os dados para o Source, ir para o Cloud Build em **Gatilhos** para automatizar os processos.
- Gerenciar repositórios, para adicionar um externo, ou verificar se o do Google foi reconhecido.
- Criar Gatilhos, uma boa prática é colocar o mesmo nome do repositório
  - Configura o tipo de ação que irá dar o start no Cloud Build
  - Se houver alguma variável declarada pelo usurário, informa em **Variável de substituição**
  - Conta de serviço, pode especificar uma, caso já tenha uma com autorização, ou deixar em branco usando uma default.
- Quando executado um novo commit e realizado um Push, o processo será automático no Cloud Build
```
$ git push --all google
```
