# Logisim-MatchingGame

## Descrição geral do projeto
Este projeto consiste na implementação do jogo da memória (matching game) no simulador de circuitos Logisim.  
Neste projeto, o jogo é composto por um grid com 16 displays hexadecimais, dispostos em 4 linhas e 4 colunas, no qual cada par de números foi escolhido de maneira arbitrária.  
O jogo é jogado é por dois jogadores, e o objetivo é fazer mais pontos que o oponente para ganhar a aprtida, no qual cada ponto corresponde a um par de números acertado.  
O jogo é dividido por rodadas alternadas entre os jogadores.  

![image](https://github.com/user-attachments/assets/9ef04b21-f6a8-4f22-9a7f-666998a5601d)


## Máquina de estados
Cada rodada é dividida em diferentes estados, que servem para identificar o estado atual do jogo. O projeto apresenta 4 estados que são identificados por meio de um contador, no qual cada nuúmero do contador representa um estado diferente.

* Estado 0: Ambos os displays ocultos
* Estado 1: Primeira escolha revelada
* Estado 2: Primeira e segunda escolhas reveladas
* Estado 3: Fase de comparação e retorno ao Estado 0  

![image](https://github.com/user-attachments/assets/abaa8cb2-cc42-471d-aaa2-25c955e0b82e)

## Escolhendo os números
A seleção dos números é feita por meio de coordenadas, as quais são controladas por dois contadores de dois bits, um para as coordenadas da linha e outro para as coordenadas da coluna, ambos indo de 0 a 3.  
Os contadores têm seu valor incrementado por meio de dois botões, um responsável por navegar entre as linhas, e o outro, pelas colunas, ou seja, a cada pulso de do botão o contador incrementa um ao seu valor atual. Ao chegar no valor máximo, o
contador volta pro início. Além disso, há, na saída desses contadores, os túneis !Linha e !Coluna que se ligam em dois registradores, os quais recebem o valor atual de cada contador. 

![image](https://github.com/user-attachments/assets/8f860c5a-bd1c-4163-a2d6-7e987344a1cb)  

Para ter um referência visual de qual display a coordenada atual representa, há um circuito que verifica os valores dos contadores e acende leds da acima do displays da posição correspondente. Dessa forma, se o contador da linha apresentar o valor 00 e o
contador da coluna o valor 01, o led L01, que corresponde à essa coordenada, será ativado, e assim por diante.  

![image](https://github.com/user-attachments/assets/7822b0d9-5122-417a-be08-276ed970d20f)  

Após atingir a coordenada do primeiro número que se deseja selecionar, o jogador pressiona o botão "Confirma" que emite um pulso de clock para o registrador que grava as coordenadas da linha e para o registrador que grava as coordenadas da coluna, apenas
quando a rodada está no estado 0.
A saída do registrador da linha é conectada ao túnel Linha-n1, e o segundo registrador ao túnel Coluna-N2. De forma análoga, é registrada a coordenada do segundo número, apenas quando a rodada está no estado 1.  

![image](https://github.com/user-attachments/assets/fcfcc7c5-db58-4f41-80f3-9166dbc280a7)

## Exibindo os números
O conteúdo dos displays é exibido em dois casos:
* O jogador escolheu determinado display durante a rodada
* O par ao qual pertence o número de determinado display foi acertado

O circuito que exibe o valor do display selecionado segue a mesma lógica do circuito dos leds. O bits das coordenadas são ligados a portas ANDs, que têm suas portas negadas de acordo com o valor que cada coordenada representa.
Há um par de portas AND para cada resultado possível, uma para o primeiro número(Linha-N1 e Coluna-N1), e outra para o segundo número(Linha-N2 e Coluna-N2).
Por fim, existe uma porta OR com três entradas, que recebe como entradas as saídas das portas ANDs e o túnel AcertadoXX, no qual "XX" representa a coordenada do número acertado.

![image](https://github.com/user-attachments/assets/5a7eb827-288e-448e-a8ea-741b95c6c67b)  

A saída da porta OR passa é recebids pelo túnel MuxXX, o qual é conectado ao multiplexador cuja saída exibe o conteúdo do seu display correspondente. Se o valor do túnel for 0, o multiplexador não apresenta nenhuma saída, e, portanto, o display permanece
oculto. Se o valor do túnel for 1, a saída do multiplexador será o número definido para o display correspondente.  

![image](https://github.com/user-attachments/assets/4c3fe76b-c6b4-4e86-b6c9-0bef4fadea11)

## Alternando entre os jogadores
A alternação entre os jogadores é feita por meio de um contador de um bit. O valor do contador é incrementado cada vez que se atinge o estado 0. A saída do contador é conectada aos túneis jogador 1 e jogador 2, com o valor do túnel jogador 1 negado.
Dessa forma, quando o contador está no valor 0 o túnel jogador 1 terá o valor 1 e o túnel jogador 2 terá o valor 0 e vice-versa.  

![image](https://github.com/user-attachments/assets/9965cbb6-c7be-46e3-adff-930229433265)

## Fazendo pontos
Após os dois números serem selecionados, dois multiplexadores recebem as coordenadas dos túneis Linha-N1 e Coluna-N1 e dos túneis Linha-N2 e Coluna-N2 e seleciona a saida de acordo com as coordenadas. Em seguida, os números selecionados entram em um
comparador que retorna 1 se els forem iguais e 0 caso contrário. A saída do comparador é conectada a dois contadores que representam a pontuação de cada jogador. Caso a saída do contador seja 1, o contador que representa a pontuação do jogador atual é
incrementado em uma unidade.  

![image](https://github.com/user-attachments/assets/0bee3132-99ad-4061-afa2-6c76f1b0359f)

Além disso, se a saída do comparador for 1, o túnel Verifica será ativado, que servirá para indicar que os túneis AcertadoXX das posições dos números escolhidos devem ser ativados, o que indicará que o par foi acertado e não poderá mais ser escolhido.  

![image](https://github.com/user-attachments/assets/b0345e9b-09ee-4ed6-91b1-01413432f6ca)

