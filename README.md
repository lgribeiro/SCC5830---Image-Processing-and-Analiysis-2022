
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
   * [Base de dados](#base-de-dados)
   * [Metodologia e experimentos](#metodologia-e-experimentos)
      * [Pré-processamento com morfologia](#pré-processamento-com-morfologia)
      * [Detecção de marca ópticas](#detecção-de-marca-ópticas)
      * [Leitura e validação do código da prova](#leitura-e-validação-do-código-da-prova)
   * [Resultados](#resultados)
   * [Conclusão](#conclusão)
<!--te-->

Introdução
============

O objetivo deste trabalho é implementar uma solução codificada na linguagem python, usando técnicas de processamento de imagem e visão computacional para a leitura e interpretação de provas digitalizadas segundo o formato usado pelo software Auto Multiple Choice (AMC)¹. Este software auxilia o usuário a criar e gerenciar questionários de múltipla escolha (MCQ) com marcação automatizada. AMC é um software livre, distribuído sob a licença GPLv2+.


Com software AMC é possivel corrigir automaticamente as folhas de respostas concluídas e digitalizadas de uma prova com a técnica de reconhecimento de marca óptica na folha em questão. A marca óptica (marca circular preta) é colocada nos cantos extremos como pontos de referência na folha. Uma vez detectadas as marcações dos cantos, caso necessário a rotação e perspectiva são corrigidas para a leitura das opcões assinaldas (quadradinhos pintados), permitindo assim a validação do código da prova, identificação do aluno e a correção das questões. A Figura 1 é um exemplo da página do cartão de resposta de uma prova.

<img src="https://user-images.githubusercontent.com/31041239/174407993-505e0dc5-e7ef-41e0-a96b-5e3a745f8288.PNG" width="450" height="650">
<p align = "rigth">
<b>Figura 1 - </b>Exemplo de prova
</p>


Base de dados
============

O modelo padrão da prova para esse estudo foi criada com o software AMC. Existem três tipos de provas com o mesmo conteudo, porém com as questões "embaralhada, uma forma de inibir a famosa cola.

A Figura 2 apresenta os componentes do código de uma prova em vermelho, em verde a identificação do aluno e por último em azul o cartão de resposta das questões. Na parte superior da prova (Figura 2) é possível ver o texto <b>+1/2/59+</b>, que corresponde a uma representação textual do código da prova no AMC. O código de uma prova é composto por três dados:

• P = número da prova, p=1;    

• pag = número da página, pag=2;    

• nv = número de verificação, nv=59.

<img src="https://user-images.githubusercontent.com/31041239/174413722-102485b0-63b2-4cac-a1f1-f6e607760263.PNG">
<p align = "rigth">
<b>Figura 2 - </b>Códigos de leitura
</p>

Esses mesmos números são codificados em base binária, tal como indicado na Figura 3, para facilitar sua leitura automática pelo AMC. O número de verificação (nv) possui 6 bits e é usado para possibilitar a detecção de erros na leitura do documento digitalizado por apresentar redundância de dados. Ele pode ser obtido a partir do número da prova (P) e do número da página (pag) pela seguinte fórmula:

<b>nv=60-[(P-1)4+(pag-1)]%60</b>, onde % denota o operador de resto da divisão.

<img src="https://user-images.githubusercontent.com/31041239/174415036-74675022-bb07-448a-bc7f-2629cd22601b.png">
<p align = "rigth">
<b>Figura 3 - </b>Representação do código da prova
</p>

A partir do modelo padrão da prova foi criado a base de dados do trabalho para atender dois cenários. O primeiro cenário digitalizou as provas em uma máquina de escaner em um ambiente totalemnte controlado. E o segundo cenário as provas foram digitalizadas pelo celular sem qualquer preocupação com luminosidade, rotação e perspectiva, ocasionando muitos ruídos em cada prova digitalizada. O maior desafio desse trabalho é a remoção desses ruídos para uma melhor detecção e leitura dos códigos da prova. A base de dados se encontra na pasta ~/provas  

Metodologia e experimentos
============
Para alcançar o objetivo desse trabalho, será utilizado os métodos de morfologia matemática, threshold otsu e detecção de marca óptica. Cabe salientar que tais métodos ainda etão à ser abordados no decorrer da disciplina. 


<h2 align="center"> 
    :construction:  Projeto em construção  :construction: :hammer:
</h2>

Pré-processamento com morfologia
-----


Detecção de marca ópticas
-----


Leitura e validação do código da prova
-----


Resultados
============


Conclusão
============

¹ https://www.auto-multiple-choice.net/index.en
