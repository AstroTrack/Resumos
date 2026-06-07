RESUMO DEVOPS - ASTROTRACK

1. OBJETIVO

O objetivo da entrega DevOps do AstroTrack foi conteinerizar a API Java Spring Boot e o banco de dados em um ambiente integrado com Docker.
A solucao possui dois containers principais:
- app-astrotrack-563210: container da aplicacao Java.
- db-astrotrack-563210: container do banco Oracle XE.
Os dois containers executam na mesma rede Docker e se comunicam internamente.

2. TECNOLOGIAS USADAS

- Docker
- Docker Compose
- Java 21
- Spring Boot
- Oracle XE
- Maven
- Rede bridge customizada
- Volume nomeado

3. CONTAINER DA APLICACAO

O container da API e criado pelo arquivo AstroTrack_devops/app/Dockerfile.
Esse Dockerfile e multi-stage:
- Primeiro estagio: usa Maven com Java 21 para compilar o projeto.
- Segundo estagio: usa Java 21 para rodar o .jar da aplicacao.
Pontos que atendem ao edital:
- Usa imagem personalizada.
- Define WORKDIR /app.
- Cria usuario nao privilegiado chamado astro_user.
- Executa com USER astro_user.
- Expoe a porta 8080.
- Roda a API com java -jar /app/astrotrack.jar.
Assim, a aplicacao nao roda como root.

4. CONTAINER DO BANCO

O banco roda no container db-astrotrack-563210.
A imagem usada e gvenzl/oracle-xe:21-slim.
O banco usa variaveis de ambiente:
- ORACLE_PASSWORD
- APP_USER
- APP_USER_PASSWORD
A porta do banco e 1521:1521.
O banco possui volume nomeado astro_oracle_data.
Esse volume e mapeado em /opt/oracle/oradata.
Isso garante persistencia dos dados.

5. DOCKER COMPOSE

O arquivo principal e AstroTrack_devops/docker-compose.yml.
Ele sobe os containers app-astrotrack-563210 e db-astrotrack-563210.
Os dois nomes possuem o RM 563210.
O docker-compose tambem cria a rede astrotrack_network.
A aplicacao acessa o banco pela URL:
jdbc:oracle:thin:@//db-astrotrack-563210:1521/XEPDB1
Esse host funciona porque app e banco estao na mesma rede Docker.

6. HEALTHCHECK

O banco possui healthcheck.
A API usa depends_on com condition: service_healthy.
Isso faz a aplicacao aguardar o Oracle ficar pronto antes de iniciar.

7. SCRIPT DO BANCO

O script fica em AstroTrack_devops/db/init.sql.
Ele cria as tabelas:
- AT_CLIENTES
- AT_MOTORISTAS
- AT_VEICULOS
- AT_VIAGENS
- AT_CHECKPOINTS
- AT_USUARIOS_SISTEMA
Principais relacionamentos:
- AT_VIAGENS referencia AT_CLIENTES.
- AT_VIAGENS referencia AT_MOTORISTAS.
- AT_VIAGENS referencia AT_VEICULOS.
- AT_CHECKPOINTS referencia AT_VIAGENS.
Isso comprova multiplas tabelas e relacionamento no banco.

8. COMANDOS PRINCIPAIS

Subir os containers em segundo plano:
docker-compose up -d --build

Ver containers ativos:
docker-compose ps

Ver logs gerais:
docker-compose logs

Ver logs da API:
docker-compose logs app-astrotrack

Ver logs do banco:
docker-compose logs db-astrotrack

Parar containers:
docker-compose down

Parar e apagar volume:
docker-compose down -v

9. EVIDENCIAS COM DOCKER EXEC

Provar usuario nao-root da API:
docker container exec -it app-astrotrack-563210 whoami

Resultado esperado:
astro_user

Provar diretorio de trabalho da API:
docker container exec -it app-astrotrack-563210 pwd

Resultado esperado:
/app

Listar estrutura da API:
docker container exec -it app-astrotrack-563210 ls -la

Ver usuario do banco:
docker container exec -it db-astrotrack-563210 whoami

Ver diretorio do banco:
docker container exec -it db-astrotrack-563210 pwd

Listar estrutura do banco:
docker container exec -it db-astrotrack-563210 ls -la

10. EVIDENCIAS COM SELECT

Listar tabelas:
docker container exec -it db-astrotrack-563210 bash -lc "echo \"SELECT table_name FROM user_tables ORDER BY table_name; EXIT;\" | sqlplus -s ASTRO_USER/astro_password@//localhost:1521/XEPDB1"

Ver clientes:
docker container exec -it db-astrotrack-563210 bash -lc "echo \"SELECT id_cliente, nome, cnpj, email, status FROM AT_CLIENTES; EXIT;\" | sqlplus -s ASTRO_USER/astro_password@//localhost:1521/XEPDB1"

Ver viagens:
docker container exec -it db-astrotrack-563210 bash -lc "echo \"SELECT id_viagem, id_cliente, origem, destino, status FROM AT_VIAGENS; EXIT;\" | sqlplus -s ASTRO_USER/astro_password@//localhost:1521/XEPDB1"

Ver checkpoints:
docker container exec -it db-astrotrack-563210 bash -lc "echo \"SELECT id_checkpoint, id_viagem, latitude, longitude FROM AT_CHECKPOINTS; EXIT;\" | sqlplus -s ASTRO_USER/astro_password@//localhost:1521/XEPDB1"

11. COMO EXPLICAR A ARQUITETURA

A arquitetura DevOps separa a API e o banco em containers diferentes.
A API roda na porta 8080.
O Oracle XE roda na porta 1521.
Os containers estao conectados pela rede astrotrack_network.
A API acessa o banco pelo nome do container db-astrotrack-563210.
O volume astro_oracle_data garante que os dados do Oracle continuem salvos mesmo apos reiniciar os containers.

12. PERGUNTAS PROVAVEIS

Pergunta: Por que usar Docker Compose?
Resposta: Porque o projeto precisa subir API e banco juntos, configurando rede, portas, variaveis, volume e dependencia entre containers.

Pergunta: Como a API se conecta ao banco?
Resposta: Pela URL jdbc:oracle:thin:@//db-astrotrack-563210:1521/XEPDB1, usando o nome do container do banco na rede Docker.

Pergunta: Como foi garantida a persistencia?
Resposta: Com o volume nomeado astro_oracle_data, mapeado para /opt/oracle/oradata.

Pergunta: A aplicacao roda como root?
Resposta: Nao. O Dockerfile cria o usuario astro_user e executa a API com USER astro_user.

Pergunta: Como comprovar que roda em background?
Resposta: Com docker-compose up -d --build e depois docker-compose ps.

Pergunta: Como comprovar logs?
Resposta: Com docker-compose logs, docker-compose logs app-astrotrack e docker-compose logs db-astrotrack.

Pergunta: Como comprovar persistencia no banco?
Resposta: Fazendo CRUD pela API e depois SELECT direto no container Oracle com sqlplus.

Pergunta: Como comprovar relacionamento entre tabelas?
Resposta: Pelo init.sql, onde AT_VIAGENS possui foreign keys para cliente, motorista e veiculo, e AT_CHECKPOINTS possui foreign key para viagem.

13. FRASE CURTA PARA APRESENTAR

A parte DevOps do AstroTrack entrega uma arquitetura conteinerizada com dois containers integrados: um para a API Java Spring Boot e outro para o Oracle XE. A aplicacao roda com usuario nao privilegiado, usa imagem personalizada, variaveis de ambiente, porta exposta, rede Docker compartilhada e volume nomeado para persistencia. O README traz o tutorial completo, comandos de execucao, logs, exec nos containers e SELECTs para evidenciar os dados no banco.

