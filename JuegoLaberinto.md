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
