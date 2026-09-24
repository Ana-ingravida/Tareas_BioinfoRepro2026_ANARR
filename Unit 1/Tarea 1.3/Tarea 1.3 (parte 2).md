# Tarea 1.3 (parte 2)

**Autora: Ana Ramírez**

###Ejercicio 6

*Abre en RStudio el script `PracUni1Ses3/mantel/bin/1.IBR_testing.r`. Este script realiza un análisis de [aislamiento por resistencia](http://www.bioone.org/doi/abs/10.1554/05-321.1) con Fst calculadas con ddRAD en Berberis alpina.*

*Lee el código del script y determina:*

* **¿Qué hacen los dos for loops del script?**
  
  El objetivo es realizar un Mantel test, prueba utilizada en ecología para evaluar la correlación entre dos matrices de distancia calculadas entre pares de muestras. Para lograrlo es necesario crear un primer loop, que permita obtener la matriz de distancia efectiva (para comprobar la autocorrelación espacial) y la media de la matriz para cada trama; luego se debe crear un segundo loop, que permita realizar consecutivamente el Mantel test para cada condición de estudio.

* **¿Qué paquetes necesitas para correr el script?**
  
  Se requieren de los paquetes ade4, sp y ggplot2.
  
  ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-09-23-22-05-57-image.png)

* **¿Qué paquetes necesitas para correr el script?**
  
  Es necesario contar con los siguientes archivos: surveyed_mountains.tsv, BerSS.sumstats.tsv, BerSS.fst_summary.tsv, Balpina_focalpoints.txt.

### Ejercicio 7

*Escribe una función llamada `calc.tetha` que te permita calcular tetha dados Ne y u como argumentos. Recuerda que tetha =4Neu.*

*Include the comment: Red banannas*

* Definir el objeto en el cual estará la función: 
  
  `calc.tetha <- function(Ne, u)`

* Indicar las instrucciones e identificar partes de la función:
  
  ```
  ##Función que calcula tetha dado Ne y u
    ##Argumentos
    #Ne
    #u
    ##Función
    #Cálculo de tetha:
    calculo.tetha <- 4*Ne*u
    return(paste("Red bananas", calculo.tetha))
  ```
  
  ***NOTA***: `return` se coloca para que el resultado de la función vuelva al objeto asociado.

* Dar valores a los argumentos: 
  
  `calc.tetha(Ne= 3, u=2)`

![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-09-23-22-26-19-image.png)

# 
