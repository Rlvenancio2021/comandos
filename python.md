# Python

## Curiosidades

Criado em 1989 por Guido Van Rossum

**Objetivos de Van Rossum para a linguagem Python**

- Uma linguagem fácil e intuitiva
- Código aberto, para que todos possam contribuir
- Código tão inteligível quanto inglês
- Adequada para tarefas diárias, e produtiva

## Comandos do dia a dia

Verificar qual é a versão do Python
```
python -V
python3 -V
```

## Modo Interativo

Podemos acessar o modo inteirativo de duas formas

1. Acesso direto ao interpretador Python
```
$ python
$ python3
```
2. Para rodar um app
```
$ python3 -i app.py
```

# Funções

**Dir** Sem argumentos, retorna a lista de nomes no escopo local atual. Com argumento, retorna uma lista de atributos válidos para o objeto.

**Help** Invoca o sistema de ajuda integrado. Funciona no modo off-line
```
$ dir()
$ dir(100)
$ help()
$ help(100)
```

**Função de entrada "input"**
*site* https://docs.python.org/3/library/functions.html#input
**Função de saída "print"**
*site* https://docs.python.org/3/library/functions.html#print
```
$ nome = input("Digite seu primeiro nome: ")
$ sobrenome = input("Digite seu sobrenome: ")

$ print(nome, sobrenome)
$ print(nome, sobrenome, end="...\n")
$ print(nome, sobrenome, sep="#")
```

## Variáveis e Constantes

Python **não possui uma palavra reservada** para declarar uma **constante**, neste caso por **Convênção** a constante é declarada em letras maiúscula, desta forma os programadores entenderam que é um dados que não pode sofrer alteração.

```
$ age = 28 //Declaração de uma variável
$ STATES = ['SP','PR','RJ'] //Declaração de uma constante
```

**Converter tipo da uma variável**
```
$ preco = 10.30
$ print(preco)
$ >>> 10.30

$ preco = int(preco)
$ print(preco)
$ >>> 10

$ print(str(preco))
$ >>> 10.3
```
*Obs* **str()** é o construtor de "String"

## Boas práticas

- O padrão de nomes deve ser *snake case.* ex. nome_completo;
- Escolher nomes sugestivos;
- Nome de constantes todo em maiúsculo;
- Estudar as convênções do Python.

## Operadores 

### Operadores Matemáticos
```
 + Soma
 - Subtração
 * Multiplicação
 / Divisão
// Divisão inteira (retornar um número inteiro)
 % Módulo (retorno o resto da divusão)
** Exponenciação
```

Segue as mesma regras da matemática

- Parêntesis
- Expoêntes
- Multiplicações e divisões (da esquerda para a direita)
- Somas e subtrações (da esquerda para a direita)

### Operadores de Comparação

O retorno será sempre um valor booleano (Verdadeiro/True ou Falso/False)

```
 == Igual
 != Diferente
 >= Maior ou Igual
 <= Menor ou Igual
 > Maior
 < Menor
```

### Operadores de Atribuição

São operadores utilizados para definir o valor inicial ou sobrescrever o valor de uma variável

```
   = Atribuição ou Substituição Simples
  += Atribuição com adição
  -= Atribuição com subtração
  *= Atribuição com multiplicação 
  /= Atribuição com divisão
 //= Atribuição com divisão inteira
  %= Atribuição com módulo
 **= Atribuição com exponenciação
```

### Operadores Lógicos

São operadores utilizados em conjunto com os operadores de comparação, para montar uma expressão lógica.

```
 and Operador lógico "E"
 or  Operador lógico "OU"
 not Operador lógico "Negação" é utilizado no início da expresão (ex. not 1000 > 1500)
```

## Operadores de Identidade

São operadores utilizados para comparar se os dois objetos testados ocupam a mesma posição na memória.

```
 is Está
 is not Não está

//Exemplo

$ curso = "Curso de Python"
$ nome_curso = curso
$ saldo, limite = 200,200

$ curso is nome_curso
$ >>> True

$ curso is not nome_curso
$ >>> False

$ saldo is limite
>>> True
```

### Operadores de Associação

São operadores utilizados para verificar se um objeto está presente em uma sequência.

```
 in 
 not in

//Exemplo

$ curso = "Curso de Python"
$ frutas = ["laranja", "uva", "limão"]
$ saques = [1500, 100]

$ "Python" in curso
$ >>> True

$ "maça" not in frutas
$ >>> True

$ 200 in saques
$ >>> False
```

## Estruturas Condicionais e de Repetição

**Estruturas Condicionais**

A estrutura condicional premite o desvio de fluxo de controle, quando determinadas expressões lógicas são atendidas

```
 if
 if/else
 if/elif/else
```

**if aninhado** é o uso do if dentro de outro bloco de if

**if ternário** permite escrever uma condição em uma única linha. Ele é composto por três partes, a primeira parte é o retorno caso a expressão retorne verdadeiro, a segunda parte á a expressão lógica e a terceira parte é o retorno caso a expressão não seja atendida.

```
$ status = "Sucesso" if saldo >= saque else "Falha"
```

**Estruturas de Repetição**

```
 for    Quando se sabe o número de vezes que será executado ou percorrer um um objeto iterável (ex. lista, string)
 while  Quanto não tem certeza de quantas vezes o código será executado. 
```

Exemplo de imprimir números pares ou ímpares usando o for
```
$ for numero in range(100):
$     if numero % 2 == 1:  // Será exibitos números pares, para os ímpares basta incidar "== 0" 
$         continue
$     print(numero, end=" ")
```

## Manipulação de strings

```
$ curso = "pYtHon"

$ print(curso.upper())
$ >>PYTHON

$ print(curso.lower())
$ >>> python

$ print(curso.title())
$ >>> Python
```

Para remover espaços em branco
```
.strip()
.lstrip()
.rstrip()
```

Para Junção e centralização
```
$ print(curso.cente(10, "#")) // Neste caso o espaço que será criado será preenchido com "#"
$ >>> "##Python##"

$ print(".".join(curso))
$ >>> "P.y.t.h.o.n"
```


## Conexão Python-Django / MySQL

Para conectar o Python ao MySQL      

1. Instalar o pacote pymysql
```
pip3 install pymysql
```
2. Nas pasta do Projeto criado modificar os seguintes arquivos: 

2.1  Arquivo "__init.py__"
```
import pymysql

pymysql.install_as_MySQLdb()
```

2.2. Arquivo "setting.py" na constante "DATABASES"
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql', #Neste caso o mysql para conexão com MySQL
        'NAME': 'nome do banco de dados',
        'USER': 'usuário',
        'PASSWORD': 'senha',
        'HOST': 'localhost ou caminho de conexão (ex. IP)',
        'PORT': '',
    }
}
```
Caso tenha outros banco de dados a serem conectados, pode-se criar outra chave/valor com os dados do novo banco.

3. Por trabalhar com conexão ao Banco de Dados melhor utilizar variáveis de ambiente para proteção dos dados.

Intalar pacote dotenv via comando:
```
pip install python-dotenv
```

Realizar as seguintes alterações no código:

Criar arquivo com o nome ".env" na raiz do projeto e nele criar as variáveis com os dados de login no Banco de Dados:
```
nome_bd = 'nome do banco de dados'
usuario_bd = 'nome do usuário'
senha_bd = 'senha de acesso ao banco de dados'
```

No arquivo "settings.py"
```
import dotenv
dotenv.load_dotenv(dotenv.find_dotenv())
...
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.getenv('nome_bd'),
        'USER': os.getenv('usuario_bd'),
        'PASSWORD': os.getenv('senha_bd'),
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

4. Carregar dados no Banco de Dados com FRAMEWORK DJANGO

Quando temos por exemplo um arquivo JSON com os dados para serem carregados em massa para o BD.
```
python manage.py loaddata <nome do arquivo com extensão>
```

## Instalação de pacotes

É uma boa prática criar um arquivo chamado "requirement.txt" que lista todos os pacotes que foram instalados durante a construção do projeto. Para criar o arquivo basta usar o comando:
```
pip freeze > requirement.txt
```
Será criado um arquivo com a lista dos pacotes, interessante carregar o arquivo no GitHub.

Para realizar a instalação dos pacotes listados no arquivo usar o comando:
```
pip install -r requirement.txt
```
