# Desafio SCS DEVOPS

Criarei um arquivo docker-compose.yml e implantarei um aplicativo composto por dois contêineres, sendo um deles contendo um servidor web que serve ngnix uma página HTML,
e o segundo contêiner será composto por um servidor de banco de dados MySQL.

## Requisitos

- Docker
- Docker Compose

## Configuração

### Agora vamos instalar o docker com o passo a passo a seguir:
#### primeiramente entre no terminal no Ubuntu e digite esse codigo=     
- 'sudo apt install -y apt-transport-https ca-certificates curl software-properties-common'

#### após rodar esse codigo, coloque esse outro=  
- 'curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg'

#### agora coloque esse codigo= 
- 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null'

#### feito isso coloque esse codigo=
- 'sudo apt install -y docker-ce docker-ce-cli containerd.io'
#### codigo para o Docker ser executado automaticamente=
- 'sudo systemctl start docker'
- 'sudo systemctl enable docker'

### Agora vamos instalar o Docker Compose:
#### Ainda no terminal digite o seguinte o codigo para instalar a versão mais recente=
- 'sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
#### agora digite esse codigo para da permissão=
- 'sudo chmod +x /usr/local/bin/docker-compose'
#### pronto agora é só verificar a versão instalada com esse codigo=
- 'docker-compose --version'

Os codigos mencionados você pode encontrá-los em [docker.com](https://www.docker.com/get-started).

## Uso de Variáveis de Ambiente

O aplicativo utiliza variáveis de ambiente para configurar os contêineres, 
#### Configuração do Contêiner MySQL (bank)
O contêiner MySQL é responsável pelo banco de dados:

- O nome do contêiner é definido como bank.
- A imagem oficial do MySQL é usada como base para este contêiner(mysql:latest).
- A porta 3307 do host está mapeada para a porta 3306 do contêiner para acessar o MySQL.
- As variáveis de ambiente configuram a senha do usuário root, o nome do banco de dados, o nome de usuário e a senha do usuário MySQL.
- Volumes são configurados para persistência de dados, armazenando os dados do MySQL em ./data.
- O arquivo pessoa.sql é copiado para /docker-entrypoint-initdb.d/ e será executado na inicialização do MySQL.

#### Configuração do Contêiner Nginx (web)
O contêiner Nginx é responsável por servir páginas HTML. Aqui estão algumas informações sobre sua configuração:

- O nome do contêiner é definido como persister.
- A imagem oficial do Nginx é usada como base para este contêiner(nginx:latest).
- A porta 8080 do host está mapeada para a porta 80 do contêiner para acessar o servidor web Nginx.
- Volumes são configurados para servir arquivos HTML personalizados armazenados em ./html.

#### Rede de Contêineres
- Ambos os contêineres (bank e web) estão conectados à rede Docker chamada 'mynetwork'.

Isso permite a comunicação entre os contêineres, se necessário.
  
  Para configurá-las, crie um arquivo `.env` no diretório do seu projeto com as seguintes variáveis:

```env
version: '3'
services:
  bank:
    image: mysql:latest
    container_name: bank
    ports:
      - "3307:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 123321
      MYSQL_DATABASE: mariaab
      MYSQL_USER: anjos
      MYSQL_PASSWORD: 123
    volumes:
        - ./data:/var/lib/mysql
        - ./pessoa.sql:/docker-entrypoint-initdb.d/pessoa.sql
    networks:
      - mynetwork

  web:
    image: nginx:latest
    container_name: persister
    ports:
      - "8080:80"
    volumes:
        - ./html:/usr/share/nginx/html
    networks:
      - mynetwork

networks:
  mynetwork:




