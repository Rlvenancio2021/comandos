# Terraform

Ferramenta open source de automação de processos.

Permite **criar a infraestrutura** de maneira **segura e previsível**, sem que ocorra erros humanos.

site:
https://cloud.google.com/docs/terraform
https://registry.terraform.io/providers/hashicorp/google/latest/docs

**Intalar Terraform**
site: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

No **Cloud Shell** já tem o Terraform instalado

## Primeiro Script 

### Cria uma instancia (GCP) de forma automática

No Cloud Shell pasta criada para exemplo "terraform-hello"

- Cria um diretório
- Cria o arquivo ".tf"
```
resource "google_compute_instance" "terraform" {
  project      = "PROJECT_ID"
  name         = "terraform"
  machine_type = "n1-standard-1"
  zone         = "us-central1-a"
  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }
  network_interface {
    network = "default"
    access_config {
    }
  }
}
```
- Rodar comandos
```
$ terraform init       # Irá baixar todos as dependencias necessárias para o Terraform

$ terraform plan       # É o plano de execução, tudo que será criado após a execução do script

$ terraform apply      # Para execução do plano

$ terraform show

$ terraform destroy    # Para exlcuir a máquina criado
```

### Cria um Cloud Storage para guarda de informações do projeto (terraform.state)

Terraform cria um arquivo no diretório de **STATE**, que guarda informações referente a infraestrutura criada, é um arquivo importante que não pode ser guardado no GitHub, no GCP, podemos guardar esses arquivo no Cloud Storage, da seguinte forma.

Incluir no código um recurso de **backend**
```
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
    }
  }

   backend "gcs" {
    bucket  = "<NOME DO BUCKET>"    # Necessário criar o bucket primeiro
    prefix  = "terraform/state"
  }

}
```

### Criar Variáveis e Saídas

Criar um arquivo separado só para a declaração de variáveis (sugestão de nome "variables.tf"

No arquivo main.tf, declarar a variável da sequinte forma **"${var.network_name}"**

Usar variável torna o código mais reutilizável e de fácil manutenção, pois não será necessário correr todo o código para realizar as modificações desejadas.

```
variable "network_name" {
  description = "Nome da Rede"
  type        = string
  default     = "terraform-network"   # Aqui é o nome da variável
}

variable "centro_custo_rh" {
  description = "Nome da Rede"
  type        = string
  default     = "rh"
}
```

**Uma forma de retornar o valor dos recursos, exemplo IP da máquina criado depois da execução do Terraform**

No para o arquivo "outputs.tf"

```
output "ip" {
  value = google_compute_instance.vm_instance.network_interface.0.network_ip
}
```
*Obs* 
- **vm_instance** é dado ao recurso criado no arquivo **main.tf**
  *resource "google_compute_instance" "vm_instance" {...*
- **network_interface.0** *network_interface* é o nome de recurso que desejo pegar, e *0* (zero) é o IP da máquina


## Exportar recursos do Google Cloud para o formato Terraform

site: https://cloud.google.com/docs/terraform/resource-management/export?hl=pt-br

Ferramenta para exportação

Necessário instalar a interface **config-connecto**
```
$ gcloud components install config-connector
```

Exportação dos recursos, irá exportar todos os recursos existente no GCP, este comando server para ajudar a regularizar os projetos legados para o modelo Terraform

Ideal executar os comandos dentro de um diretório criado para está finalidade

Comando apenas para exibir na tela
```
gcloud beta resource-config bulk-export \
  --project=<PROJECT_ID> \
  --resource-format=terraform
```

Comando para gravar o arquivo
```
gcloud beta resource-config bulk-export \
  --path=OUTPUT_DIRECTORY_NAME \
  --project=<PROJECT_ID> \
  --resource-format=terraform
```

## Importando os estados (state)

site: https://cloud.google.com/docs/terraform/resource-management/import?hl=pt-br

O state é o que mantém sincronizado o que tem no projeto e o que está sendo alterado

Criar módulos do Terraform a partir do código gerado
Importante criar o estado (state) para melhor controle das alterações do projeto
```
$ gcloud beta resource-config terraform generate-import entire-tf-output <NOME DA PASTA ONDE OS ARQUIVOS FORAM EXPORTADOS>
```

Irá gerar dois arquivos
 - um **scritp** de importação dos módulos **terraform_import_20221201-02-04-59.sh**
 - E **gcloud-export-modules.tf**

- Rodar os comandos
```
$ terraform init
$ ./terraform_import_20221201-02-04-59.sh
```
Irá gerar o arquivo **terraform.tfstate**


## Armazenar o estado (state) do Terraform em um bucket do Cloud Storage

site: https://cloud.google.com/docs/terraform/resource-management/store-state?hl=pt-br

Para os casos em que não foi configurado no Terraform o Storage para guardar o arquivo de *state*

Pegar como referência o arquivo **backend.tf**
```
terraform {
 backend "gcs" {
   bucket  = "BUCKET_NAME"
   prefix  = "terraform/state"
 }
}
```

Rodar comandos
```
$ terraform init

$ terraform apply
```

Depois podemos apagar os arquivos de *state* que estiver no diretório.
