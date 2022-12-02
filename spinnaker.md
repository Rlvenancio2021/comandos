# Spinnaker

É uma ferramento opensource para Continuous Delivery (CD) em multi cloud. Com ela, é possível entregar mudanças de software com alta velocidade e confiança.
Serve para criar um pipeline de entrega, automatizando a execução das diversas tarefas necessárias em um deploy.

site: https://spinnaker.io/

Multi-cloud continuous delivery for the enterprise

## Passo a Passo para instalação

Para ser executado no terminal ou Cloud Shell

github: https://github.com/digitalinnovationone/instalacaospinnakergcp

Após o passo7 da Instalação e Configuração Spinnaker, ainda no cloud shell no icone **Vizualização Web** ir em **Vizualizar na porta 8080**

Em caso de voltar a rodar os comandos no Cloud Shell depois de um tempo, será necessário rodar novamente *Passo4* para exportar as variáveis e o *Passo7* para conectar o Spinnaker

Ao rodar os comandos para a **Inclusão segundo cluster GKE** no **Passo3** script de adiciona conta no GKE, informar o contexto do *cluster do spinneker*

**Configurar o Pipeline de Exemplo**

Seguindo o passo a passo, irá utilizar uma ferramento chamada **spin** para salvar alguns conteúdos e variáveis.

Após seguir todos os passos, inclusive os de Edição, pode-se verificar o resultado nos seguintes recursos
**GCP**
 - Cloud Build
 - Source Repositories
**Spinnaker**
 - Applications
   - Navegando em Aplicação desejada / Infraestrutura / Load Balancers (Clica na aplicação) / **Ingress** contém o Endereço de IP da aplicação que pode ser consultada para ver ela rodando. 

**Para rodar a aplicação em produção** neste exemplo será necessário um processo manual, no *Spinnaker* em **Pipeline / Deploy to Production** terá um item em destaque que passando o mouse irá solicitar um **Continue** para aprovar a rodar a aplicação na produção.
