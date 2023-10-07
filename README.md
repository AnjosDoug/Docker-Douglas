# Desafio SCS DEVOPS

Criarei um arquivo docker-compose.yml e implantarei um aplicativo composto por dois contêineres, sendo um deles contendo um servidor web que serve ngnix uma página HTML,
e o segundo contêiner será composto por um servidor de banco de dados MySQL.

## Requisitos

- Docker
- Docker Compose

## Configuração

### Agora vamos instalar o docker com o passo a passo aseguir:
#### primeiramente entre no terminal no Ubuntu e digite esse codigo=     
- 'sudo apt install -y apt-transport-https ca-certificates curl software-properties-common'

#### após rodar esse codigo, coloque esse outro=  
- 'curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg'

#### agora coloque esse codigo= 
- 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null'

#### feito isso coloque esse codigo=
- 'sudo apt install -y docker-ce docker-ce-cli containerd.io'


Antes de começar, certifique-se de ter o Docker e o Docker Compose instalados no seu sistema. Você pode encontrá-los em [docker.com](https://www.docker.com/get-started).

## Uso de Variáveis de Ambiente

O aplicativo utiliza variáveis de ambiente para configurar os contêineres. Para configurá-las, crie um arquivo `.env` no diretório do seu projeto com as seguintes variáveis:

```env
MYSQL_ROOT_PASSWORD=senha_root
MYSQL_DATABASE=nome_do_banco
MYSQL_USER=nome_do_usuario
MYSQL_PASSWORD=senha_do_usuario

