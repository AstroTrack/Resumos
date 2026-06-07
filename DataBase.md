# AstroTrack - Documentação Detalhada do Banco de Dados

## 1. Visão Geral

O **AstroTrack** é uma solução de monitoramento logístico via satélite desenvolvida para a **Global Solution 2026/1**. O projeto foi criado com o objetivo de demonstrar a aplicação de tecnologias espaciais no setor logístico, permitindo o acompanhamento de veículos e cargas mesmo em regiões sem cobertura terrestre.

A proposta central é utilizar satélites como meio de comunicação para registrar informações de viagens, checkpoints e alertas operacionais, garantindo maior visibilidade e segurança das operações.

### O Problema

Empresas de transporte frequentemente enfrentam dificuldades para monitorar veículos em áreas remotas. A ausência de sinal de telefonia ou internet dificulta o rastreamento em tempo real e reduz a capacidade de resposta diante de situações críticas.

### A Solução

O AstroTrack centraliza todas as informações logísticas em um banco de dados estruturado, permitindo:

* Cadastro de clientes, motoristas e veículos;
* Controle completo de viagens;
* Registro de checkpoints de localização;
* Emissão de alertas operacionais;
* Associação de satélites ao monitoramento;
* Geração de relatórios gerenciais;
* Automatização de processos através de PL/SQL.

---

## 2. Modelagem de Dados

### 2.1. Decisões de Modelagem

A modelagem foi construída utilizando o Oracle SQL Data Modeler seguindo princípios de normalização e integridade referencial.

Optamos por separar as informações em entidades específicas para evitar redundância de dados e facilitar futuras expansões do sistema.

### Entidades Principais

#### AT_CLIENTES

Responsável pelo armazenamento dos clientes que contratam os serviços logísticos.

**Decisão:** manter uma entidade exclusiva para clientes permite reutilização em múltiplas viagens sem duplicação de dados.

#### AT_MOTORISTAS

Armazena informações dos motoristas responsáveis pelas operações.

**Decisão:** um motorista pode realizar diversas viagens ao longo do tempo, justificando uma relação 1:N com a tabela de viagens.

#### AT_VEICULOS

Responsável pelo cadastro dos veículos monitorados.

**Decisão:** separar veículos das viagens possibilita o reaproveitamento dos registros e o acompanhamento histórico da utilização da frota.

#### AT_VIAGENS

Tabela central do sistema.

**Decisão:** todas as operações importantes do projeto convergem para a viagem, tornando-a o núcleo da modelagem.

#### AT_CHECKPOINTS

Registra os pontos de monitoramento coletados durante o trajeto.

**Decisão:** modelagem 1:N com viagens para armazenar múltiplas posições geográficas ao longo do percurso.

#### AT_ALERTAS

Armazena eventos relevantes ocorridos durante uma viagem.

Exemplos:

* Desvio de rota;
* Excesso de velocidade;
* Situação de emergência;
* Parada não programada.

#### AT_SATELITES

Representa os satélites responsáveis pela comunicação e monitoramento.

**Decisão:** incluir essa entidade conecta diretamente o projeto ao tema da Economia Espacial.

#### AT_USUARIOS_SISTEMA

Controla os usuários autorizados a acessar a plataforma.

---

## 3. Integridade e Relacionamentos

O modelo utiliza:

* Chaves Primárias (PK);
* Chaves Estrangeiras (FK);
* Restrições NOT NULL;
* Restrições UNIQUE;
* Integridade Referencial.

### Principais Relacionamentos

* Cliente → Viagens (1:N)
* Motorista → Viagens (1:N)
* Veículo → Viagens (1:N)
* Viagem → Checkpoints (1:N)
* Viagem → Alertas (1:N)
* Satélite → Checkpoints (1:N)

Essa estrutura garante consistência dos dados e evita registros órfãos.

---

## 4. Implementação Oracle

### DDL (Data Definition Language)

O DDL foi gerado inicialmente pelo Oracle SQL Data Modeler e posteriormente ajustado para atender às necessidades do projeto.

Foram criadas:

* 8 tabelas relacionais;
* Chaves primárias;
* Chaves estrangeiras;
* Restrições de integridade.

### DML (Data Manipulation Language)

Foi criada uma carga inicial de dados contendo registros suficientes para simular um ambiente operacional real.

Os dados representam:

* Clientes;
* Motoristas;
* Veículos;
* Viagens;
* Checkpoints;
* Alertas;
* Satélites;
* Usuários.

---

## 5. Programação PL/SQL

### Procedures

As procedures foram desenvolvidas para automatizar operações do sistema.

**Por que utilizar Procedures?**

* Centralização das regras de negócio;
* Redução de código repetido;
* Maior controle sobre as operações realizadas no banco.

### Functions

As functions retornam informações calculadas a partir dos dados armazenados.

**Benefícios:**

* Reutilização de lógica;
* Facilidade na construção de relatórios;
* Encapsulamento de cálculos.

### Triggers

As triggers executam ações automaticamente quando determinados eventos ocorrem.

**Objetivo:**

Garantir consistência e automatizar processos sem depender da aplicação.

### Package

As rotinas foram agrupadas em packages para melhorar organização e manutenção.

**Benefícios:**

* Código mais modular;
* Maior reutilização;
* Melhor desempenho.

---

## 6. Cursores e Blocos Anônimos

### Cursores Explícitos

Foram implementados cursores explícitos para demonstrar processamento linha a linha de conjuntos de registros.

**Por que utilizar cursores?**

Permitem percorrer resultados de consultas de forma controlada, possibilitando validações e processamentos específicos.

### Blocos Anônimos

Foram desenvolvidos blocos anônimos contendo:

* Estruturas condicionais;
* Estruturas de repetição;
* Manipulação de variáveis;
* Consultas SQL.

Esses blocos servem para demonstrar os recursos de programação oferecidos pelo Oracle PL/SQL.

---

## 7. Modelagem NoSQL

Além da modelagem relacional, o projeto apresenta uma estrutura NoSQL baseada em documentos JSON.

### Motivo da Escolha

Em sistemas de rastreamento é comum consultar todas as informações de uma viagem simultaneamente.

Por esse motivo, o modelo NoSQL agrupa em um único documento:

* Cliente;
* Motorista;
* Veículo;
* Satélites;
* Checkpoints;
* Alertas;
* Estatísticas da viagem.

### Benefícios

* Menor quantidade de consultas;
* Maior velocidade de leitura;
* Estrutura flexível;
* Adequação para sistemas de monitoramento em tempo real.

---

## 8. Relatórios e Indicadores

Foram desenvolvidos relatórios SQL utilizando JOINs para cruzamento de informações entre múltiplas tabelas.

Os relatórios permitem analisar:

* Viagens por cliente;
* Viagens por motorista;
* Utilização de veículos;
* Alertas registrados;
* Histórico de monitoramento.

Essas informações podem auxiliar gestores na tomada de decisões e no acompanhamento operacional.

---

## 9. Conclusão

O AstroTrack demonstra a aplicação prática de modelagem relacional e não relacional em um cenário realista de logística baseada em comunicação via satélite.

O projeto permitiu aplicar conceitos fundamentais de banco de dados, incluindo modelagem, SQL, PL/SQL, procedures, functions, triggers, packages, cursores e modelagem NoSQL.

Além de atender aos requisitos da disciplina, a solução apresenta uma aplicação alinhada ao tema da Economia Espacial, evidenciando como tecnologias espaciais podem contribuir para a modernização das operações logísticas.
