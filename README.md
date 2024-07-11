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
2. Linguagem de programação: python:3.11.4, javascript, html e css;
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
No desenvolvimento do sistema foi utilizado a linguagem de programação Python na versão 3.11.4, bem como as funcionalidades incluídas nas bibliotecas nativas da linguagem, além do framework Flask para implementação da API Rest e o SQLite3 para o banco de dados.

## 2.1 Comunicação entre bancos
Para a comunicação do banco [bradesco](https://github.com/Pegasus77-Adriel/pb2_main/tree/main/bradesco), [nubank](https://github.com/Pegasus77-Adriel/pb2_main/tree/main/nubank) e [picpay](https://github.com/Pegasus77-Adriel/pb2_main/tree/main/picpay) foi utilizado o protocolo de comunicação HTTP, inclusive os códigos padronizados usados pelos servidores web para indicar a resposta a uma solicitação feita pelo cliente.

## 2.2 Protocolo HTTP
Nesse projeto foi utilizado o protocolo HTTP (Hypertext Transfer Protocol), que é um protocolo de comunicação utilizado na web para a transferência de dados entre clientes (como navegadores) e servidores. Ele define como as mensagens são formatadas e transmitidas, bem como as ações que os servidores e navegadores devem executar em resposta a vários comandos.
  - Métodos HTTP Utilizados: O mais utilizado foi o método POST, usado esse em muitos casos para enviar comandos, como depositar em uma conta em outro banco, além de repassar os dados necessários para o processamento da operação para o banco de destino. Em contrapartida, foi implementado o método GET, sendo ele usado para fazer requisições para consultar dados, por exemplo o saldo da conta de um cliente.
    
Algumas das rotas possíveis para comunicação interna e externa dos bancos:
  - `/signup`
    
     Usada para criação de uma nova conta bancária de um cliente.
  
  - `/login`
    
     Usada para autenticação do login do cliente
  
  - `/account/<cpf>`
    
     Usada para solicitar todas contas bancárias que contém um determinado CPF em todos os bancos.

    É necessário indicar apenas o **CPF**.

 - `/account/contas/<cpf>`
    
     Usada para solicitar todas contas bancárias que contém um determinado CPF em único banco.

     É necessário indicar apenas o **CPF**.

 - `/account/sacar/<cpf>/<valor>`
    
     Usada para realizar a operação de sacar dinheiro de uma conta.

     É necessário indicar o **CPF** e também o **valor** a ser sacado.

 - `/account/depositar/<cpf>/<valor>`
    
     Usada para realizar a operação de depositar dinheiro de uma conta.

     É necessário indicar o **CPF** e também o **valor** a ser depositado

 - `/account/transferir/<cpf1>/<cpf2>/<valor1>/<valor2>/<banco1>/<banco2>`
    
     Usada para realizar a operação de transferencia de uma ou duas contas simultaneamente.

     É necessário indicar os **CPF** dos destinatários, os respectivos **valores** a serem transferidos e também seus correspondentes **bancos**
    

## 2.3 Atomicidade e segurança
- ROLLBACK: esse comando foi comumente utilizado nas interações com o banco de dados para reverter uma transação que foi iniciada, mas não foi concluída. Quando uma transação é revertida, todas as alterações feitas durante essa transação são desfeitas, retornando o banco de dados ao seu estado anterior ao início da transação, isso tudo afim de garantir a atomicidade das operações.
- LOCK: foi utilizado em parceira com o **ROLLBACK** e tem por objeitvo garantir que apenas uma thread ou processo possa acessar um recurso compartilhado por vez, como por exemplo o bancos de dados e algumas rotas da API Rest.

## 2.3 Interface 
O sistema conta com duas telas principais como pode pode ser visto abaixo:




<p align="center">
  <strong>Tela de Cadastramento e Login</strong>
</p>
<p align="center">
  <img src="https://github.com/Pegasus77-Adriel/pb2_main/blob/main/tela_inicial.png" alt="tela inicial">
</p>




<p align="center">
  <strong>Tela Interna de Operações</strong>
</p>
<p align="center">
  <img src="https://github.com/Pegasus77-Adriel/pb2_main/blob/main/tela_interna.png" alt="tela interna">
</p>

  
## 2.8 Aplicação

A [aplicação](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/aplicacao/app.py) tem como objetivo fazer o gerenciamento dos sensores, utilizando-se do servidor como intermediário para a comunicação entre as duas partes (sensor e aplicação). Por esse motivo, a aplicação é capaz de realizar comandos de ligar ou desligar um sensor facilmente atráves das requisições enviadas ao servidor, do mesmo modo que consegue receber dados remotamente de um sensor. Logo abaixo, pode ser visto o diagrama completo unindo todas 3 entidades do projeto, sensor, servidor e aplicação.
![diagrama geral](https://github.com/Pegasus77-Adriel/Gerenciamento-de-Sensores-Inteligentes/blob/main/diagrama%20completo.png)

## 3. Melhorias Futuras
O sistema é totalemente funcional, porém pode ser melhorado para se tornar mais eficiente. Entre as melhorias possíveis podem ser citadas:
- Adicionar um tempo para o servidor tente se reconectar com sensor de forma autonôma sem gerar erro;
- Adicionar mais funcionalidades além das que já estão implementas para o gerenciamento dos sensores;
- Criar uma interface gráfica para tornar a experiência do usuário a melhor possível.



