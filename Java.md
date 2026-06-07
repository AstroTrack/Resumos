RESUMO JAVA ADVANCED - ASTROTRACK

1. OBJETIVO DA API

O AstroTrack e uma API REST criada em Java 21 com Spring Boot 3.
A solucao gerencia logistica satelital para frotas em areas de sombra.
A API controla clientes, motoristas, veiculos, viagens e checkpoints de rastreamento.

2. ARQUITETURA EM CAMADAS

O projeto segue organizacao em camadas:
- Controller: recebe requisicoes REST.
- Service: executa regras de negocio.
- Repository: acessa o banco com Spring Data JPA.
- DTO: transporta dados de entrada e saida.
- Model: representa entidades JPA.
- Exception: centraliza erros.
- Security: configura JWT e protecao dos endpoints.

Fluxo principal:
Controller -> Service -> Repository -> Banco de Dados.

3. PACOTES PRINCIPAIS

control: controllers REST.
service: regras de negocio.
repository: interfaces JpaRepository.
model: entidades JPA e enums.
dto: Java Records.
exception: tratamento global de erros.
security: JWT e Spring Security.
config: Swagger/OpenAPI.

4. ENTIDADES DO DOMINIO

Principais entidades:
- Cliente
- Motorista
- Veiculo
- Viagem
- Checkpoint
- UsuarioSistema

Essas entidades representam o dominio de logistica satelital.

5. CRUD COMPLETO

A API possui CRUD completo para:
- Clientes
- Motoristas
- Veiculos
- Viagens
- Checkpoints

Operacoes usadas:
- GET para listar e buscar.
- POST para criar.
- PUT para atualizar.
- DELETE para remover.

6. ENDPOINTS PRINCIPAIS

Autenticacao:
- POST /auth/register
- POST /auth/login

CRUDs:
- /clientes
- /motoristas
- /veiculos
- /viagens
- /checkpoints

Cada CRUD possui:
- GET
- GET por ID
- POST
- PUT por ID
- DELETE por ID

Endpoints extras:
- GET /viagens/status?status=EM_ANDAMENTO
- GET /checkpoints/viagem/{idViagem}
- GET /hateoas

7. STATUS HTTP

A API usa status HTTP adequados:
- 200 OK para consultas e atualizacoes.
- 201 Created para criacao.
- 204 No Content para exclusao.
- 400 Bad Request para validacao ou regra de negocio.
- 401 Unauthorized para falha de autenticacao.
- 403 Forbidden para acesso negado.
- 404 Not Found para recurso inexistente.

8. DTOs E JAVA RECORDS

O projeto usa Java Records para transferencia de dados.
Exemplos:
- ClienteRequest e ClienteResponse.
- ViagemRequest e ViagemResponse.
- AuthRequest e AuthResponse.
- RegisterRequest e UsuarioResponse.

Os DTOs evitam expor diretamente as entidades JPA.

9. VALIDACAO

A API usa Spring Validation nos DTOs.
Principais anotacoes:
- @NotBlank
- @NotNull
- @Size
- @Email
- @CPF
- @CNPJ
- @Pattern
- @DecimalMin
- @DecimalMax

As validacoes impedem dados invalidos antes da regra de negocio.

10. PERSISTENCIA

A persistencia usa Spring Data JPA e JpaRepository.
Repositories principais:
- ClienteRepository
- MotoristaRepository
- VeiculoRepository
- ViagemRepository
- CheckpointRepository
- UsuarioSistemaRepository

O banco configurado e Oracle SQL.
As credenciais ficam protegidas por variaveis de ambiente.

11. MODELAGEM AVANCADA

O projeto contempla:
- Heranca com PessoaLogistica usando @MappedSuperclass.
- Objetos embutidos com @Embedded e @Embeddable.
- Rota e Coordenada como objetos embutidos.
- Chave composta com UsuarioSistemaId usando @EmbeddedId.
- Relacionamentos @ManyToOne e @OneToMany.
- Multiplas tabelas relacionadas.

Relacionamentos principais:
- Viagem possui Cliente, Motorista e Veiculo.
- Checkpoint pertence a uma Viagem.

12. TRATAMENTO DE ERROS

O projeto possui GlobalExceptionHandler com @RestControllerAdvice.
Ele trata recurso nao encontrado, regra de negocio, credenciais invalidas, acesso negado, validacao e erro interno.
As respostas usam ErroResponse com status, mensagem, caminho e campos invalidos.

13. SEGURANCA E JWT

A API usa Spring Security com JWT.
Rotas publicas:
- /
- /health
- /auth/**
- /swagger-ui/**
- /v3/api-docs/**
- /error

As demais rotas exigem Bearer Token.
O JWTAuthFilter estende OncePerRequestFilter.
Ele valida o token e autentica o usuario nas rotas protegidas.

14. AUTENTICACAO

Cadastro:
POST /auth/register

Body:
{
  "usuario": "admin",
  "email": "admin@astrotrack.com",
  "senha": "senha123"
}

Login:
POST /auth/login

Body:
{
  "email": "admin@astrotrack.com",
  "senha": "senha123"
}

O login retorna um token JWT.

15. HATEOAS

O projeto usa Spring HATEOAS em endpoints dedicados:
- GET /hateoas
- GET /clientes/hateoas
- GET /motoristas/hateoas
- GET /veiculos/hateoas
- GET /viagens/hateoas
- GET /checkpoints/hateoas

Esses endpoints retornam links de navegacao para listar, buscar, criar, atualizar e remover recursos.

16. SWAGGER, CORS E DEPLOY

A documentacao usa SpringDoc OpenAPI.
Swagger:
/swagger-ui.html

OpenAPI JSON:
/v3/api-docs

O CORS esta configurado no SecurityConfig.
Para Render, o application.properties usa:
server.port=${PORT:8080}
server.address=0.0.0.0

Rotas publicas de health check:
- GET /
- GET /health

17. PERGUNTAS PROVAVEIS

Pergunta: Por que usar DTOs?
Resposta: Para separar o contrato da API das entidades JPA.

Pergunta: Onde ficam as regras de negocio?
Resposta: Nas classes Service.

Pergunta: Como a API protege os endpoints?
Resposta: Com Spring Security e JWT.

Pergunta: Como a modelagem avancada aparece?
Resposta: Com heranca, chave composta, objetos embutidos e relacionamentos JPA.

Pergunta: Como os erros sao padronizados?
Resposta: Pelo GlobalExceptionHandler, que retorna ErroResponse.

18. FRASE CURTA PARA APRESENTAR

O AstroTrack e uma API REST em Java 21 com Spring Boot 3 para logistica satelital. O projeto segue arquitetura em camadas, possui CRUD completo, DTOs com Java Records, validacao, JPA com Oracle, tratamento global de erros, Swagger, HATEOAS, CORS e seguranca com JWT. A modelagem contempla heranca, chave composta, objetos embutidos e relacionamentos entre multiplas tabelas.

