# PROJETO DOCKER

## Após fazer o clone do Projeto

Rode os Comandos:
```bash
cd projeto-docker
docker compose build
```
## Adicionando um novo Projeto
- A Pasta www será o local onde será adicionado os projetos
- crie, ou arraste, o projeto para a pasta www

## Configurando o Arquivo nginx
- Na pasta **docker/nginx** adicione o arquivo **.conf** para configuração do projeto
- exemplo: **projeto-default.conf**
- configurar o arquivo da seguinte maneira
```
server {
    index index.php index.html;
    listen 443 ssl;
    listen 80;
    server_name default.co; //SUBSTITUIR VALOR DEFAULT PARA O NOME DO PROJETO, adicionando .co no final, exemplo: "projeto-default.co"
    error_log  /var/log/nginx/default.co-error.log; //SUBSTITUIR O VALOR DEFAULT PARA O NOME DO PROJETO, exemplo: "/var/log/nginx/projeto-default.co-error.log"
    access_log /var/log/nginx/default.co-access.log; //SUBSTRITUIR...
    root /usr/share/nginx/html/default/public; //SUBSTRITUIR O VALOR DEFAULT PARA O NOME DO PROJETO, exemplo: "/usr/share/nginx/html/projeto-default/public"
    ssl_certificate     /usr/local/nginx/ssl/cert.crt;
    ssl_certificate_key /usr/local/nginx/ssl/cert.key;
    ssl_protocols       SSLv3 TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php82:9000; //VERSÃO UTILIZADA DO PHP (CASO QUEIRA ALTERAR A VERSÃO MUDE php82, para php70)
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
    location / {
        try_files $uri $uri/ /index.php?$query_string;
        gzip_static on;
    }
}
```

## Configurando Banco de Dados
- no arquivo docker-compose.yml:
- configurar o arquivo de conexao do banco
  
```bash
  database-mysql:
    container_name: mysql
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    volumes: 
      - ./docker/database/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: nome_do_seu_banco
      MYSQL_USER: docker
      MYSQL_ROOT_PASSWORD: sua_senha
    ports:
      - 3306:3306
```

- Após ter concluido esses procedimentos:
- Na pasta raiz rode o comando:
Rode os Comandos:
```bash
docker compose up -d
```
## Para finalizar... 
- Vá até o arquivo de confuguração de hosts na sua máquina e adicione o novo host
```
127.0.0.1	projeto-default.co //server_name do arquivo de configuração do nginx
```
- Vá na Página do navegador e digite:
- http://default.co
