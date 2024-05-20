# Trabalho Prático02 - Jogo MotoRider em MonoGame com C# - Engenharia e Desenvolvimento de Jogos Digitais - Marco Macedo nº27919 / Miguel Freitas nº29562 

# __Indíce__
1. [__Introdução__](#Introdução)
2. [__Implementação__](#Implementação)
3. [__Instruções de Jogo__](#instru)
4. [__Decisões de Implementação__](#decisoes)
5. [__Análise dos Códigos Disponibilizados__](#analise)
6. [__Conclusão__](#Conclusão)

# __Introdução__
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

## __Menu__

# __Estrutura do Projeto__

O projeto MotoRider foi estruturado de forma modular, facilitando a manutenção e expansão. As principais classes do projeto são:

* [|-- Button.cs](#button)
* [|-- Enemy.cs](#enemy)
* [|-- EnemyGenerator.cs](#EnemyGenerator)
* [|-- Game1.cs](#game1)
* [|-- Player.cs](#player)
* [|-- Program.cs](#program)

## __Descrição das Classes:__

<a name="#button"></a>
### __Button.cs__
A classe Button gerencia os botões da interface do usuário (UI) no jogo. Este componente é essencial para interações básicas, como reiniciar o jogo ou sair.

```
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using System;
using System.Collections.Generic;
using System.Text;

namespace TDV
{
    class Button
    {
        Texture2D texture;
        Vector2 position;
        Rectangle rectangle;

        Color color = new Color(255, 255, 255, 255);

        public Vector2 size;

        public Button(Texture2D newTexture, GraphicsDevice graphics) 
        {
            texture = newTexture;
            size = new Vector2(graphics.Viewport.Width/4, graphics.Viewport.Height/9);
        }

        bool down;
        public bool isClicked;
        
        public void Update(MouseState mouse) 
        {
            rectangle = new Rectangle((int)position.X, (int)position.Y,(int)size.X, (int)size.Y);
            Rectangle mouseRectangle = new Rectangle(mouse.X, mouse.Y, 1, 1);

            if (mouseRectangle.Intersects(rectangle))
            {
                if (color.A == 255) down = false;
                if (color.A == 0) down = true;
                if (down) color.A += 3; else color.A -= 3;
                if (mouse.LeftButton == ButtonState.Pressed) isClicked = true;
                
            }
            else if (color.A < 255)
            {
                color.A += 3;
                isClicked = false;
            }
        }

        public void setPosition(Vector2 newPosition) 
        {
            position = newPosition;
        }

        public void Draw(SpriteBatch spriteBatch) 
        {
            spriteBatch.Draw(texture, rectangle, color);
        }
    }
}
```
<a name="#enemy"></a>
### __Enemy.cs__
A classe Enemy representa os carros inimigos na pista. Cada instância possui métodos para movimentação e detecção de colisão com o jogador.

```
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using System;
using System.Collections.Generic;
using System.Text;

namespace TDV
{
    public class Enemy
    {
        public Texture2D texture;
        public Vector2 position;
        public Vector2 velocity;
        public Vector2 Position
        {
            get { return position; }
        }

        public Enemy(Texture2D newTexture, Vector2 newPosition, Vector2 newVelocity)
        {
            texture = newTexture;
            position = newPosition;
            velocity = newVelocity;
        }

        public void Update()
        {
            position += velocity;

        }

        public void Draw(SpriteBatch spriteBatch)
        {
            // Ajuste a posição X para mover a imagem para a direita
            float offsetX = 30; // Ajuste esse valor conforme necessário
            spriteBatch.Draw(texture, new Vector2(position.X + offsetX, position.Y), null, Color.White, (float)3.1415926536, new Vector2(0, 0), 0.5f, SpriteEffects.None, 0);
        }




        //public void Draw(SpriteBatch spriteBatch)
        //{
        //   spriteBatch.Draw(texture, position, null, Color.White, (float)3.1415926536, new Vector2(0, 0), 0.5f, SpriteEffects.None, 0);
        //}
        //public Rectangle Rectangle
        //{
        //    get
        //    {
        //        return new Rectangle((int)position.X, (int)position.Y, texture.Width, texture.Height);
        //    }
        //}
        #region colisions
        //public bool IsTouchingLeft(Enemy enemy)
        //{
        //    return this.Rectangle.Right + this.velocity.X > enemy.Rectangle.Left &&
        //        this.Rectangle.Left > enemy.Rectangle.Left &&
        //        this.Rectangle.Bottom > enemy.Rectangle.Top &&
        //        this.Rectangle.Top < enemy.Rectangle.Bottom;
        //}

        //public bool IsTouchingRight(Enemy enemy)
        //{
        //    return this.Rectangle.Left + this.velocity.X < enemy.Rectangle.Right &&
        //        this.Rectangle.Right > enemy.Rectangle.Right &&
        //        this.Rectangle.Bottom > enemy.Rectangle.Top &&
        //        this.Rectangle.Top < enemy.Rectangle.Bottom;
        //}

        //public bool IsTouchingTop(Enemy enemy)
        //{
        //    return this.Rectangle.Bottom + this.velocity.Y > enemy.Rectangle.Top &&
        //        this.Rectangle.Top < enemy.Rectangle.Top &&
        //        this.Rectangle.Right > enemy.Rectangle.Left &&
        //        this.Rectangle.Left < enemy.Rectangle.Right;
        //}

        //public bool IsTouchingBottom(Enemy enemy)
        //{
        //    return this.Rectangle.Top + this.velocity.Y < enemy.Rectangle.Bottom &&
        //        this.Rectangle.Bottom > enemy.Rectangle.Bottom &&
        //        this.Rectangle.Right > enemy.Rectangle.Left &&
        //        this.Rectangle.Left < enemy.Rectangle.Right;
        //}

        #endregion

    }
}
```
<a name="#EnemyGenerator"></a>
### __EnemyGenerator.cs__
A classe EnemyGenerator gerencia a criação e posicionamento dos carros inimigos na pista. Utiliza um sistema de tempo para gerar inimigos em intervalos regulares.

```
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using System;
using System.Collections.Generic;
using System.Text;

namespace TDV
{
    class EnemyGenerator
    {
        public Texture2D texture;
        float density;
        Rectangle spawnzone;
        int e = 0;

        public List<Enemy> enemies = new List<Enemy>();

        float timer;

        Random rand1, rand2;

        public EnemyGenerator(Texture2D newtexture, Rectangle spawn, float newDensity)
        {
            texture = newtexture;
            density = newDensity;
            spawnzone = spawn;

            rand1 = new Random();
            rand2 = new Random();
        }

        public void createEnemy()
        {
            enemies.Add(new Enemy(texture, new Vector2((40 + spawnzone.X) + (float)rand1.NextDouble() * spawnzone.Width, spawnzone.Height + spawnzone.Y), /*new Vector2(0, rand2.Next(4, 4))*/ new Vector2(0,6)));
        }

        public void Update(GameTime gameTime, GraphicsDevice graphics)
        {
            timer += (float)gameTime.ElapsedGameTime.TotalSeconds;

            while (timer > 0)
            {
                timer -= 0.3f / density;
                createEnemy();
                e++;
            }

            for (int i = 0; i < enemies.Count; i++)
            {
                enemies[i].Update();

                if (enemies[i].Position.Y > graphics.Viewport.Height + texture.Height)
                {
                    enemies.RemoveAt(i);
                    i--;
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            foreach (Enemy enemy in enemies)
            {
                enemy.Draw(spriteBatch);
            }
        }

    }
}
```
<a name="#game1"></a>
### __Game1.cs__

# __Conclusão__
MotoRider é um jogo simples e envolvente que testa os reflexos e habilidades do jogador. O desenvolvimento utilizando MonoGame em C# permitiu a criação de um jogo eficiente e divertido, com controles intuitivos e um desafio crescente. Este relatório detalha a implementação do jogo, destacando as decisões técnicas e de design que contribuíram para a experiência final do usuário.

