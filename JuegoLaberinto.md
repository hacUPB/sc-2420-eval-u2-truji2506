## Inicio

Empezamos realizando las llamadas librerias definiendo las variables y las variables globales.

```c
#include <SDL.h>
#include <stdio.h>
#include <stdbool.h>

#define WINDOW_WIDTH 640
#define WINDOW_HEIGHT 480
#define TILE_SIZE 40
#define FPS 60
#define FRAME_TARGET_TIME (1000/FPS)

// Variables globales
SDL_Window* window = NULL;
SDL_Renderer* renderer = NULL;
bool gameIsRunning = true;
int lastUpdateTime = 0;
```

Establecemos la matriz para la creación del laberinto teniendo en cuenta que tengo un problema al momento de colorear las lineas, ya que se dibujan son rectangulos, ademas establecemos la posición inicial del jugador.

```c
// Mapa del laberinto (1 = pared, 0 = camino)
int maze[12][16] = {
    {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
    {1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1},
    {1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1},
    {1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1},
    {1, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
    {1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1},
    {1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1},
    {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1}
};

// Posición inicial del jugador
int playerX = 1 * TILE_SIZE;
int playerY = 1 * TILE_SIZE;
int playerSpeed = 5;
```
En este paso establecemos que hay adentro del Setup y del Update en el Setup inicializamos el juego y establecemos que tan rapido inicia el juego, en el Update realizamos un codigo para establecer la rapidez del juego a los FPS que necesitamos
```c
void Setup() {
    SDL_Init(SDL_INIT_VIDEO);
    window = SDL_CreateWindow("Juego de Laberinto", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, WINDOW_WIDTH, WINDOW_HEIGHT, 0);
    renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
}

// Función Update
void Update() {
    int time_to_wait = FRAME_TARGET_TIME - (SDL_GetTicks() - lastUpdateTime);
    if (time_to_wait > 0 && time_to_wait <= FRAME_TARGET_TIME) {
        SDL_Delay(time_to_wait);
    }
    lastUpdateTime = SDL_GetTicks();
}
```


En este paso realizamos la funcion de renderizar, ponemos el limpiador de pantalla y dibujar el laberinto con las especificaciones adecuadas, tambien en el redner ponemos que tamaño tiene el jugador y presentar lo renderizado en la pantalla 

```c
// Función Render
void Render() {
    // Limpiar pantalla
    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
    SDL_RenderClear(renderer);

    // Dibujar laberinto
    for (int row = 0; row < 12; row++) {
        for (int col = 0; col < 16; col++) {
            if (maze[row][col] == 1) {
                SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
                SDL_Rect wall = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                SDL_RenderFillRect(renderer, &wall);
            }
        }
    }

    // Dibujar jugador (un cuadrado rojo)
    SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);
    SDL_Rect player = { playerX, playerY, LINE_SIZE, LINE_SIZE };
    SDL_RenderFillRect(renderer, &player);

    // Presentar lo renderizado en la pantalla
    SDL_RenderPresent(renderer);
```
## DIFICULTADES

Se me presenta una dificultad que al momento de realizar las condiciones de movimiento del jugador ya que traspasa los objetos que no lo deberia de hacer

![image](https://github.com/user-attachments/assets/a46231ba-45cd-4029-a83b-3e9be7a20c29)


Se realizaron algunas modificaciones al codigo con la ayuda del profesor para que reconozca las columnas 



![image](https://github.com/user-attachments/assets/c117b1db-aa54-49af-af38-2b35b06100a1)

En casa estuve consultando codigo de los errores que tenia se le definio el tamaño de la linea, el tamaño del personaje, tambien se le añadio un compensador del tamaño de la linea y el juegador se le añadio la velocidad del jugador se le añadio un bool intentando organizar si toca la pared no pasa y si no toca no pasa nada, la colision funciona de la columna * el tamaño de la linea x y la colision con la fila * el tamaño de la linea, tamaño de la linea hacho y tamaño de la linea largo esto en el checkCollision en los eventos se puso un if checkcollision para el jugador y tenga en cuenta las delimitaciones que se habian propuesto antes.
```c
#define WINDOW_WIDTH 640
#define WINDOW_HEIGHT 480
#define LINE_SIZE 40  
#define PLAYER_SIZE 30 
#define PLAYER_OFFSET (LINE_SIZE - PLAYER_SIZE) / 2
#define PLAYER_SPEED 5  
#define FPS 60
#define FRAME_TARGET_TIME (1000 / FPS)

SDL_Window* window = NULL;
SDL_Renderer* renderer = NULL;
bool gameIsRunning = true;
int lastUpdateTime = 0;

int maze[12][16] = {
    {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
    {1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1},
    {1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1},
    {1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1},
    {1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
    {1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1},
    {1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1},
    {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1}
};

int playerX = 1 * LINE_SIZE + PLAYER_OFFSET;
int playerY = 1 * LINE_SIZE + PLAYER_OFFSET;


void Setup() {
    SDL_Init(SDL_INIT_VIDEO);
    window = SDL_CreateWindow("Juego de Laberinto", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, WINDOW_WIDTH, WINDOW_HEIGHT, 0);
    renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
}

void Update() {
    int time_to_wait = FRAME_TARGET_TIME - (SDL_GetTicks() - lastUpdateTime);
    if (time_to_wait > 0 && time_to_wait <= FRAME_TARGET_TIME) {
        SDL_Delay(time_to_wait);
    }
    lastUpdateTime = SDL_GetTicks();
}

void Render() {
    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
    SDL_RenderClear(renderer);

    for (int row = 0; row < 12; row++) {
        for (int col = 0; col < 16; col++) {
            if (maze[row][col] == 1) {
                SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
                SDL_Rect wall = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                SDL_RenderFillRect(renderer, &wall);
            }
        }
    }

    SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);
    SDL_Rect player = { playerX, playerY, PLAYER_SIZE, PLAYER_SIZE };
    SDL_RenderFillRect(renderer, &player);

    SDL_RenderPresent(renderer);
}

bool CheckCollision(int newX, int newY) {
    SDL_Rect playerRect = { newX, newY, PLAYER_SIZE, PLAYER_SIZE };

    for (int row = 0; row < 12; row++) {
        for (int col = 0; col < 16; col++) {
            if (maze[row][col] == 1) {
                SDL_Rect wallRect = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                if (SDL_HasIntersection(&playerRect, &wallRect)) {
                    return true;
                }
            }
        }
    }

    return false;
}

void HandleEvents() {
    SDL_Event event;
    while (SDL_PollEvent(&event)) {
        if (event.type == SDL_QUIT) {
            gameIsRunning = false;
        }
    }

    const Uint8* state = SDL_GetKeyboardState(NULL);

    int newPlayerX = playerX;
    int newPlayerY = playerY;

    if (state[SDL_SCANCODE_UP]) {
        newPlayerY -= PLAYER_SPEED;
    }
    if (state[SDL_SCANCODE_DOWN]) {
        newPlayerY += PLAYER_SPEED;
    }
    if (state[SDL_SCANCODE_LEFT]) {
        newPlayerX -= PLAYER_SPEED;
    }
    if (state[SDL_SCANCODE_RIGHT]) {
        newPlayerX += PLAYER_SPEED;
    }

    if (!CheckCollision(newPlayerX, newPlayerY)) {
        playerX = newPlayerX;
        playerY = newPlayerY;
    }
}

int main(int argc, char* argv[]) {
    Setup();

    while (gameIsRunning) {
        HandleEvents();
        Update();
        Render();
    }

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return 0;
}
```
##    En la revision del profe, tuve algunos comentarios por parte de el ya que no me enfoque al responder varias preguntas,

##    1. ¿Para que el player offset? 
```c
#define PLAYER_OFFSET (LINE_SIZE - PLAYER_SIZE) / 2
```
Esto tiene el proposito de centrar el jugador dentro de las celdas del laberinto es para calcular el tamaño de la celda de la pared y el tamaño del jugador esto nos daria 5 quien posiciona el jugador con 5 pixeles de margen por cada lado que en este caso serian los 4.

##    2. ¿Para que usar el int player = 1 * Line_size + Player_offset?
```c
int playerX = 1 * LINE_SIZE + PLAYER_OFFSET;
int playerY = 1 * LINE_SIZE + PLAYER_OFFSET;
```
Aca lo que hacemos es llamar la definición de Player offset quien es que maneja la posicion del jugador en las coordenadas (1,1) ya que al multiplicar por 1, el jugador tenga esta estas coordenadas fijas y asi hacemos que tenga un posicion mas adecuada por el tema del laberinto ya que en las coordenadas (0,0) estaria parte de la pared.

##    3. ¿Para que sirve el CheckCollision en el codigo?

```c
bool CheckCollision(int newX, int newY) {
    SDL_Rect playerRect = { newX, newY, PLAYER_SIZE, PLAYER_SIZE };

    for (int row = 0; row < 12; row++) {
        for (int col = 0; col < 16; col++) {
            if (maze[row][col] == 1) {
                SDL_Rect wallRect = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                if (SDL_HasIntersection(&playerRect, &wallRect)) {
                    return true;
                }
            }
        }
    }

    return false;
}
```
##    Siguiendo por partes para la explicación

1.
```c
SDL_Rect playerRect = { newX, newY, PLAYER_SIZE, PLAYER_SIZE };
```
Aca creamos un rectangulo que se llama playerRect que seria la nueva posición del jugador, con las dimensiones del Player_Size los parametros newX y newY Son nuevos parametros calculado en la entrada del jugador.

2.
```c
for (int row = 0; row < 12; row++) {
    for (int col = 0; col < 16; col++) {
        if (maze[row][col] == 1) {
```
Este bucle recorre todas las celdas del laberinto. y el if indica que en cada celda hay una pared
3.
```c
SDL_Rect wallRect = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
```
Si una celda es una pared, se crea un rectángulo wallRect para representar esa pared en la pantalla. El tamaño de la pared es de LINE_SIZE x LINE_SIZE.
Las coordenadas de la pared (col * LINE_SIZE, row * LINE_SIZE) indican su posición en la pantalla en función de la fila y columna del laberinto.
4.
```c
if (SDL_HasIntersection(&playerRect, &wallRect)) {
    return true;
}
```
La función SDL_HasIntersection verifica si el rectángulo del jugador (playerRect) se cubre con el rectángulo de una pared wallRect.
Si hay una intersección, significa que el jugador está colisionando con una pared, por lo que se devuelve true y aqui hace logico el bool
5.
```c
return false;
```
Si no detecta ninguna colision puede moverse a la nueva posición sin chocar

## ¿Para que el if(!CheckCollision(newPlayerX, newPlayerY))?
```c
if (!CheckCollision(newPlayerX, newPlayerY)) {
    playerX = newPlayerX;
    playerY = newPlayerY;
}
```
Llama a la funcion CheckCollision, que verifica las coordenadas del jugador en X y en Y con una pared del laberinto, la funcion devuelve verdadero si hay colision y falso cuando no lo hay, ! esta condicion lo que hace es que solo se ejecuta cuando no hay condición en otras palabras asegura que el jugador solo se movera si la nueva posición no colisiona con una pared. playerX = newPlayerX lo que hace es actualizar a las nuevas coordenadas calculadas, permitiendo  que el jugador se mueva a la nueva posición.  

