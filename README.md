# SCC5830---Image-Processing-and-Analiysis-2022



<h2 align="center">SCC5830 - Image Processing and Analysis (2022)</h1>
<h2 align="center">Instituto de Ciêcias Matemáticas e Computação - Universidade de São Paulo.</h1>
<h1 align="center"><b>Uma solução para a correção de provas digitalizadas de múltipla escolha.</b></h1>
<h2 align="center">Professor: Moacir Antonelli Ponti.</h1>
<h2 align="center">Alunos:</h1>


Tabela de conteúdos
=================
<!--ts-->
   * [Introdução](#Introdução)
   * [ Metodologia e experimentos](#metodologia-e-experimentos)
      * [Pre processamento com morfolgia](#pre-processamento)
      * [Detecção marca ópticas](#local-files)
      * [Rotação e perspectiva](#remote-files)
      * [Leitura e validação do código da prova](#multiple-files)
      * [Cenários](#Cenários)
   * [Resultado](#resultado)
   * [ Conclusão](#conclusão)
<!--te-->

#Introdução
O objetivo deste trabalho é implementar uma solução codificada na linguagem python, usando técnicas de processamento de imagem e o mínimo de visão computacional para a leitura e interpretação de provas digitalizadas segundo o formato usado pelo software Auto Multiple Choice (AMC)¹. Este software auxilia o usuário a criar e gerenciar questionários de múltipla escolha (MCQ) com marcação automatizada. AMC é um software livre, distribuído sob a licença GPLv2+.


Com as folhas de respostas concluídas e digitalizadas usa se o MAC para corrigir las automaticamente pelo AMC,e este por sua vez usa o reconhecimento de marca óptica.
A marca óptica (marca circular preta) é usada nos cantos extremos da folha para permitir criar pontos de referência. Uma vez detectadas as marcações dos cantos, a prova pode então ser exibida com sua rotação corrigida e permitindo a leitura do código da prova e de outras informações que o usuário queira automatixzar a leitura , como identificação do aluno e correção das questões da prova, tal como indicado na Figura 1.
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
nv=60-[(P-1)4+(pag-1)]%60
onde % denota o operador de resto da divisão.

¹ https://www.auto-multiple-choice.net/index.en
