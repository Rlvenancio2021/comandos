# CLI GIT

## Configurações globais do 'GIT'

```
$ git config --local = Para alterações em um projeto específico
$ git config --global = Para alterações globais em todos os projetos
$ git config <nome do campo> = Para vizualizar o a configuração específica (git config user.name)

$ git config --global init.defaultBranch <nome da branch>

$ git config --global user.email <"digitar o e-mail entre aspas">
$ git config --global user.name <Digite o nome>
```


## Criando vinculo a diretório GITHUB

```
$ git remote add origin <git@github.com:<usuário do GitHub ex.Rlvenancio2021>/<nome do repositório>.git> 
//A palavra "Origin" funciona como um apelido para o endereço do repositório e será utilizando no momento de realizar o commit.
$ git remote -v //Comando para listar o repositórios remotos vinculados
$ git remote rename <nome atual> <novo nome>
$ git remote remove <nome do repositório que deseja excluir>
```


## Comandos para consulta de comite

 - link: https://devhints.io/git-log
```
$ git log
$ git log --oneline
$ git log -p //Vizualiza as alteração do commit
	     // Para vizualizar as alterações de um commit espscífico, basta passar a hash como parâmetro.
$ git log --graph
// Podemos combinar comandos
$ git log --oneline --graph
$ git log -n <número de commits que deseja exibir>
```

## Tags e Releases

Elementos utilizados para identificar um ponto específico do código, seja a marca de uma versão (ou seja ponto em que houve uma 
entrega que o programa esta entregável), ou por qualquer outra necessidade que demonstre que aquele pontos do projeto (até 
aquele commit).
Com a criação da TAG e trabalhando com um repositório remoto exemplo GitHub, é possível baixar o programa completo até o momento 
em que foi Taguiado permitindo que tenham acesso a versão do sistema, podendo ser baixado o arquivo completo para uso.

```
$ git tag -a <nome da TAG/Versionado do APP> -m '<OPCIONAL pode-se escrever uma mensagem>'
$ git tag //Lista todas as Tags disponíveis
$ git push <nome do repositório> <nome da TAG> //Envia a Tag para o repositório remoto  
```

## Criar repositório para controle de alterações

### Repositório Local

```
$ git init --bare
 //Parâmetro "bare" que indica que serão controladas apenas as alterações. Necessário ser em pasta 
diferente da que o Git controle o versionamento do código completo.Desta forma podemos usá-lo como repositório local, voltando na pasta do projeto e passando o endereço completo do 
caminho para o "git remote".

$ git remote add <endereço completo da pasta>
```

Este repositório local funciona igual o remoto, podendo ser clonado e compartilhado com outras pessoas.


## Comandos do dia-a-dia

```
$ git clone <endereço do repositório a ser clonado> <OPICIONALpodemos passar como parâmetro um nome para pasta>
$ git branch
$ git branch <nome da nova branch> //Cria uma nova branch
//Ou
$ git checkout -b <nome da nova branch> //Cria e loga na nova branch
$ git checkout <nome da branch que deseja logar>
$ git checkout -- <nome do arquivo> //Comando para desfazer uma alteração ainda não commitada de um arquivo específico.
$ git merge //Para unir os trabalhos entre branch diferentes, necessário logar na branch que irá receber o merge.
$ git diff //Mostra a última alteração, porém antes de add para a área de "stage"(antes do comando add).
$ git diff <hash reduzida>..<hash reduzada> //Mostra a diferença de uma hash "Até (..)" a outra
```

### Salvar para continuar depois
```
$ git stash //Comando que salva as alterações em local separado
$ git stash list //Comando para listar os arquivos salvos
$ git stash drop //Comando para apagar a lista de arquivos saldo na stash
```
Retornar os arquivos salvos para continuar o trabalho, temos duas formas
```
$ git stash apply <indice da stash> //Mantém o arquivo na lista de stash
$ git stash pop //Retorna o arquivo e tira da lista
```

### Git Checkout outras funções

Comando para desfazer uma alteração ainda não comitada de um arquivo específico
```
$ git checkout -- <nome do arquivo>
```

Comando para visualizar uma commit específico da lista, com isso é possível será criada uma branch aleatória que 
permite realizar testes e alterações a partir do commit escolhido sem afetar as ramificações do projeto, caso queira fazer uma 
ramificação definitiva é necessário criar uma branch. É uma forma de "NAVEGAR" pelo projeto.
```
$ git checkout <hash de um commit> //Pode ser os primeiro 6 ou 7 caracteres do hash
$ git switch -c <nome da nova branch> //Comando para criar uma nova branch a partir do commit escolhido.
```


### Comandos especiais
```
$ git rebase <nome da branch que deseja buscar> //Com esse comando é feito um redirecionamento da base do commit, porém 
pode gerar conflitos. Mantém os commit da branch que recebeu (branch logada) como sendo o último realizado, referenciando 
as branch recebidas.
$ git checkout -- <nome do arquivo> //Comando para desfazer uma alteração ainda não commitada de um arquivo específico.
$ git revert <código hash do commit> //Comando para reverte um commit
```
