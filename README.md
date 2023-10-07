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

### Criação do Diretorio 'meu-docker' e os contêiners

Vamos criar um diretório chamado 'meu-docker' para armazenar nosso projeto Docker:     
1. em seu terminal entre como super usuario, para isso digite=     
'sudo su' depois coloque a sua senha.
2. agora para criar um arquivo para armazenar o Projeto Docker digite o seguinte=      
'mkdir meu-docker'.
3. agora vamos entrar nesse arquivo que acabamos de criar, digite esse codigo=      
'cd meu-docker'.
4. Dentro do diretório 'meu-docker' vamos criar um arquivo chamado 'docker-compose.yml', então digite o esse codigo=     
'nano docker-compose.yml'.
5. Dentro desse arquivo vamos criar um contêiner onde o codigo estará na parte de (Uso de Variáveis de Ambiente).
6. Após isso vamos salvar os conteudos escritos dentro de 'nano docker-compose.yml', digite o seguinte comando no teclado=    
'Ctrl + O e depois aperte Enter' agora precione 'Ctrl + X' para voltar para a pasta 'meu-docker'.
7. Agora vamos criar uma pasta chamada 'html', para isso digite o codigo=    
'mkdir html'.
8. Agora vamos entrar nesse arquivo que acabamos de criar, digite esse codigo=    
'cd html'.
9.  Vamos criar dentro de 'html' um arquivo chamado 'index.html', então digite o esse codigo=    
'nano index.html'.
10. 
11. Agora vamos salvar os conteudos escritos dentro de 'nano index.html', digite o seguinte comando no teclado=    
'Ctrl + O e depois aperte Enter' agora precione 'Ctrl + X' para voltar para a pasta 'meu-docker'.

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

## Codigos dos Contêineres
  
  Aqui está o codigo que utilizei para o contêner dentro do arquivo 'docker-compose.yml':

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
```

# Codigo Html:
Aqui está o codigo que utilizei para o contêner dentro do arquivo 'index.html':
```
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker</title>
    <style>
        /* Estilos para o cabeçalho (h1) */
        header h1 {
            color: blue;
            font-size: 24px;
        }

        /* Estilos para a barra de navegação (nav) */
        nav {
            background-color: #333;
            color: white;
        }

        nav ul {
            list-style-type: none;
            padding: 0;
        }

        nav li {
            display: inline;
            margin-right: 20px;
        }

        nav a {
            text-decoration: none;
            color: white;
        }

        /* Estilos para o conteúdo principal (main) */
        main {
            margin: 20px;
        }

        h2 {
            color: green;
        }

        /* Estilos para o rodapé (footer) */
        footer {
            background-color: #333;
            color: white;
            text-align: center;
            padding: 10px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Bem-vindo à Minha Página web pelo docker</h1>
    </header>
    <nav>
        <ul>
            <li><a href="#">Página Inicial</a></li>
            <li><a href="#">Sobre</a></li>
            <li><a href="#">Contato</a></li>
        </ul>
    </nav>
    <main>
        <h2>Sobre Nós</h2>
        <p>Somos uma empresa fictícia dedicada a demonstrações de HTML.</p>
    </main>
    <footer>
        <p>&copy; 2023 Minha Empresa</p>
    </footer>
</body>
</html>

```

