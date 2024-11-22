# Logisim-MatchingGame

## Descrição geral do projeto
Este projeto consiste na implementação do jogo da memória (matching game) no simulador de circuitos Logisim.  
Neste projeto, o jogo é composto por um grid com 16 displays hexadecimais, dispostos em 4 linhas e 4 colunas, no qual cada par de números foi escolhido de maneira arbitrária.  
O jogo é jogado é por dois jogadores, e o objetivo é fazer mais pontos que o oponente para ganhar a aprtida, no qual cada ponto corresponde a um par de números acertado.  
O jogo é dividido por rodadas alternadas entre os jogadores.

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

Após atingir a coordenada do primeiro número que se deseja selecionar, o jogador pressiona o botão "Confirma" que emite um pulso de clock para o registrador que grava as coordenadas da linha e para o registrador que grava as coordenadas da coluna.
A saída do registrador da linha é conectada ao túnel Linha-n1, e o segundo registrador ao túnel Coluna-N2. De forma análoga, é registrada a coordenada do segundo número.  

![image](https://github.com/user-attachments/assets/fcfcc7c5-db58-4f41-80f3-9166dbc280a7)

## Mostrando os números
O conteúdo dos displays é exibido em dois casos:
* O jogador escolheu determinado display durante a rodada
* O par ao qual pertence o número de determinado display foi acertado
