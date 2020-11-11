<h1 align='center'>Robô Paralelo</h1>
<p align='center'>Projeto de um Sistema Surpervisório e Controlador para um Robô Paralelo</p>

## *Estrutura Física*

<p align='center'> <img src="./imagens/robo-imagem.png" alt='foto robô'/> </p>

O Sistema Opercional atua de acordo com a apuração dos dados por parte de um Microcontrolador (Arduino Due), que gerencia o posicionamento do conjunto de Carros estruturados de acordo com a arquitetura Linear-Paralela.
## *Processo*

- [x] Leitura de Encoders
- [x] Acionamento dos Carros
- [x] Implementação do Sistema Supervisório
- [x] Controle de Posição
- [ ] Implementação de modelo matemático

## *Descrição de Etapas*
- ### Leitura de Encoders
Com a leitura dos canais provenientes do hardware, realiza-se a formulação lógica, capaz de identificar o deslocamento dos atuadores em relação ao seu eixo.
#### Sequências
Os padrões sequenciais tem como referência as `Entrada 1` e `Entrada 2`, respectivamente responsáveis pelos `Canal 1` e `Canal 2`.

###### Ciclo positivo 
| Tempo | Entrada 1 | Entrada 2 |
|-------|-----------|-----------|
|*T1*|0|0|
|*T2*|1|0|
|*T3*|1|1|
|*T4*|0|1|

###### Ciclo negativo
| Tempo | Entrada 1 | Entrada 2 |
|-------|-----------|-----------|
|*T1*|0|0|
|*T2*|0|1|
|*T3*|1|1|
|*T4*|1|0|

#### Métodos Biblioteca
Com base em tais padrões sequenciais, foi desevolvida a Biblioteca <a href='./BIBLIOTECAS/ENCODER'>ENCODER</a>, com o intuito de automatizar o processo de medição dos sinais referentes aos canais digitais.

```c++
ENCODER(port1, port2);        //Método construtor.
registerRead();               //Retorna a leitura por registrador da Porta 2.
getPort1();                   //Retorna o endereço da Porta 1.
getPort2();                   //Retorna o endereço da Porta 2.
```
A biblioteca atua inserida em anexo a uma interrupção, parametrizada para `RISING`, aplicada ao `Canal 1` em cada Encoder.

- ### Acionamento dos Carros

A movimentação dos carros é realizada por meio da manipulação de portas `PWM` e `Digital`, que indicam potência e sentido de rotação, respectivamente.
#### Padrão de Movimentação
| Porta Digital | PWM | Movimentação |
|-------|-----------|-----------|
| 0 | 0 | Parado |
| 1 | 0 | Parado |
| 1 | 255 | Sentido Horário Máximo |
| 0 | 255 | Sentido Anti-Horário Máximo |

#### Métodos Biblioteca
A Biblioteca <a href="./BIBLIOTECAS/ACIONAMENTO">ACIONAMENTO</a> foi desenvolvida a fim de simplificar o processo de movimentação dos Carros.
  
```c++
ACIONAMENTO(portPWM, portDigital);        //Construtor define as portas como saída
OUT(int value);                           //Altera os valores das portas de saída de acordo com "value".
STOP();                                   //Anula os valores das portas de saída.
```

- ### Sistema Supervisório
Foi aplicado o programa <a href="https://github.com/AsafeSilva/PID-Tuner-Controller/tree/master/PIDTuner">PID Tuner</a> com adaptações.

#### Alterações
Adaptações foram realizadas para o emprego de três atuadores.
##### Layout


<p align='center'> <img src="./imagens/ihm.png" alt='Interface Homem-Máquina'/> </p>


##### Padrão de Comunicação
###### Microcontrolador -> Computador
**Entrada**: `'I' + carro + valor + '\n'`

**Saída**: `'O'+ carro + valor + '\n'`

###### Computador -> Microcontrolador
**KP**: ` 'P' + carro + valor + '\n'`

**KI**: ` 'I' + carro + valor + '\n'`

**KD**: `'D' + carro + valor +'\n'`

**Set Point**: `'S' + carro + valor + '\n'`


- ### Controle de Posição

Por meio da integração da <a href='./Leitura_de_encoders/Leitura_de_encoders.ino'>Leitura dos Encoders</a>, <a href='./Acionamento_carro/Acionamento_carro.ino'>Acionamento de Carros</a> e Sistema Supervisório, realiza-se o <a href='./Controlador/Controlador.ino'>Controlador de Posição</a>.
#### PID
Foi utilizada a biblioteca <a href='https://github.com/AsafeSilva/PID-Tuner-Controller'>PID Controler</a> para a computação PID.

#### Definição dos Parâmetros de Controle
Com base em testes, percebeu-se melhores resultados com a utilização dos seguintes parâmetros:
  
| Ganhos | Carro A | Carro B | Carro C |
|-------|-----------|-----------|-----------|
| KP | 40.00 | 40.00 | 32.00 |
| KI | 0.00 | 0.00 | 0.00 |
| KD | 0.00 | 0.00 | 0.016 |

## *Portas Utilizadas*


|  | Carro A | Carro B | Carro C |
|-------|--------|--------|------|
| *Encoder - canal 1* | 10 | 17 | 27 |
| *Encoder - canal 2* | 11 | 16 | 29 |
| *Fim de Curso 1* | 12 | 15 | 31 |
| *Fim de Curso 2* | 13 | 14 | 33 |
| *PWM* | 9 | 18 | 25 |
| *Sentido* | 8 | 19 | 23 |
