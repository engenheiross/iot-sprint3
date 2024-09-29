# IoT Monitoring System with MQTT, STH-Comet, Orion Broker, and FIWARE with Docker

Este projeto envolve um dispositivo IoT que coleta dados de ambiente e os envia para um servidor MQTT. O sistema integra **FIWARE**, utilizando Docker para executar o **Orion Broker** e o **STH-Comet**, que gerenciam e armazenam os dados enviados. Além disso, um painel Dash é utilizado para visualização dos dados em tempo real.

## Descrição do Projeto

O dispositivo IoT está equipado com:
- **Sensor DHT-22**: Mede a umidade e a temperatura do ambiente.
- **Sensor LDR**: Mede a luminosidade do ambiente.
- **ESP32**: Microcontrolador responsável por gerenciar os sensores e enviar os dados para o servidor MQTT.

Esses sensores coletam dados periodicamente, que são enviados ao servidor MQTT como atributos individuais. O servidor FIWARE, utilizando Orion Broker e STH-Comet, é responsável por gerenciar esses dados e permitir que sejam acessados via requisições HTTP (`GET` requests).

O dispositivo também possui um atributo adicional chamado **state**, que indica se o dispositivo está ligado ou desligado.

Além disso, um servidor Dash está configurado para exibir as informações de temperatura, umidade e luminosidade em tempo real.

### Simulação no WokWi

![image](https://github.com/user-attachments/assets/b600dd3f-8123-451f-91bf-447dc95c23a7)

Para testar a integração com o servidor Dash e monitorar os dados, um link de simulação do **ESP32** foi disponibilizado através do **WokWi**. Com essa simulação, é possível visualizar o funcionamento do sistema sem a necessidade de hardware físico.

> [Clique aqui para acessar a simulação no WokWi](https://wokwi.com/projects/410386205165527041)

## Tecnologias Utilizadas

- **ESP32**: Microcontrolador responsável pela coleta e envio de dados dos sensores.
- **MQTT**: Protocolo de comunicação para enviar e receber dados do dispositivo IoT.
- **FIWARE**: Conjunto de componentes usados para gerenciar os dados IoT, incluindo o Orion Broker e o STH-Comet.
- **STH-Comet**: Armazena séries temporais de dados dos atributos enviados.
- **Orion Broker**: Facilita a comunicação e a gestão dos atributos enviados pelo dispositivo IoT.
- **Docker**: FIWARE e seus componentes são executados dentro de containers Docker.
- **Dash**: Interface web para visualização em tempo real dos dados coletados pelos sensores.
- **Postman**: Utilizado para testar e verificar as requisições dos serviços, além de realizar o Health Check do sistema.
- **WokWi**: Simulador online para testar o ESP32 e visualizar a coleta de dados sem hardware físico.

## Postman Collection

Uma **Postman Collection** foi criada e está disponível neste repositório para facilitar o Health Check dos serviços e familiarizar os usuários com as requisições do sistema. O **IP do servidor** já está incluído dentro da collection, o que facilita a configuração das requisições. Além disso, esse IP também pode ser utilizado para acessar a dashboard online, que está disponível na **porta 8050**.

IP do servidor: **191.232.39.114**

Essa collection inclui os seguintes endpoints:

1. **Health Check**: Verifica se o serviço MQTT está rodando corretamente.
2. **Provisioning a Service Group for MQTT**: Provisão de um grupo de serviços MQTT.
   - **2.1 Health Check Services**: Verifica os serviços `fiware-service` e `fiware-servicepath`.
   - **2.2 Delete a Service Group**: Remove um grupo de serviços existente.
3. **Provisioning a Smart Lamp**: Provisiona uma Smart Lamp como dispositivo IoT.
4. **Registering Smart Lamp Commands**: Registra a Smart Lamp nas entities do Orion Broker.
5. **List all Devices Provisioned**: Lista todos os dispositivos provisionados no sistema.
6. **Switching on the Smart Lamp**: Liga a Smart Lamp.
7. **Result of Smart Lamp Luminosity**: Faz uma requisição GET para verificar a medida de luminosidade armazenada no servidor.

A collection pode ser usada para executar as requisições diretamente e verificar se o sistema está funcionando conforme o esperado. Ela também é uma ótima ferramenta para novos desenvolvedores se familiarizarem com a API e os fluxos de comunicação.

## Funcionalidades

1. **Coleta de Dados**: 
   - O sensor DHT-22 coleta dados de umidade e temperatura.
   - O sensor LDR coleta dados de luminosidade.
   
2. **Comunicação com MQTT**:
   - Cada atributo (umidade, temperatura, luminosidade, e estado) é enviado ao servidor MQTT separadamente.
   
3. **Gestão de Dados com FIWARE**:
   - O FIWARE, utilizando o Orion Broker e o STH-Comet, gerencia os dados IoT. Esses dados são armazenados e podem ser acessados via `GET` requests para séries temporais.
   
4. **Visualização em Tempo Real**:
   - O servidor Dash permite visualizar gráficos de temperatura, umidade, e luminosidade, com atualizações constantes.
   
5. **Estado do Dispositivo**:
   - Atributo `state` informa se o dispositivo está ligado ou desligado.

## Estrutura do Projeto

- **`iot_device/`**: Código e configuração do dispositivo IoT.
  - Sensores: DHT-22 e LDR.
  - Envio dos dados ao servidor MQTT.
- **`dash_server/`**: Servidor Dash para exibir os dados em tempo real.
  - Gráficos de temperatura, umidade, e luminosidade.
  - Integração com o servidor MQTT para obter os dados em tempo real.
- **`fiware/`**: Configurações e scripts Docker para iniciar o FIWARE (Orion Broker e STH-Comet).
- **`postman_collection/`**: Arquivos da Postman Collection para testes e Health Check.

## Como Executar

1. **Configuração do FIWARE com Docker**:
   - Na pasta `fiware/`, execute o Docker Compose para iniciar os containers do FIWARE (Orion Broker e STH-Comet):
     ```bash
     docker-compose up -d
     ```
   - Certifique-se de que o Orion Broker e o STH-Comet estão funcionando corretamente. Você pode verificar o status dos containers com:
     ```bash
     docker ps
     ```
   
2. **Configuração do Servidor MQTT**:
   - O servidor MQTT está rodando com suporte ao Orion Broker e STH-Comet.
   - O IP do servidor é utilizado tanto nas requisições da Postman Collection quanto para acessar a dashboard pela porta 8050.
   - Certifique-se de que os atributos do dispositivo estão registrados no Orion Broker.

3. **Executando o Dispositivo IoT (ESP32)**:
   - O código do dispositivo IoT está na pasta `iot_device/`.
   - Carregue o código no ESP32 e configure a conexão com o servidor MQTT.
   - **Simulação no WokWi**: Caso não possua o hardware, use o link de simulação no WokWi para testar o sistema.

4. **Executando o Servidor Dash**:
   - O código para o servidor Dash está na pasta `dash_server/`.
   - Instale as dependências necessárias utilizando o comando:
     ```bash
     pip install -r requirements.txt
     ```
   - Execute o servidor utilizando:
     ```bash
     python app.py
     ```

5. **Executando a Postman Collection**:
   - Importe a **Postman Collection** que está na pasta `postman_collection/` no Postman.
   - Execute as requisições para testar o Health Check e a configuração dos dispositivos IoT.

6. **Visualização dos Dados**:
   - Acesse a interface do Dash no navegador através de `http://191.232.39.114:8050` para visualizar os gráficos de temperatura, umidade, e luminosidade.

## Requisitos

- **Hardware**: 
  - Microcontrolador **ESP32**.
  - Sensor DHT-22.
  - Sensor LDR.
  
- **Software**:
  - Servidor MQTT com suporte a Orion Broker e STH-Comet.
  - **FIWARE com Docker**:
    - Orion Broker.
    - STH-Comet.
  - Postman para execução das requisições.
  - Python 3.x para o servidor Dash.
  - **WokWi**: Para simulação do ESP32.

## Dependências

No servidor Dash, certifique-se de instalar as seguintes bibliotecas Python:

```bash
pip install dash plotly pandas requests
```

## Futuras Melhorias
- Adicionar notificações ao usuário quando certos limites de temperatura, umidade ou luminosidade forem atingidos.
- Melhorar a interface do Dash para incluir controle do dispositivo (ligar/desligar) via MQTT.
