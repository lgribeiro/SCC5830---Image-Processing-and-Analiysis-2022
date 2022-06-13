
<h2 align="center">SCC5830 - Image Processing and Analysis (2022) - ICMC<br><br>Relátorio Parcial</br></br></h2>

<h1 align="center"><br><br><b>Uma solução para a correção de provas digitalizadas de múltipla escolha.</b></br></br></h1>
<h2 align="center">
  <br>Professor: 
    <br>Moacir Antonelli Ponti.</br></br>
  <br">Alunos:
      <br>Luiz Ribeiro 5967710</br>
      <br>Matheus Diniz</br>
      <br>Connnor</br>

   </br>
</h2>

Tabela de conteúdos
=================
<!--ts-->
   * [Introdução](#introdução)
   * [Metodologia e experimentos](#metodologia-e-experimentos)
      * [Pré-processamento com morfologia](#pré-processamento-com-morfologia)
      * [Detecção de marca ópticas](#detecção-de-marca-ópticas)
      * [Rotação e perspectiva](#rotação-e-perspectiva)
      * [Leitura e validação do código da prova](#leitura-e-validação-do-código-da-prova)
      * [Cenários](#cenários)
   * [Resultados](#resultados)
   * [Conclusão](#conclusão)
<!--te-->

Introdução
============

O objetivo deste trabalho é implementar uma solução codificada na linguagem python, usando técnicas de processamento de imagem e o mínimo de visão computacional para a leitura e interpretação de provas digitalizadas segundo o formato usado pelo software Auto Multiple Choice (AMC)¹. Este software auxilia o usuário a criar e gerenciar questionários de múltipla escolha (MCQ) com marcação automatizada. AMC é um software livre, distribuído sob a licença GPLv2+.


Com software AMC é possivel corrigir automaticamente as folhas de respostas concluídas e digitalizadas de uma prova com a técnica de reconhecimento de marca óptica na folha em questão. A marca óptica (marca circular preta) é colocada nos cantos extremos como pontos de referência na folha . Uma vez detectadas as marcações dos cantos, a prova pode então ser exibida com sua rotação corrigida, permitindo a leitura e validação do código da prova, identificação do aluno e correção das questões como por exemplo. A Figura 1 representa a leitura e validação do código da prova.
O código de uma prova é composto por três dados:

• P = número da prova,

• pag = número da página,

• nv = número de verificação.

A Figura 2 apresenta os componentes do código de uma prova em detalhes. Na parte
superior da prova (Figura 2) é possível ver o texto +633/1/52+, que corresponde a uma
representação textual do código da prova no AMC. Esses mesmos números são codificados
em base binária na região indicada em vermelho, tal como indicado na Figura 3, para
facilitar sua leitura automática pelo AMC. O número de verifica¸cão (nv) possui 6 bits e
´e usado para possibilitar a detec¸cão de erros na leitura do documento digitalizado por
apresentar redundˆancia de dados. Ele pode ser obtido a partir do número da prova (P) e
do número da página (pag) pela seguinte fórmula:

nv=60-[(P-1)4+(pag-1)]%60, onde % denota o operador de resto da divisão.

Metodologia e experimentos
============
Para alcançar o objetivo desse trabalho, será utilizada os métodos de morfologia matemática, threshold otsu e detecção de marca óptica. Cabe salientar que tais métodos ainda etão à ser abordados no decorrer da disciplina. 

Pré-processamento com morfologia
-----


Detecção de marca ópticas
-----


Rotação e perspectiva
-----

Leitura e validação do código da prova
-----

Cenários
-----


Resultados
============


Conclusão
============

¹ https://www.auto-multiple-choice.net/index.en
