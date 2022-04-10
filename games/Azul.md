# Azul

- Jogo de tile drafting
- 2-4 Jogadores (BGG: best 2)
- 30-45 min

## Intro

Os jogadores competem para decorar as paredes do Palácio Real de Évora com os padrões de azulejo mais bonitos.


## Setup

- Cada jogador tem o seu tabuleiro (todos no lado colorido)
- Cada jogador põe o seu marcador de pontuação (cubo preto) no 0
- Colocam-se N+1 expositores no centro da mesa (N = número de jogadores)
- Colocam-se todos os azulejos (100) no saco
- Preenche-se cada expositor com 4 peças aleatórias do saco

## Turn

### Drafting

O jogador inicial coloca o marcador de primeiro jogador no centro da mesa e realiza o seu turno. Depois segue no sentido dos ponteiros do relógio.

Cada jogador faz 1 de duas ações:
- Retirar todos os azulejos da mesma cor de um expositor, movendo os outros desse expositor para o centro da mesa; OU
- Retirar todos os azulejos da mesma cor do centro da mesa
  - Se o marcador de primeiro jogador ainda estiver no centro, retirá-lo e colocá-lo na zona das sobras/penalidades (-1, etc), da esquerda para a direita. (Se não couberem, voltam para a caixa do jogo.)

De seguida, adiciona os azulejos que tirou a apenas uma linha da parte esquerda do tabuleiro.
- (São adicionados da direita para a esquerda nessa linha)
- Se a linha estiver vazia, verificar que a parede (à direita) ainda não tem aquela cor (na mesma linha).
- Se a linha à esquerda não estiver vazia, só se pode continuar a mesma cor.
- Azulejos que não couberem na linha descem para as sobras.


### Revestimento da parede e pontuação

Cada jogador reveste a sua parede (à direita):
- Cada linha completa à esquerda gera um azulejo novo daquela cor para adicionar à parede na direita.
  - Os outros azulejos da linha à esquerda voltam para a caixa do jogo.
- Pontuam-se os azulejos adicionados:
  - Cada azulejo isolado dá 1 ponto
  - Cada azulejo com adjacentes:
    - Dá X pontos pelos X adjacentes na linha (incluindo ele próprio)
    - Dá Y pontos pelos Y adjacentes na coluna (incluindo ele próprio)

Cada jogador perde os pontos indicados em cada espaço ocupado da linha de sobras (não é só o último espaço ocupado, são todos).
- O marcador de primeiro jogador volta para o centro.
- As outras sobras voltam para a caixa do jogo.

Preenche-se novamente os expositores com os azulejos do saco para a próxima ronda.
- Se o saco ficar vazio, os azulejos da caixa do jogo voltam para o saco.

## End

- O jogo acaba quando um jogador tiver uma linha horizontal completa.
- Adicionam-se os pontos bónus a cada jogador:
  - +2 pontos por cada linha (horizontal) completa na parede
  - +7 pontos por cada coluna (vertical) completa na parede
  - +10 pontos por cada cor que apareça em todas as linhas da parede
- Desempate: mais linhas completas, senão partilha-se a vitória.

## Modo alternativo

- Usa-se o lado sem cor do tabuleiro de jogador (para todos)
- Cada azulejo só pode aparecer uma vez em cada linha e coluna (estilo sudoku)
  - Se, no momento de revestir, a posição deixar de ser válida por causa dos azulejos que entretanto acabaram, descartar a linha do padrão (esquerda) para as sobras.

