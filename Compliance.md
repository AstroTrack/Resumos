# ASTROTRACK – LOGÍSTICA SATELITAL

## Introdução

O AstroTrack é uma solução de logística satelital desenvolvida para permitir o monitoramento contínuo de veículos e cargas em regiões sem cobertura de redes móveis convencionais, como 4G e 5G. A proposta utiliza dispositivos IoT instalados nos veículos para coletar dados de localização GPS e informações operacionais, transmitindo esses dados através de comunicação via satélite para uma plataforma centralizada.

A solução foi criada para atender principalmente empresas de transporte, logística e agronegócio, permitindo maior segurança das cargas, monitoramento em tempo real e melhoria da eficiência operacional.

---

## Visão da Arquitetura

A Visão da Arquitetura foi construída com base nos conceitos de Stakeholders, Drivers, Goals, Requirements e Constraints.

### Stakeholders

Os principais stakeholders identificados foram:

* Empresa de Transporte
* Gestor Logístico
* Motorista
* Cliente Contratante

### Drivers

Os drivers representam os fatores que motivam o projeto:

* Falta de cobertura móvel em regiões remotas.
* Necessidade de aumentar a segurança das cargas.
* Busca por maior eficiência logística.

### Objetivos

Com base nos drivers foram definidos os seguintes objetivos:

* Garantir rastreamento contínuo 24 horas por dia.
* Reduzir perdas e incidentes envolvendo cargas.
* Melhorar a eficiência logística das operações.

### Requisitos

Para atingir os objetivos foram definidos os seguintes requisitos:

* Rastreamento em tempo real.
* Comunicação satelital.
* Gestão de alertas.
* Armazenamento histórico das operações.

### Restrições

Os requisitos possuem restrições operacionais específicas:

* Atualização da localização a cada 60 segundos.
* Disponibilidade mínima de 99,9%.
* Precisão GPS inferior a 10 metros.
* Retenção dos dados por 5 anos.

---

## Arquitetura de Negócio

A Arquitetura de Negócio representa o fluxo operacional da solução.

O processo inicia com o evento "Viagem Iniciada", que dispara o processo principal denominado "Monitoramento Logístico".

Durante a execução do processo são realizadas as seguintes atividades:

1. Capturar posição GPS.
2. Coletar dados da carga.
3. Transmitir dados via satélite.
4. Processar informações recebidas.
5. Atualizar o dashboard de monitoramento.
6. Gerar alertas operacionais.

Cada atividade produz informações importantes para a operação:

* Dados de Localização.
* Pacote de Telemetria.
* Registro de Rastreamento.
* Painel Atualizado.
* Notificação Operacional.

O processo termina com o evento "Viagem Monitorada com Sucesso".

Também foram definidos os atores responsáveis pelas atividades:

* Administrador.
* Motorista.
* Gestor Logístico.
* Analista de Operações.

---

## Arquitetura de Sistema

A Arquitetura de Sistema foi organizada em três camadas.

### Camada de Apresentação

Responsável pela interação dos usuários com a solução.

Componentes:

* Dashboard Web.
* Aplicativo Mobile.
* Portal Administrativo.

### Camada de Aplicação

Responsável pelas regras de negócio.

Componentes:

* API AstroTrack.
* Serviço de Rastreamento.
* Serviço de Alertas.
* Serviço de Autenticação.
* Serviço de Relatórios.

### Camada de Dados

Responsável pelo armazenamento das informações.

Componentes:

* PostgreSQL.
* Veículo.
* Motorista.
* Localização.
* Carga.
* Alerta.
* Usuário.
* Histórico de Rotas.

Essa arquitetura garante separação adequada das responsabilidades, facilitando manutenção, escalabilidade e segurança.

---

## Arquitetura de Tecnologia

A Arquitetura de Tecnologia representa a infraestrutura necessária para operação da solução.

Os componentes tecnológicos definidos foram:

* Dispositivo IoT.
* Rede Satelital.
* Satélite.
* Gateway Satelital.
* Internet.
* Firewall.
* Load Balancer.
* Servidor de Aplicação.
* Servidor de Banco de Dados.
* Servidor de Backup.

O fluxo operacional ocorre da seguinte forma:

O dispositivo IoT instalado no veículo coleta os dados de localização e os envia para a rede satelital. Os dados passam pelo satélite e pelo gateway satelital, chegando à internet. Em seguida passam pelo firewall, load balancer e servidor de aplicação, onde são processados e armazenados no banco de dados. Por fim, os dados são replicados para o servidor de backup.

Essa estrutura garante disponibilidade, segurança e confiabilidade para o sistema.

---

## Conclusão

O AstroTrack apresenta uma solução completa para monitoramento logístico em regiões sem cobertura móvel. A utilização de comunicação via satélite permite rastreamento contínuo, maior segurança para as cargas e melhor eficiência operacional.

A arquitetura proposta foi estruturada seguindo os conceitos do TOGAF e ArchiMate, contemplando visão estratégica, processos de negócio, componentes de software e infraestrutura tecnológica necessários para o funcionamento da solução.
