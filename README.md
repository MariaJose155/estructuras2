María José Arce Marín
B60561

Heillen Sosa

Darieth Saddam Fonseca Zúñiga
B62738

# INTRODUCCIÓN
Con este tema se pretende aprender como implementar Tomasulo algoritmo para ejecutar instrucciones fuera de orden haciendo uso de tablas RAT, una cola de instrucciones y Regs. Aplicando esto se pretende eliminar la ejecución en orden (poco eficiente generando burbujas) del programa y en lugar de esto anterior ejecutar instrucciones tan pronto los operandos en la estación de reserva esten listos.


# CONCEPTOS 

## IPL:

Propone:

-El hardware reorganiza la ejecución de instrucciones.

-Es más poderoso que la programación estática (SW).

-Elimina el requerimiento de que las instrucciones se ejecuten en el orden del programa.

-Aprovechar el pipeline al máximo.

Se basa en dos enfoques: estático basado en software y dinamico basado en hardware

A) Técnicas estátics (compilador)

Entonces tenemos lo siguiente

LOOP:

1.LD FO,0(R1)

2.ADD F4, F0, F2

3.SD F4,0(R1)

4.ADDIU R1,R1,#-8

5.BNE R1,R2,LOOP

Entonces notemos que en la linea 1 y 2 con la dependencia F0 se da un stall (se inserta una burbuja), en la linea 3 con la dependencia  F4 se da un stall y en la linea 4 y 5 con la dependencia R1 se da un stall.

Entonces podes se puede hacer el siguiente cambio:

1.LD FO,0(R1)

2.ADDIU R1,R1,#-8

3.ADD F4, F0, F2

4.SD F4,8(R1)

5.BNE R1,R2,LOOP

Donde el único stall se da en la linea 4 con F4.Entonces se detentan dependencias simples en el codigo y se reordena para evitar burbujas.

B) Loop unralling: Aumentar el número de instrucciones relativas al branch para aumantar el número de instrucciones sobre las cuales se detecta paralelismo.

Entonces en general para ILP: se detentan dependencias simples en el codigo y se reordena para evitar burbujas.

## OOO (Ejecución fuera de orden):

-La ejecución de las operaciones inicia tan pronto sus operandos esten listos.

-El problema de la ejecución fuera de orden es que existe la posibilidad de introducir Hazards WAR y WAW que no existen en el pipeline con ejecución en orden.

-La solución a este problema es renombrar el registro, separar el concepto de registros de arquitectura y registros físicos.

## Registros de Arquitectura:

Son los disponibles para el compilador. EL compilador no usa registros físicos porque hay N registros físicos y hay un límite de registros que la arquitectura nos permite exponer.

## Registros físicos:

Son los registros en hardware donde se puede almacenar un valor.

## Tabla RAT (register alias table):

Es una tabla donde se lleva el mapeo entre registros de arquitectura y registros físicos.

## Tomasulo:

Es un algoritmo que permite ejecutar instrucciones fuera de orden.

-Emite las instrucciones en orden del procesador.

-Determina de donde vienen las operaciones (RF,RS).

-Obtiene una estacion de reserva.

-Etiqueta el registro destino.

*Tomasulo(ciclos y restricciones):

-Capture y Dispath (típicamente no se realizan en el mismo ciclo)

-Issue y Dispath (típicamente no se realizan en el mismo ciclo)

-RAT-Write y Issue (observar el issue)

-Load y store(esto se realiza en orden)

## Estación de reserva:

Es una tabla que posee una cantidad limitada de espacios por tipo de instrucción y que sirve para esperar a que los operadores de una instrucción dejen de ser un registro y se conviertan en una constante, una vez eso sucede se hace Dispatch y se libera el espacio en la estación de reserva para la siguiente instrucción en la cola.

# EJEMPLOS
# QUIZ
1. Usando el algoritmo Tomasulo, ¿cuándo puede ocurrir Dispatch?

R/ Cuando una instrucción se envia a la unidad de reserva y sus operandos ya no son un registro entonces se le puede hacer Dispatch, eso si, si es una instrucción de tipo Load o Store entonces aunque esten listas deberan esperar su turno, ya que se realizan en orden.

2. Mencione una de las proposiciones de IPL con enfoque en programación dinámica.

R/ El hw reorganiza la ejecución de inst, usando las dependecias. Es más poderoso que la programación estática (sw). Elimina los requerimientos de que las inst se ejecuten en el orden del programa. Ejecuta una inst apenas la misma esté lista para hacerlo. 

3. Mencione la diferencia entre registros físicos y de arquitectura.

R/ De arquitectura: Son los disponibles para el compilador (programador). EL compilador no usa registros físicos porque hay N registros físicos y hay un límite de registros que la arquitectura nos permite exponer. Físicos: Son los registros en hardware donde se puede almacenar un valor.

4. Use el algoritmo de tomasulo sobre las siguientes instrucciones para determinar en qué período se ejecutan (Issue, Dispatch, Write). Considere los siguientes tiempos de ejecución: add = 5 ciclos, sub = 5 ciclos, mul = 12 ciclos, además considere que solo tiene una unidad funcional de add/sub y de mul. Los valores iniciales son R1 = 2, R2 = 1, R3 = -2, R4 = 3.

Intrucciones: 

sub R1, R2, R3

add R2, R1, R3

mul R1, R4, R2

Ciclo 0

<img src="../master/quiz1.jpeg" width ="450">

Ciclo 1

<img src="../master/quiz2.jpeg" width ="450">

Ciclo 2

<img src="../master/quiz3.jpeg" width ="450">

Ciclo 9

<img src="../master/quiz4.jpeg" width ="450">

Ciclo 15

<img src="../master/quiz5.jpeg" width ="450">

Ciclo 27

<img src="../master/quiz6.jpeg" width ="450">

 
# BIBLIOGRAFIA

"Microprocessor Architecture: From simple pipelines to chip multiprocessors",Baer J-L,University of Washington, Seattle, 2010

