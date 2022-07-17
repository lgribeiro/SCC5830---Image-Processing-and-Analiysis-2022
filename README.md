
<h2 align="center">SCC5830 - Image Processing and Analysis (2022) - ICMC<br><br>Relátorio Parcial</br></br></h2>

<h1 align="center"><br><br><b>Uma solução para a correção de provas digitalizadas de múltipla escolha.</b></br></br></h1>
<h2 align="center">
  <br>Professor: 
  
    Moacir Antonelli Ponti
  
  <br>Alunos:
  
      Connor Sterrett 12119731
      
      Luiz Ribeiro 5967710
      
      Matheus Diniz 11933342
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
         * [Rotação e perspectiva](#rotação-e-perspectiva)
      * [Leitura e validação do código da prova](#leitura-e-validação-do-código-da-prova)
   * [Resultados](#resultados)
   * [Conclusão](#conclusão)
<!--te-->

Introdução
============

O objetivo deste trabalho é implementar uma solução codificada na linguagem Python, usando técnicas de processamento de imagem e visão computacional para a leitura e interpretação de provas digitalizadas segundo o formato usado pelo software Auto Multiple Choice (AMC)¹. Este software auxilia o usuário a criar e gerenciar questionários de múltipla escolha (MCQ) com marcação automatizada. AMC é um software livre, distribuído sob a licença GPLv2+.


Com software AMC é possivel corrigir automaticamente as folhas de respostas concluídas e digitalizadas de uma prova com a técnica de reconhecimento de marca óptica na folha em questão. A marca óptica (marca circular preta) é colocada nos cantos extremos como pontos de referência na folha. Uma vez detectadas as marcações dos cantos, caso necessário a rotação e perspectiva são corrigidas para a leitura das opcões assinaldas (quadradinhos pintados), permitindo assim a validação do código da prova, identificação do aluno e a correção das questões. A Figura 1 é um exemplo da página do cartão de resposta de uma prova.

<img src="https://user-images.githubusercontent.com/31041239/174407993-505e0dc5-e7ef-41e0-a96b-5e3a745f8288.PNG" width="450" height="650">
<p align = "rigth">
<b>Figura 1 - </b>Exemplo de prova
</p>


Base de dados
============

O modelo padrão da prova para esse estudo foi criado com o software AMC. Existem três tipos de provas com o mesmo conteúdo, porém com as questões "embaralhadas". O código em LaTeX e o exemplo em .pdf da prova padrão podem ser encontrados na pasta /templates.

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

A partir do modelo padrão da prova foi criado a base de dados do trabalho para atender dois cenários. O primeiro cenário digitalizou as provas em uma máquina de escaner em um ambiente totalmente controlado. E o segundo cenário as provas foram digitalizadas pelo celular sem qualquer preocupação com luminosidade, rotação e perspectiva, ocasionando muitos ruídos em cada prova digitalizada. O maior desafio desse trabalho é a remoção desses ruídos para uma melhor detecção e leitura dos códigos da prova. A base de dados se encontra na pasta ./exames.  


Metodologia e experimentos
============
Para alcançar o objetivo desse trabalho, será utilizado os métodos de morfologia matemática, threshold otsu e detecção de marca óptica.


Pré-processamento com morfologia
-----
Foram implementados neste trabalho os métodos de morfologia matemática dilatação e erosão, bem como tranformação tom de cinza e binarização usando o método Otsu.  Para decidir qual método de mofologia será usado, uma comparação foi feita olhando a mudança das marcas ópticas e o preenchimento do exame.  Foi decidido usar o metodo de opening (erosion > dilation) + dilation + dilation.
<img src="https://user-images.githubusercontent.com/12867263/179374646-47dc39a4-530a-4c43-8152-f3315049f6ba.png">
<p align = "rigth">
<b>Figura 4 - </b>Comparação das métodos de morfologia
</p>


Detecção de marca ópticas
-----
A imagem original foi redimensionada para otimizar o tempo processamento de cada etapa, e em seguida são aplicados alguns filtros e operações morfológicas para remover ruído e realçar a marcação circular que será detectada pelo algoritmo.  

Antes de todas operações a imagem foi transformada em tons de cinza e em seguida a binarização da imagem. A operação morfológica de dilatação foi aplicado para aumentar bordas e preencher buracos porém antes foi aplicado a operação morfológica opening (é o processo de erosão seguida de dilatação) para a remoção de ruídos da imagem.

Após aplicação dos filtros na figura 4 verifica se poucos ruidos e blobs bem realçados. Blob significa Objeto Binário Grande, onde o termo “Grande” se concentra no objeto de um tamanho específico, e outros objetos binários “pequenos” são geralmente considerados como ruído.

Foram usados os algoritmos SimpleBlobDetector e Hough Circles com a imagem pré processada, ou seja, com aplicação dos filtros e operações morfologicas. Para as etapas seguintes foi retomada a imagem original aplicando somente o método de Otsu. 

A imagem foi recortada em 4 quandrantes para diminuir a area da imagem e excluir possivies marcaçoes indesejáveis na região central da imagem. Para cada quadrante executa a função que detectar Blobs. 

Para o método de SimpleBlobDetector foi configurado com os parametros de circularidade e convexidade para aproximar o blob detectado de um círculo, evitando outros os blobs indesejáveis.

Para o método Hough Circles que usa a Transformada Hough para achar circulos tambem foi ajustados seus parâmetros.

Essas funções retornam quase sempre mais de um blob, então o programa selciona o potencial blob usando a condição de que o blob deve ter o tamanho entre 15 e 18 radius e deve ser o mais longínquo (distância euclidiana) do centro da imagem.

<img src="https://user-images.githubusercontent.com/31041239/174932804-4986f821-8fa5-4f68-97a7-c6e3a35e3063.jpeg">
<p align = "rigth">
<b>Figura 5 - </b>Detecção da marca óptica grifada em vermelho
</p>


Rotação e perspectiva
-----
Antes de aplicar a detecção da seção anterior, o programa faz uma verificação se a altura da imagem é maior que a largura, caso nao seja ele aplica uma rotação de 90 graus. Caso essa função é chamada pela segunda vez, a rotação será de 180 graus.

A correção de perspectiva da imagem é toda amarrada na detecção dos blobs desejados, ou seja, somente irá funcionar se obtver quatro pontos da detecção. Para assim achar a matriz de rotação e translação e aplicar com o auxílio da função warpPerspective do opencv.

Lembrando que detecção de blobs é feita com a imagem preprocessada com todos os filtros explicados na seção anterior, porem a correção de perspectiva é feita ma imagem original com a plicação do metodo de binarização de Otsu. A Figura 5  mostra uma imagem rotacionada e com outra perspctiva.


Leitura e validação do código da prova
-----

<img src="https://user-images.githubusercontent.com/31041239/174933239-ef2bed64-4600-4a70-810b-d8ee32268594.jpeg">
<p align = "rigth">
<b>Figura 6 - </b>Leitura do código da prova
</p>


Com rotação corrigida todas as coordenadas das marcações para leitura são calculas a partir da distância em pixels entre as duas marcações circulares (W) no topo da prova, aquelas retornadas pelo metodo de detecção de blobs.

Esse retângulo maior foi subdivido em 12 partes e diminuindo um pouco a sua area para que ele seja contido pela sua caixa mais afora, todo cálculo  é feito sobre W. O mesmo foi feito para a caixa de leitura do número USP. Essas áreas (caixas subdividas) serão usada para verificar ausência ou presença de pixel preto. Se o número de pixels pretos forem maior que o número de pixels brancos
então essa posição recebe o valor 1. Importante pensar esses retângulos como matrizes, assim  é identificável pela posição (posição da matriz para o numero Usp) e na conversão de binário pra decimal para a leitura e validação do código da prova. A Figura 6 apesar de binarizada é possível perceber como os subretângulos são desenhados.

Resultados
============


<h2 align="center"> 
  Nosso algoritmo foi capaz de realizar a identificação dos marcadores óticos e consequentemente a leitura dos códigos da prova e matricula do aluno em provas standard, porém devido a presença de muitos ruidos  de luminancia nas provas fotografadas por celulares, não foi possivel realizar a identificação dos marcadores óticos e consequentemente a leitura dos dados.
</h2>

Conclusão
============


Este trabalho apresentou todos os processos para criar uma solução de visão computacional e processamento de imagem para a leitura e interpretação de provas digitalizadas segundo o formato usado pelo AMC usando conceitos importante de processamento de imagem adquiridos na disciplina.
Fato que, para a criação do programa não existe um único processo ou maneira, existem muitas possibilidades e foi apresentado uma delas. Novas features podem ser adicionadas ao programa com a finalidade de facilitar a vida do professor, como por exemplo conexão com o banco de dados para guardar as notas, contabilizá-las e gerar relatório da disciplina ou de modo mais granular por aluno.

¹ https://www.auto-multiple-choice.net/index.en
