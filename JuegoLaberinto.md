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




