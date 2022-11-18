# Docker

repositório base https://github.com/luizomf/blog-django-for-deploy

Vídeo base https://www.youtube.com/watch?v=NkfEn3IroYU&list=WL&index=256

## Comandos iniciais de instalação e configuração

### Instalação
```
$ sudo apt install docker.io
```

### Criar usuário
Com utilização do "$USER" será criado o usuário do docker conforme o que estiver logado no sistema, com isso não será mais necessário utilizar "sudo" para escrever um comando
```
$ sudo usermod -aG docker $USER
```

## Comandos do dia-a-dia

Comando para listar as imagens do docker
```
docker ps
```

Comando para criar uma imagem para Banco de Dados - no caso MySQL
```
sudo docker run --restart always -d --name mariadb -p 3306:3306 -e MYSQL_ROOT_PASSWORD='<senha>' mariadb
```

Comando para acessar o Container do docker
```
docker exec -it mariadb bash
```

## Comandos para deploy do projeto

Configuração do Socket
```
sudo nano /etc/systemd/system/gunicorn_blog.socket
sudo nano /etc/systemd/system/gunicorn_blog.service
```

Os arquivos que serão criados nesses caminhas conterão os seguintes dados:
 **gunircorn.socket**
```
[Unit]
Description=Blog socket //Campo livre para escrita

[Socket]
ListenStream=/run/gunicorn_blog.sock

[Install]
WantedBy=sockets.target
```
**gunicorn.service**
```
[Unit]
Description=Blog daemon //Campo livre para escrita
Requires=gunicorn_blog.socket
After=network.target

[Service]
User=ubuntu //Usuário logado no servidor
Group=www-data
WorkingDirectory=/home/ubuntu/blog //caminho do Projto
ExecStart=/home/ubuntu/blog/venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn_blog.sock \
          blog.wsgi:application //Caminho onde está o arquivo "WSGI.PY"

[Install]
WantedBy=multi-user.target
```

Comando para rodar sistema
```
sudo systemctl start gunicorn_blog.socket
sudo systemctl enable gunicorn_blog.service
```

Comando para verificar status do sistema
```
sudo systemctl status gunicorn_blog.socket
sudo systemctl status gunicorn_blog.service
```

Comando para realizar teste do "gunicorn" irá retornar um HTML
```
curl --unix-socket /run/gunicorn_blog.sock localhost
```


## Configurando firewall
url base = https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04-pt

Compando para instalação
```
# Instalação
$ sudo apt install ufw

# Configuração de políticas padrões
$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing

# Permitir acesso ssh
$ sudo ufw allow ssh

# Habilita o UFW
$ sudo ufw enable

# Menu de ajuda
$ sudo ufw --help

# Listar configurações
$ sudo ufw app list
$ sudo ufw status

# Permitir outras conexões
$ sudo ufw allow 80
$ sudo ufw allow 443
$ sudo ufw status
```


## Configurando domínio

```
$ sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
$ sudo apt install certbot
$ sudo certbot certonly --standalone -d <domínio>
```


## Configuração do NGINX

Instalação
```
$ sudo apt install nginx
```

Reiniciar o servidor nginx
```
$ sudo systemctl restart nginx
```
Reiniciar o gunicorn
```
$ sudo systemctl restart gunicorn_blog
```

Apagar servidor padrão na pasta /etc/nginx/sites-enable/
```
# Navegar até a pasta
$ sudo rm defaul
```
Criar servidor próprio
 - Sem domínio configurado
```
$ 
```

## Criando uma imagem Docker

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

Para enviar a imagem para o repositório oficial

6. Logar do Docker via CLI
```
$ docker login
```

7. Realizar Upload da imagem para o repositório
```
$ docker push <NOME DA IMAGEM CONFORME ITEM 4 incluindo a TAG> 
