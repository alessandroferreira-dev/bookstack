**Observações:** No arquivo docker-compose.yml é preciso gerar uma chave antes e adcionar neste arquivo para que o bookstak funcione com o comando abaixo:
  **docker run -it --rm --entrypoint /bin/bash lscr.io/linuxserver/bookstack:latest appkey**
  No arquivo docker-compose.yml você ira adicionar a linha **- APP_KEY=base64: [código da chave]=**

**Observação 02:** Caso ao executar o docker-compose.yml e entrar na aplicação aparecer o erro 500 é necessário configurar as variáveis do banco de dados internas dentro do container

**Guia de Instalação e Correção do BookStack no
Docker (Windows Desktop)
Este guia descreve as etapas para implementar e corrigir o BookStack executando em contêineres
Docker no Windows Desktop.**


**Etapa 1 — Acessar o contêiner do BookStack**
1. No PowerShell:
docker compose exec bookstack bash
Isso abre o terminal do contêiner do BookStack.


**Etapa 2 — Verificar o arquivo .env interno**
Dentro do contêiner, execute:
cat /config/www/.env | grep DB_
Se aparecer valores incorretos (como 'database_username'), será necessário corrigi-los no próximo
passo.


**Etapa 3 — Corrigir o arquivo .env**
Edite o arquivo com:
nano /config/www/.env
Substitua as variáveis por:
DB_HOST=db DB_USER=bookstack DB_PASS=bookstackpass DB_DATABASE=bookstack
APP_URL=http://localhost:6875 APP_KEY=
Salve com Ctrl+O, Enter e saia com Ctrl+X.


**Etapa 4 — Limpar cache do Laravel (BookStack)**
Execute os seguintes comandos no contêiner:
cd /app/www php8 /app/www/artisan config:clear php8 /app/www/artisan cache:clear php8
/app/www/artisan config:cache
Esses comandos limpam o cache e recarregam as configurações do BookStack.


**Etapa 5 — Reiniciar os contêineres**

Saia do contêiner com 'exit' e, no PowerShell, execute:
docker compose restart
Isso reinicia o BookStack e o banco de dados com as novas configurações.


**Etapa 6 — Testar o acesso**
Abra o navegador e acesse:
http://localhost:6875
Login padrão:
Usuário: admin@admin.com Senha: password
■ Dica: Se o erro 'Access denied for user database_username' aparecer novamente, verifique se o
volume antigo foi removido com:
docker volume rm bookstack_db_data bookstack_bookstack_data
