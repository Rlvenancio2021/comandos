# AWS

site: https://aws.amazon.com

## Infraestrutura Global AWS

- **Regiões AZ:** regiões geográficas
- **Availability Zones:** datacenters
- **Local Zones:** datacenters de menores dimensões, conectado a uma AZ
- **Wave Length:** servidor específico de aplicações com baixa latência conectado a uma AZ
- **Outspots:** datacenters terceiros (colocations) com servidores AWS.

## Serviços da AWS

- Computação
- Serviços serverless
- Armazenamento
- Banco de Dados
- Big Data
- Machine Learning

*site:* https://aws.amazon.com/pt/products/

## IAM

**Identity and ACess Management - IAM** Seriço gratuíto e não possui limite de uso.

***Conceitos do IAM***

- **Identity:** Fornece acesso a uma conta na AWS
- **IAM Users:** Representa uma pessoa ou serviço que utiliza serviços AWS
- **User Groups:** Coleção de usuários IAM
- **IAM Roles:** Conjunto de permissões que determinam o nível de acesso de uma identidade aos serviços AWS.
  - **Inline policy:** permissões atreladas diretamente a uma identidade (não são reaproveitáveis)
  - **Managed policy:** Conjunto de permissões disponível para várias identidades.
- **IAM Policies:** Define permissões de acesso a serviços AWS
- **IAM Permissions:** Nível mais baixo da hierarquia, determina se uma identidade pode ou não tomar uma ação sobre um recurso na AWS (Allow/Deny).

***Boa práticas***

**Conta raiz:** não utilizá-la em tarefas diárias de desenvolvimento
**Usuários:** Crie usuários individuais.
**Privilégios mínimos:** prover apenas o nível de acesso necessário
**Permissões:** Utilizar grupos de usuários com permissões
**Auditoria:** Ativar o AWS CloudTrail
**Senhas:** senhas fortes
**MFA:** Ativar para usuários privilegiados

### Criar Usuários

Um usuário do IAM é uma entidade criada para representar a pessoa ou aplicação que o utiliza para interagir com os serviços da AWS e consiste de um nome e credenciais.

Pesquisar por **IAM** *Identity and Acess Management*

Ao Adicionar um usuário lembra de melhor prática que é dar permissões ao grupo e adicionar o usuário ao grupo específico, e ao selecionar **Tipo de acesso à AWS** flegar **Chave de acesso** e **Senha**.

Um exemplo de permissões:
1. Podemos criar um bucket no **S3**, depois, selecionar o usuário e **Adicionar política em linha**;
2. Selecionar o Serviços (neste caso S3);
3. Em **níveis de acesso** escolher o desejado e verificar as opções (neste caso não flegar por exemplo *"Gravação"* e sim dentre as opções de gravação apenas as que forem necessárias para o Grupo e/ou Usuário.
4. Em **Recursos / Adicionar ARN (Amazon Resource Name)**
  - Campo **Especificar ARN para object** indicar o nome do recursos, neste caso o nome do Bucket que foi criado;
  - Campo **Bucket name** informar o nome do Bucket que foi criado;
  - Campo **Object nome** informar "asterisco" ou flegar **qualquer** para todos os objetos dentro do Bucket.
5. Informar um **Nome** para a política de permissão criada.

*OBS* caso necessário, é possível editar a política para modificar as permissões.

### Criar Grupos de Usuários

Via IAM

Selecionar Grupos, da um nome ao grupo, em seguida **Associar políticas de permissões** um exemplo pesquisar por **Lambda** e selecionar **AWSLambda_FullAccess** e criar grupo

### Criando Roles e Policies AWS IAM

Uma **política (policy)** é um objeto na AWS que, quando associado a uma identidade ou a um recurso, define suas permissões.
Uma **função (role)** é uma identidade da AWS com políticas de permissões que determinam quais ações podem ou não serem executadas na AWS. É um conjunto de políticas de acesso.

Primeiro cria a Política(Policy) depois cria a função(role) atribuindo a política criada.

Em IAM clicar em **políticas/policies** em seguida clicar em **Criar política/ Create policy**
 - Atribuir a política criada ao Grupo desejado para assim lliberar o acesso aos membros do grupo.

Em IAM clicar em **funções/roles** em seguida clicar em **Criar funções/Create roles**
 - Atribui a um serviço exemplo ao criar um **Lambda**

## Imersão ao Ecossistema Cloud Data AWS

Os serviços em nuvem consistem em infraestrutura, plataformas ou software hospedados por fornecedores terceirizados e disponibilizados aos usuários via internet

### EC2

Processamento em Cloud

**Amazon Elastic Compute Cloud - EC2** é uma parte central da plataforma de cloud computing da AWS. O EC2 permite que os usuários alugem computadores virtuais nos quais rodam suas próprias aplicações. O EC2 permite a implantação de aplicações escaláveis ao prover um **Web service** através do qual um usuário pode iniciar uma **Amazon Marchine Image** para criar uma máquina virtual "instância", contendo qualquer software desejado.

### RDS

Sistemas de Gerenciamento de Banco de Dados

**Amazon Relational Database Service (Amazon RDS)** é um serviço de banco de dados SQL gerenciado fornecido pelo AWS. O Amazon RDS suporta uma variedade de mecanismos de banco de dados para armazenar e organizar dados e ajuda nas tarefas de gerenciamento de banco de dados, como migração, backup e recuperação.
Compatível com:
  - MySQL
  - PostgreSQL
  - Oracle
  - SQL Server
  - MariaDB
RDS é um serviço da Web que facilita a configuração, a operação e escalabilidade de um banco de dados relacional na Nuvem AWS.

### S3

Armazenamento em Cloud

**Amazon S3** ou **Amazon Simple Storge Service** é um serviço oferecido pela AWS que fornece armazemento de objetos por meio de uma intergace de serviço da Web.


## Introdução a Engenharia de Dados na AWS

**AWS Snow Family**

- Dispositivos portáteis altamente seguros para coletar e processar dados e migração para dentro e para fora da AWS
- Dispositivos offline para realizar migrações de dados.
- Migração
    - Snowcone (Um HD externo de alta capacidade, disponibilizado via correio)
    - Snowball Edge (Um HD porém grande em tamanha físico)
    - Snowmobile (Um conteiner via caminhão, onde os dados são copiados e levados para as instalações da AWS)
- Edge computing
    - Snowcone
    - Snowball Edge
*OBS* 
  - São dispositivos físicos, disponibilizados pela AWS para realizar a migração desejada.
  - Edge computing - São os dados criados em locais onde o acesso dos dados não é constante ou com apresenta dificuldade para transferência de dados, exemplo um Navil uma Mina.

**AWS Kinesis**

Alternativa gerenciada ao Apache Kafka
Ótimo para logs de aplicativos, métricas, IoT, clickstreams, big data "em tempo real" e framework de processamento de streaming

**AWS Elastic MapReduce**

Serviço totalmente gerenciado

- Estrutura gerenciada do Hadoop em instâncias EC2
- Inclui Spark, HBase, Presto, Flink, Colmeia e mais
- Notebooks EMR
- Vários pontos de integração com AWS
- HDFS

Cluster EMR
**Master node**: gerencia o cluster
- Rastreia o status das tarefas, monitora o cluster
- Única instância EC2
- Também conhecido como "nó líder"
**Code node**: hospeda dados HDFS e executa tarefas
**Task node**: executa tarefas, não hospeda dados
- Opcional
- Sem risco de perda de dados ao remover
- Bom uso de instância pontuais.

Se for necessário escalar o processamento de dados, pode-se inserir mais Task Node no cluster. Podem ser desligado sem risco de perda de dados, pois eles não armazenam dados apenas processa.

**AWS Glue**

É um serviço de integração de dados **sem servidor** que facilita descobrir, preparar e combinar dados para análise, machine learning e desenvolvimento da aplicação.

- AWS Glue DataBrew
- AWS Glue Elastic Views

O AWS Glue não armazena dados. Trabalha com a estrutura do banco de dados. Trabalho com catálogo.
Dados não estruturadas são tratados como estruturados.


**AWS Redshift**

Serviço de armazenamento de dados (banco de dados) gerenciado em escala de petabytes.
Totalmente gerenciado pela AWS.

- Desempenho 10 vezes melhor do que outros DW
- Projetado para OLAP, não OLTP
- Econômico
- Interfaces SQL, ODBC, JDBC
- Escalado sob demanda
- Replicação e backups integrados
- Monitoramento via CloudWatch / CloudTrail

**Exemplo de aplicação do projeto**

site: https://github.com/Rlvenancio2021/dio-live-aws-bigdata-2
