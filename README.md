# Trabalho Prático02 - Jogo MotoRider em MonoGame com C# - Engenharia e Desenvolvimento de Jogos Digitais - Marco Macedo nº27919 / Miguel Freitas nº29562 

# __Indíce__
1. [__Introdução__](#Introdução)
2. [__Implementação__](#Implementação)
3. [__Instruções de Jogo__](#instru)
4. [__Decisões de Implementação__](#decisoes)
5. [__Análise dos Códigos Disponibilizados__](#analise)
6. [__Conclusão__](#Conclusão)

#  Introdução
Neste trabalho vamos apresentar a implementação de uma jogo 2D Top Down de corrida de motos chamado MotoRider, desenvolvida utilizando o framework MonoGame em C#. O objetivo é fornecer uma visão geral da estrutura do projeto, decisões de implementação e instruções de jogo. Além disso, será feita uma análise dos códigos disponibilizados, abordando a organização e a lógica implementada.

<p align="center">
  <img src="https://i.imgur.com/jfN51Is.png"  alt="MotoRider" width=800>
</p>

## __Estrutura de Pastas:__

* __MotoRider/__
    - |-- Content/
    - |-- Fonts/
    - |-- SpriteSheets/
    - |-- Sounds/

* __Code/__
    - [|-- Button.cs](#button)
    - [|-- Enemy.cs](#enemy)
    - [|-- EnemyGenerator.cs](#EnemyGenerator)
    - [|-- Game1.cs](#game1)
    - [|-- Player.cs](#player)
    - [|-- Program.cs](#program)

<p></p>
  <img src="https://i.imgur.com/UjWmrWv.png"  alt="Estruturadepastas" width=500>

<a name="instru"></a>

# __Instruções de Jogo__

##  __Objetivo:__ 
O objetivo de MotoRider é guiar a moto do jogador pela estrada o máximo de tempo possível, evitando colisões com os carros inimigos que aparecem na pista. O jogo termina imediatamente se a moto colidir com um carro.

## __Controlos:__
* Tecla W : Move a moto para cima.
* Tecla S : Move a moto para baixo.
* Tecla A : Move a moto para a esquerda.
* Tecla D : Move a moto para a direita.

## __Jogabilidade:__

1. Início do Jogo:
   - O jogo começa com a moto posicionada na parte inferior da tela.

2. Movimentação:
   - Use as setas do teclado para desviar dos carros inimigos que aparecem de forma aleatória na pista.

3. Inimigos:
   - Carros inimigos surgem na parte superior da tela e se movem para baixo. A velocidade e a frequência dos carros aumentam conforme o tempo de jogo.

4. Colisões:
   - Se a moto colidir com um carro inimigo, o jogo termina e a pontuação final é apresentada.

5. Pontuação:
   - A pontuação é baseada no tempo que o jogador consegue sobreviver sem colidir com os carros inimigos.

## Menu

# Conclusão
MotoRider é um jogo simples e envolvente que testa os reflexos e habilidades do jogador. O desenvolvimento utilizando MonoGame em C# permitiu a criação de um jogo eficiente e divertido, com controles intuitivos e um desafio crescente. Este relatório detalha a implementação do jogo, destacando as decisões técnicas e de design que contribuíram para a experiência final do usuário.









