# AstroTrack

**IMPORTANTE** -> Se faz necessário compilar o projeto com o ip da maquina quando for rodar! Segue o passo a passo:
Abrir o arquivo html e o sketch.ino e achar onde está o campo com o IP em ambos (192.xx.xx.x) e trocar pelo ip atual da máquina.
> Para saber o IP basta abrir o terminal de comando do windows e rodar o comando ´ipconfig´, ele aparecerá mais ou menos assim:
> ```
> Adaptador Ethernet Ethernet:
>   Sufixo DNS específico de conexão. . . . . . :
>   Endereço IPv4. . . . . . . .  . . . . . . . : 192.168.10.4 ---> ESTE AQUI É O IP que deve ser trocado no código
>   Máscara de Sub-rede . . . . . . . . . . . . : 255.255.255.0
>   Gateway Padrão. . . . . . . . . . . . . . . : 192.168.10.1
> ```
Após trocar o ip, se faz necessário compilar o projeto novamente: 
- Verfique se a placa está identifica corretamente (esp32 DevModule)
- Verifique se todas as esxtensões necessárias estão instaladas
- No meno superior clique em ´Sketch´
- Por fim em ´Export Compiled Binary´

O AstroTrack é uma solução de monitoramento logístico de ponta a ponta, desenvolvida para modernizar a gestão de frotas e a segurança de motoristas da Global Solution. O conceito central gira em torno de uma "logística orbital", onde cada veículo é tratado como uma unidade em missão, monitorado por telemetria constante.

## OBJETIVO DO PROJETO

O AstroTrack é uma solução de IoT (Internet of Things) projetada para rastrear 
a localização de veículos de carga e monitorar eventos críticos, como a 
abertura não autorizada do compartimento de carga e o acionamento de botões de 
pânico pelo motorista.

## ARQUITETURA DO SISTEMA

O sistema segue um modelo de arquitetura Edge-Gateway-Frontend:

- EDGE (ESP32): Dispositivo de hardware responsável pela coleta de dados dos 
  sensores, processamento local básico e envio de telemetria via Wi-Fi.
- GATEWAY/BACKEND (Mock API): Um servidor intermediário que recebe os dados 
  via HTTP POST e os armazena. Atualmente implementado com 'json-server' para 
  prototipação rápida.
- FRONTEND (Dashboard): Interface web que consome os dados do backend via 
  HTTP GET e apresenta o status em tempo real ao operador.

## ESTRUTURA DE PASTAS

/astrotrack_esp32/
  - astrotrack_esp32.ino: Código-fonte principal (firmware).
  - diagram.json: Esquema de ligações de hardware (compatível com Wokwi).
  - libraries.txt: Lista de bibliotecas necessárias para compilação.
  - wokwi.toml: Configuração da simulação.

/dashboard/
  - index.html: Interface do usuário (UI) e lógica de consumo da API.
  - db.json: Banco de dados simulado para o json-server.

## DETALHES TÉCNICOS - HARDWARE (ESP32)
Componentes Utilizados:
- Microcontrolador: ESP32.
- Display: LCD 16x2 com interface I2C (Endereço 0x27).
- Sensores: 
    * Botão de Pânico (Digital Input - GPIO 12).
    * Sensor de Porta (Digital Input - GPIO 13).
- Atuadores:
    * LED de Pânico (GPIO 2).
    * LED de Sistema/Status (GPIO 4).

### Bibliotecas Utilizadas:
- WiFi.h: Gerenciamento da conexão sem fio.
- HTTPClient.h: Realização de requisições POST para a API.
- LiquidCrystal_I2C.h: Controle do display LCD via protocolo I2C.
- Wire.h: Comunicação I2C.

### Lógica de Funcionamento:
O firmware lê os sensores em um loop constante. Sempre que ocorre uma mudança 
de estado (porta aberta/fechada ou pânico acionado) ou a cada 5 segundos 
(intervalo de segurança), o dispositivo envia um JSON para o servidor.

## DETALHES TÉCNICOS - BACKEND (API)
A comunicação é feita via HTTP REST.
Endpoint: http://[IP_SERVIDOR]:3000/telemetry

Estrutura do JSON enviado:
```json
{
  "deviceId": "ASTRO_001",
  "panic": true/false,
  "door_open": true/false,
  "latitude": -23.5505,
  "longitude": -46.6333,
  "timestamp": 12345678
}
```
> Nota para o Back-end: No ambiente de produção, o json-server deve ser 
substituído por uma API robusta (Node.js, Python/FastAPI, etc.) que valide 
os dados e os persista em um banco de dados relacional ou de séries temporais.

## DETALHES TÉCNICOS - FRONTEND (DASHBOARD)
Tecnologias: HTML5, CSS3 (Vanilla), JavaScript (ES6+).

#### Funcionalidades:
- Polling: A cada 2 segundos, o dashboard faz um GET no endpoint de telemetria.
- Visualização Dinâmica: 
    * Cards de status mudam de cor e ícone baseados nos alertas.
    * Animações de "pulse" indicam que o sistema está online.
    * Formatação de coordenadas GPS para exibição amigável.

#### Estilo:
- Tipografia: Fonte 'Inter' via Google Fonts.
- Ícones: FontAwesome 5.
- Design: Dark Mode com elementos translúcidos (Glassmorphism).

## POR QUE ESTAS ESCOLHAS?
- JSON: Formato padrão de mercado, facilitando a integração entre C++ e Web.
