 # Trabajo Práctico #1 - Fases de traducción y errores

## Enunciado: 

Objetivos
* Identificar las fases de traducción y errores.

Temas
* Fases de traducción.
* Preprocesamiento.
* Compilación.
* Ensamblado.
* Vinculación (Link).
* Errores en cada fase.

Tareas
1. Investigar las funcionalidades y opciones que su compilador presenta para
limitar el inicio y fin de las fases de traducción.
2. Para la siguiente secuencia de pasos:
	a. Transicribir en readme.md cada comando ejecutado.
	b. Describir en readme.md el resultado u error obtenidos para cada paso

### Secuencia de Pasos

1. Escribir hello2.c, que es una variante de hello.c:
```
	#include <stdio.h>
	int/*medio*/main(void){
		int i=42;
		 prontf("La respuesta es %d\n");
```

2. Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su
contenido.

3. Escribir hello3.c, una nueva variante:
```
	int printf(const char *s, ...);

	int main(void){
		int i=42;
		prontf("La respuesta es %d\n");
```
4. Investigar la semántica de la primera línea.
5. Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias
entre hello3.c y hello3.i.
6. Compilar el resultado y generar hello3.s, no ensamblar.
7. Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar
hello4.s, no ensamblar.
8. Investigar hello4.s.
9. Ensamblar hello4.s en hello4.o, no vincular.
10.Vincular hello4.o con la biblioteca estándar y generar el ejecutable.
11.Corregir en hello5.c y generar el ejecutable.
12.Ejecutar y analizar el resultado.
13.Corregir en hello6.c y empezar de nuevo.
14.Escribir hello7.c, una nueva variante:
```
	int main(void){
		int i=42;
		 printf("La respuesta es %d\n", i);
	}
```
15.Explicar porqué funciona.

Restricciones
* El programa ejemplo debe enviar por stdout la frase "La respuesta es 42", el
valor 42 debe surgir de una variable.

Productos:
```
	DD-FasesErrores
	|-- readme.md
	|-- hello2.c
	|-- hello3.c
	|-- hello4.c
	|-- hello5.c
	|-- hello6.c
	`-- hello7.c
```

## Resolución: 

Punto 1 y 2) 
Se ejecutó el comando:

> gcc -E Hello2.c -o Hello2.i 

Esto generó como le pedimos, el archivo Hello2.i y al abrir su contenido podemos notar que se reemplazó el `#include<stdio.h>` por todo el contenido de ese archivo, es decir, el codigo que contenia stdio.h que es la cabecera o "header" y contiene el prototipo de las funciones, no todo el código fuente. Al final del mismo se puede ver que está el código entero de nuestro programa, sin otra modificación que el reemplazo del comentario /*medio*/ entre "int" y "main" por un unico espacio. Esto es lo que se encarga de hacer el preprocesador entre otras cosas. 

Punto 3 y 4)
La linea: 

> int printf(const char *s, ...);

está declarando el prototipo de la función llamada printf. La cual sabemos por el int del principio, que retorna un valor de este tipo, y recibe como primer parámetro una constante de tipo puntero a char, y una cantidad indeterminada de parámetros. 

Punto 5)
Al ejecutar: 
> gcc -E Hello3.c -o Hello3.i 

Podemos notar abriendo el archivo, que esta vez no se trajo ninguna información ni datos que no estén en nuestro código. Ya que sólo definimos el prototipo que vamos a utilizar. 

Punto 6)
Se ejecutó: 
> gcc -S Hello3.c -o Hello3.s

Y la consola me tira error, no reconoce la palabra prontf y nos sugiere usar printf. Tambien tira en main "error: expected declaration or statement at end of input", en la cual se debe referir a que falta una llave que cierre el main. 

Punto 7 y 8)
Se ejecutó: 
> gcc -S Hello4.c -o Hello4.s

Y al abrir el archivo, se obserba el programa en lenguaje ensamblador como se esperaba. 
Además, en el main se observan los comandos similares vistos en clases, con respecto al stack que se va modificando a medida que se ejecuta el programa. Se obserban los comandos pushq, movq, subq, call y pop entre otros. 

Punto 9)
Se ejecutó: 
> gcc -C Hello4.s -o Hello4.o

Se generó correctamente, y no se puede leer al abrirse (Tiene sentido porque está en la etapa en la que el código esta en lenguaje máquina).

Punto 10) 
Se ejecutó: 
> gcc Hello4.0 -o Hello4.exe

Y no generó nada, dió error. Debe ser porque no está definida la biblioteca estandar.

Punto 11 y 12)
Se corrigió, y con el comando: 
> gcc Hello5.c -o Hello5.exe
Se generó el ejecutable. Pero al probarlo, veo que no me da el valor que quiero, sino que da otro valor cualquiera. Si volvemos a ejecutar, observamos que nos da otro valor distinto. Es porque el i=42 está definido fuera del printf y no sabe que de dónde tiene que sacar el valor para mostrar en pantalla. Se soluciona agregando el i como 2do parámetro del printf.

Punto 13) 
Se agregó:
> printf("La respuesta es %d\n", i);

Punto 14 y 15)
Funciona, porque se linkea de forma implícita con la biblioteca estándar de C. De todos modos la consola tira un warning.
