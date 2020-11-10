<h1 align='center'>Robô Paralelo</h1>
<p align='center'>Projeto de um Sistema Surpervisório e Controlador para um Robô Paralelo</p>

<h2><i>Estrutura Física</i></h2>
<img>
<p>O sistema físico é constituido pela operação em conjunto de três motores CC a fim de controlar o anexo estrutural posicionado no interior da máquina</p>

<h2><i>Processo</i></h2>
<span><b>[x]</b> <i>Leitura de Encoders</i></span><br>
<span><b>[x]</b> <i>Acionamento dos Carros</i></span><br>
<span><b>[x]</b> <i>Implementação do Sistema Supervisório</i></span><br>
<span><b>[x]</b> <i>Definição dos Parâmetros PID</i></span><br>
<span><b>[x]</b> <i>Controle de Posição</i></span><br>
<span><b>[   ]</b> <i>Implementação de modelo matemático</i></span><br>

<h2><i>Descrição de Etapas</i></h2>
<ul>
  <li>
    <h3>Leitura de Encoders</h3>
    <p>Com a leitura dos canais provenientes do hardware, realiza-se a formulação lógica capaz de identificar o deslocamento dos atuadores em relação ao seu eixo.</p>
    <h4>Sequências</h4>
    <span>
      <h5>Positiva</h5> 
      <h5>Negativa</h5>
    </span>
    <p>Com base em tais padrões sequenciais, foi desevolvida a Biblioteca <a href='./BIBLIOTECAS/ENCODER'>ENCODER</a>, com o intuito de automatizar o processo de medição dos sinais referentes aos canais digitais.</p>
  </li>
  <li>
    <a href='./Controlador'>controlador</a>
    <p></p>
  </li>
<ul>

