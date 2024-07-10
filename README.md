# Sistema Bancário Descentralizado 

## Problema 2 - Conectividade e Concorrência

### Autores
<div align="justify">
  <li><a href="https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes.git">@Adriel-Santana</a></li>
</div>

### Máquina
1. Sistema operacional:
  - Windows 10;
  - Ubuntu 22.04 LTS;
2. Linguagem de programação: python:3.11.4;
  - Bibliotecas nativas utilizadas:
    - flask;
    - socket
    - json;
    - request;
    - threading;
    - os
    - datetime
### 

### Instruções
Observação: O docker deve está previamente instalado na máquina.
1. Clone o repositório.
   ```sh
   git clone https://github.com/Pegasus77-Adriel/pb2_main.git
   ```
2. Abra a pasta atráves do explorador de arquivos da sua máquina.
3. Abra o terminal no diretório
   * Windows, use
   ```sh
   No campo de endereço, digite 'cmd' e pressione Enter
   ```
    * Linux, use
   ```sh
   Clique com o botão direito do mouse em uma área vazia dentro do diretório,
   Selecione "Abrir no terminal" ou uma opção semelhante.
   ```
4. Compile os arquivos com extensão .py em cada pasta do projeto.
* Abra a bradesco:
     ```sh
     cd bradesco
     ```
   * Compile o arquivo app.py:
     ```sh
       docker build -t app-1 .
     ```
   * retorne a pasta anterior:
       ```sh
       cd ..
      ```
* Abra a pasta nubank:
     ```sh
     cd nubank
     ```
   * Compile o arquivo app.py:
     ```sh
       docker build -t app-2 .
     ```
   * retorne a pasta anterior:
       ```sh
       cd ..
      ```
* Abra a pasta picpay:
     ```sh
     cd picpay
     ```
   * Compile o arquivo app.py:
     ```sh
       docker build -t app-3 .
     ```
   * retorne a pasta anterior:
       ```sh
       cd ..
      ```
5. Crie uma rede local docker.
* execute o comando :
     ```sh
     docker network create minha_rede_local
     ```
6. Execute os containers docker.
* execute a imagem app-1:
     ```sh
     docker run --network=minha_rede_local -p 59997:59997 -e SERVER-IP1='coloque o IP da máquina que o container do nubank está rodando' -e SERVER-IP2= 'coloque o IP... do picpay' -e SERVER-IP3= 'coloque o IP ... do bradesco' -it app-1 
     ```
* execute a imagem app-2:
     ```sh
       docker run --network=minha_rede_local -p 59997:59997 -e SERVER-IP1='coloque o IP da máquina que o container do nubank está rodando' -e SERVER-IP2= 'coloque o IP... do picpay' -e SERVER-IP3= 'coloque o IP ... do bradesco' -it app-2
     ```
* execute a imagem app-3:
     ```sh
       docker run --network=minha_rede_local -p 59997:59997 -e SERVER-IP1='coloque o IP da máquina que o container do nubank está rodando' -e SERVER-IP2= 'coloque o IP... do picpay' -e SERVER-IP3= 'coloque o IP ... do bradesco' -it app-3
      ```
# Gerenciamento inteligente descentralizado de transações bancárias 
O propósito central deste sistema é otimizar o gerenciamento entre diversas contas bancárias de um mesmo cliente em diferentes bancos, além de oferecer suporte para realizar um conjunto de operações simultaneamente. Isso abrange não apenas a capacidade da aplicação de consultar dados remotamente do mesmo cliente em diferentes bancos, mas também de exercutar operações bancárias de forma eficaz, sendo elas depositar, sacar e transferir. Para alcançar esse propósito, o sistema incorpora o protocolo de comunicação HTTP, esse concebido para assegurar a confiabilidade do serviço.

## 2. Solução do problema
No desenvolvimento do sistema foi utilizado a linguagem de programação Python na versão 3.11.4, bem como as funcionalidades incluídas nas bibliotecas nativas da linguagem, além do framework Flask para implementação da API Rest.

## 2.1 Comunicação entre bancos
Para a comunicação do banco [bradesco](https://github.com/Pegasus77-Adriel/pb2_main/tree/main/bradesco), [nubank](https://github.com/Pegasus77-Adriel/pb2_main/tree/main/nubank) e [picpay](https://github.com/Pegasus77-Adriel/pb2_main/tree/main/picpay) foi utilizado o protocolo de comunicação HTTP, inclusive um padrão na troca de mensagens adotados por eles, como é mostrado no diagrama abaixo:

![diagrama sensor e servidor](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/diagrama%20servidor%20e%20sensor.png)

## 2.2 Protocolos UDP e TCP
Nesse projeto entre os sensores e o servidor foram utilizados 2 tipos de protocolos de comunicação: 
- Protocolo UDP: é um protocolo de comunicação da camada de transporte que oferece um serviço de entrega de dados rápido e eficiente, mas não garante a entrega ou a ordem dos pacotes. Ele é amplamente utilizado em situações onde a perda ocasional de dados não é crítica, como é o caso do envio de dados dos sensores para o servidor.
- Protocolo TCP-IP: é um protoclo que garante a entrega confiável de dados, verificando e retransmitindo pacotes perdidos. Além disso, o TCP controla o fluxo de dados e adapta a taxa de transmissão para evitar congestionamentos na rede. Sua orientação à conexão estabelece e encerra conexões de forma segura, garantindo a integridade dos dados transmitidos. Essas qualidades tornam o TCP uma escolha amplamente utilizada em aplicações que exigem comunicação confiável e orientada à conexão, por esse motivo esse protocolo foi escolhido para garantir que os comandos enviados pelo servidor sejam entregues de forma confiável para os sensores.
  
## 2.3 Servidor e Aplicação
Para a comunicação do [servidor](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/servidor/broker.py) com a [aplicação](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/aplicacao/app.py) foi utilizado o protocolo de comunicação HTTP através da implementação da API Rest, como pode ser visto no diagrama abaixo:
![diagrama servidor e app](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/diagrama%20servidor%20e%20app.png)
No diagrama acima por exemplo, pode ser notado uma das rota da API Rest, sendo essa utilizada pela aplicação para a alteração do intervalo de envio de dados constantes do sensor para o servidor, bem como pode ser visto também uma resposta do servidor para a respectiva requição da aplicação. 
## 2.4 protocolo HTTP e API Rest
- HTTP: é o protocolo usado para comunicação na web, que opera em um modelo cliente-servidor, onde o cliente faz requisições ao servidor, que responde com os dados solicitados. Geralmente utiliza os métodos de requisição como GET e POST, que são usados para receber e enviar dados pro servidor.
- API Rest: Em conjunto com o HTTP, as APIs REST proporcionam uma maneira eficiente e flexível para que aplicativos se comuniquem. Elas utilizam solicitações HTTP, incluindo os métodos GET, POST, PUT e DELETE, para realizar operações em recursos específicos. Esses recursos são acessados através de URLs e os dados geralmente são retornados nos formatos JSON ou XML. Essa abordagem padronizada facilita a integração entre sistemas e serviços web.
## 2.5 Threads
Os códigos do sensor e do servidor utilizam threads para receber e enviar mensagens atráves comunicação UDP e TCP respectivamente, de modo que não gere conflito ou atraso no processamento de dados.

Uma thread pode ser descrita como uma unidade básica de processamento dentro de um programa, capaz de executar tarefas relacionadas à aplicação principal de forma independente. A utilização de threads permite que várias atividades ocorram simultaneamente, o que faz com que diferentes partes do sistema executem suas tarefas quase ao mesmo tempo, embora isso possa criar uma impressão de paralelismo que não é totalmente verdade.

No código o sensor utiliza uma thread para enviar as medições constantimente via UDP para o servidor, e utiliza outra para receber os comandos via TCP provindos do servidor. Já o servidor, usa uma thread para receber os dados dos sensores via UDP, outra thread para realizar conexões TCP com os sensores, e por fim utiliza outra para comunicação com a aplicação. 
## 2.6 Sensor
O [sensor](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/sensores/dispositivos.py) tem o papel de simular dados como temperatura e umidade, ajustando esses valores de forma aleatória para garantir uma variação constante. Esses dados gerados são então enviados via UDP de maneira regular para o servidor em intervalos de tempo predefinidos, os dados são enviados no formato JSON.
## 2.7 Servidor
O [servidor](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/servidor/broker.py)
é responsável por receber os dados dos sensores e armazena-los em um buffer interno, além disso tem o papel de tratar as requisições (requests) da aplicação, por meio do processamento das informações e de enviar a resposta adequada para cada solicitação. Desse modo, o servidor interage com a aplicação através da API Rest (HTTP), e recebe dados dos sensores via UDP, bem como envia comandos para os sensores via TCP.

As rotas possíveis para comunicação com a aplicação:
- `/alterar_temp_amostragem/<segundos>/<matricula>`
  
  Usada para alterar o intervalo de tempo de envio de um determinado sensor para o servidor.

  É necessário indicar os **segundos** e a **matricula** do sensor.

- `/enviar_comando/<comando>/<matricula>`
  
  Usada para enviar o comando de **ligar** ou **desligar** para um sensor.

  É necessário indicar o **comando** e a **matricula** do sensor.

- `/receber_medicao/<matricula>`
  
  Usada para solicitar a ultima medição de um determinado sensor.

  É necessário indicar apenas a **matricula**.
  
## 2.8 Aplicação

A [aplicação](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/aplicacao/app.py) tem como objetivo fazer o gerenciamento dos sensores, utilizando-se do servidor como intermediário para a comunicação entre as duas partes (sensor e aplicação). Por esse motivo, a aplicação é capaz de realizar comandos de ligar ou desligar um sensor facilmente atráves das requisições enviadas ao servidor, do mesmo modo que consegue receber dados remotamente de um sensor. Logo abaixo, pode ser visto o diagrama completo unindo todas 3 entidades do projeto, sensor, servidor e aplicação.
![diagrama geral](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/diagrama%20completo.png)

## 3. Melhorias Futuras
O sistema é totalemente funcional, porém pode ser melhorado para se tornar mais eficiente. Entre as melhorias possíveis podem ser citadas:
- Adicionar um tempo para o servidor tente se reconectar com sensor de forma autonôma sem gerar erro;
- Adicionar mais funcionalidades além das que já estão implementas para o gerenciamento dos sensores;
- Criar uma interface gráfica para tornar a experiência do usuário a melhor possível.



