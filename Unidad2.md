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
## Clase 2
Iniciación SDL

´´´
#include "minimal.h"
#include <stdio.h>
#include <math.h>

float Calcula_menordeellos(float,float);

int main(int argc, char* argv[]){
    //pedir al usuario que ingrese dos numeros y calcule el menor de ellos
    //usar la funcion pow() para elevar ell primer número al segundo
    int numero_1, numero_2, potencia;

    printf("Ingrese un numero A: \n");
    scanf("%i", &numero_1);

    printf("Ingrese un numero B: \n");
    scanf("%i", &numero_2);

    printf("the min value is: %d\n",minimal(1,2));
    potencia = pow(numero_1,numero_2);
    printf("%i times %i = %i",numero_1,numero_2, potencia);
    
    return 0;

}
```
## Lenguaje assembler del codigo anterior
```
	.file	"main.c"
	.text
	.section	.rodata
.LC0:
	.string	"Ingrese un numero A: "
.LC1:
	.string	"%i"
.LC2:
	.string	"Ingrese un numero B: "
.LC3:
	.string	"the min value is: %d\n"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$48, %rsp
	movl	%edi, -36(%rbp)
	movq	%rsi, -48(%rbp)
	movq	%fs:40, %rax
	movq	%rax, -8(%rbp)
	xorl	%eax, %eax
	leaq	.LC0(%rip), %rdi
	call	puts@PLT
	leaq	-20(%rbp), %rax
	movq	%rax, %rsi
	leaq	.LC1(%rip), %rdi
	movl	$0, %eax
	call	__isoc99_scanf@PLT
	leaq	.LC2(%rip), %rdi
	call	puts@PLT
	leaq	-16(%rbp), %rax
	movq	%rax, %rsi
	leaq	.LC1(%rip), %rdi
	movl	$0, %eax
	call	__isoc99_scanf@PLT
	movl	$2, %esi
	movl	$1, %edi
	call	minimal@PLT
	movl	%eax, %esi
	leaq	.LC3(%rip), %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	-16(%rbp), %eax
	cvtsi2sdl	%eax, %xmm1
	movl	-20(%rbp), %eax
	cvtsi2sdl	%eax, %xmm0
	call	pow@PLT
	cvttsd2sil	%xmm0, %eax
	movl	%eax, -12(%rbp)
	movl	$0, %eax
	movq	-8(%rbp), %rdx
	xorq	%fs:40, %rdx
	je	.L3
	call	__stack_chk_fail@PLT
.L3:
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	 1f - 0f
	.long	 4f - 1f
	.long	 5
0:
	.string	 "GNU"
1:
	.align 8
	.long	 0xc0000002
	.long	 3f - 2f
2:
	.long	 0x3
3:
	.align 8
4:
´´´
