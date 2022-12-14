# Python

## Conexão Python-Django / MySQL

Para conectar o Python ao MySQL      

 1. Instalar o pacote pymysql
```
pip3 install pymysql
```
2. Nas pasta do Projeto criado modificar os seguintes arquivos: 

2.1. Arquivo "__init.py"
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
