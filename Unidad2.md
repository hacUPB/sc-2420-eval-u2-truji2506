## Clase 1 

```c
#include <stdio.h>
#include <math.h>

float calcula_IMC(float,float);

int main(int argc, char const *argv[])
{
    float estatura, peso, imc;
    char Nombre[30];

    printf ("Ingrese su nombre: \n");
    fgets(Nombre, 30, stdin);

    printf("Ingrese su peso: \n");
    scanf("%f",&peso);

    printf("Ingrese su estatura: \n");
    scanf("%f",&estatura);

    imc = calcula_IMC(peso, estatura);

    printf("%s su IMC = %f\n",Nombre,imc);

    printf("Sistemas computacionales 2024\n");
    return 0;

}

float calcula_IMC(float peso,float estatura)
{
    float IMC;
    IMC = peso / pow(estatura, 2);
    return IMC;
}
```
//
