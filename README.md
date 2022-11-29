PAV - P4: reconocimiento y verificación del locutor
===================================================

Obtenga su copia del repositorio de la práctica accediendo a [Práctica 4](https://github.com/albino-pav/P4)
y pulsando sobre el botón `Fork` situado en la esquina superior derecha. A continuación, siga las
instrucciones de la [Práctica 2](https://github.com/albino-pav/P2) para crear una rama con el apellido de
los integrantes del grupo de prácticas, dar de alta al resto de integrantes como colaboradores del proyecto
y crear la copias locales del repositorio.

También debe descomprimir, en el directorio `PAV/P4`, el fichero [db_8mu.tgz](https://atenea.upc.edu/mod/resource/view.php?id=3654387?forcedownload=1)
con la base de datos oral que se utilizará en la parte experimental de la práctica.

Como entrega deberá realizar un *pull request* con el contenido de su copia del repositorio. Recuerde
que los ficheros entregados deberán estar en condiciones de ser ejecutados con sólo ejecutar:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~.sh
  make release
  run_spkid mfcc train test classerr verify verifyerr
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Recuerde que, además de los trabajos indicados en esta parte básica, también deberá realizar un proyecto
de ampliación, del cual deberá subir una memoria explicativa a Atenea y los ficheros correspondientes al
repositorio de la práctica.

A modo de memoria de la parte básica, complete, en este mismo documento y usando el formato *markdown*, los
ejercicios indicados.

## Ejercicios.

### SPTK, Sox y los scripts de extracción de características.

- Analice el script `wav2lp.sh` y explique la misión de los distintos comandos involucrados en el *pipeline*
  principal (`sox`, `$X2X`, `$FRAME`, `$WINDOW` y `$LPC`). Explique el significado de cada una de las 
  opciones empleadas y de sus valores.
`sox` :
El programa sox es pot utilitzar per generar una senyal, sense capcelera, amb el format adecuat a partir d'una altra senyal amb un altre format. A més a més, permet la conversió de senyals guardades en un programa extern. Al fitxer d'entrada se li poden aplicar els paràmetres següents:
  -t: Tipus de fitxer d'àudio
   e: Indica la codificació que es vol aplicar
   b: Indica el número de bits
   -: Redirecció del output, pipeline

`$X2X` :
És un programa de SPTK que converteix dades a un diferent format. Els formats d'entrada i sortida són especificats a la línia de comandos. Com per exemple convertir el formats ASCII a float. A la línia de comandos, especifiques el type1, que és el format d'entrada i el type2 que és el de sortida. A més pot afegir dos paràmetres més:
-r: Especificar l'arrodoniment quan se substitueix un nombre real
un nombre enter. Per defecte és false.
-o: Retalla per mínim i màxim del tipus de dades de sortida si s'introdueix
les dades superen l'interval del tipus de dades de sortida. si l'opció -o
no es dóna, quan les longituds del tipus de dades són diferents.Per defecte és false.
%format: Especificar un format de sortida similar a 'printf()', només si el tipus2 és ASCII.


`$FRAME`: Converteix una seqüència de dades d'entrada d'un fitxer d'entrada (o entrada estàndard) a una sèrie de fotogrames possiblement superposades amb període P i longitud L, i envia el resultat a la sortida estàndard.
Té els següents paràmetres:

-l: Longitud de mostres de cada trama. Valor per defecte és de 256.
-p: Indica el període de les trames. Valor per defecte de 100.
-n: Aquesta opció s'utilitza quan, en comptes de tenir x(0) com a centre
punt del primer fotograma, es vol fer que x(0) sigui el primer punt
del primer fotograma.

`$WINDOW`: Pondera cada trama amb una finestra. Multiplica, element per element, els vectors d'entrada de longitud L des de l'arxiu d'entrada (o entrada estàndard) mitjançant una funció de finestra especificada, enviant el resultat a la sortida estàndard. Té els següents paràmetres:

-l: Longitud trama d'entrada. Valor per defecte de 256
-L: Longitud trama d'entrada. Valor per defecte és el valor de l'entrada
-n: Tipos de normalització
-w: Tipus de finestra. Per defecte és de Blackman


`$LPC`: Calcula els coeficients de la predicció linear de cada trama enfinestrada del fitxer d'entrada. Tant la senyal d'entrada com la de sortida tene un format float. L'anàlisi LPC es fa mitjançant el mètode Levinson-Durbin. Té els següents paràmetres:

-l: Longitud de la trama. Valor per defecte de 256
-m: Ordre del LPC. Valor per defecte de 25
-f: Valor mínim del determinant. Valor per defecte de 0.000001


- Explique el procedimiento seguido para obtener un fichero de formato *fmatrix* a partir de los ficheros de
  salida de SPTK (líneas 45 a 51 del script `wav2lp.sh`).

  * ¿Por qué es más conveniente el formato *fmatrix* que el SPTK?

- Escriba el *pipeline* principal usado para calcular los coeficientes cepstrales de predicción lineal
  (LPCC) en su fichero <code>scripts/wav2lpcc.sh</code>:

- Escriba el *pipeline* principal usado para calcular los coeficientes cepstrales en escala Mel (MFCC) en su
  fichero <code>scripts/wav2mfcc.sh</code>:

### Extracción de características.

- Inserte una imagen mostrando la dependencia entre los coeficientes 2 y 3 de las tres parametrizaciones
  para todas las señales de un locutor.
  
  + Indique **todas** las órdenes necesarias para obtener las gráficas a partir de las señales 
    parametrizadas.
  + ¿Cuál de ellas le parece que contiene más información?

- Usando el programa <code>pearson</code>, obtenga los coeficientes de correlación normalizada entre los
  parámetros 2 y 3 para un locutor, y rellene la tabla siguiente con los valores obtenidos.

  |                        | LP   | LPCC | MFCC |
  |------------------------|:----:|:----:|:----:|
  | &rho;<sub>x</sub>[2,3] |      |      |      |
  
  + Compare los resultados de <code>pearson</code> con los obtenidos gráficamente.
  
- Según la teoría, ¿qué parámetros considera adecuados para el cálculo de los coeficientes LPCC y MFCC?

### Entrenamiento y visualización de los GMM.

Complete el código necesario para entrenar modelos GMM.

- Inserte una gráfica que muestre la función de densidad de probabilidad modelada por el GMM de un locutor
  para sus dos primeros coeficientes de MFCC.

- Inserte una gráfica que permita comparar los modelos y poblaciones de dos locutores distintos (la gŕafica
  de la página 20 del enunciado puede servirle de referencia del resultado deseado). Analice la capacidad
  del modelado GMM para diferenciar las señales de uno y otro.

### Reconocimiento del locutor.

Complete el código necesario para realizar reconociminto del locutor y optimice sus parámetros.

- Inserte una tabla con la tasa de error obtenida en el reconocimiento de los locutores de la base de datos
  SPEECON usando su mejor sistema de reconocimiento para los parámetros LP, LPCC y MFCC.

### Verificación del locutor.

Complete el código necesario para realizar verificación del locutor y optimice sus parámetros.

- Inserte una tabla con el *score* obtenido con su mejor sistema de verificación del locutor en la tarea
  de verificación de SPEECON. La tabla debe incluir el umbral óptimo, el número de falsas alarmas y de
  pérdidas, y el score obtenido usando la parametrización que mejor resultado le hubiera dado en la tarea
  de reconocimiento.
 
### Test final

- Adjunte, en el repositorio de la práctica, los ficheros `class_test.log` y `verif_test.log` 
  correspondientes a la evaluación *ciega* final.

### Trabajo de ampliación.

- Recuerde enviar a Atenea un fichero en formato zip o tgz con la memoria (en formato PDF) con el trabajo 
  realizado como ampliación, así como los ficheros `class_ampl.log` y/o `verif_ampl.log`, obtenidos como 
  resultado del mismo.
