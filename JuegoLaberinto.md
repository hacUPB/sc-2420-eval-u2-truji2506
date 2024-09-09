## Inicio

1.) Empezamos realizando las llamadas librerias definiendo las variables y las variables globales

´´´c
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
´´´
