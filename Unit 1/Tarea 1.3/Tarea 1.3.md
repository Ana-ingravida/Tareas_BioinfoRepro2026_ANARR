# Tarea 1.3

**Autora: Ana Ramírez**

### Ejercicio 1

*Crea una variable con el logaritmo base 10 de 50 y súmalo a otra variable cuyo valor sea igual a 5.*

+ Crear la primera variable (log10(50)).

+ Crear la segunda variable (5).

+ Sumar ambas variables.

```
x <- log10(50)
y <- 5
x+y
```

<img width="840" height="137" alt="Evidencia ejercicio 1_Tarea 1 3" src="https://github.com/user-attachments/assets/bc6d33a4-f208-43ad-9b5c-337cbbcb6338" />

*Figura 1*: Resultado del ejercicio 1, en el cual se suman dos variables.



### Ejercicio 2

*Suma el número 2 a todos los números entre 1 y 150.*

* Crear el vector:
  
   `vec.z <- 1:150`

* Crear la variable para número a sumar: 
  
  `num.sum <- 2`

* Crear variable resultado donde se sume el vector y el número a sumar, para obtener el resultado: 
  
  `result <- vec.z + num.sum`

* Ejecutar la variable resultado para resolver la operación: 
  
  `result`

<img width="893" height="220" alt="Evidencia ejercicio 2_Tarea 1 3" src="https://github.com/user-attachments/assets/db607204-16f0-40cf-b6aa-cbdf76a33a3e" />

*Figura 2*: Resultado del ejercicio 2, en el cual se suma un número a todos los números entre 1 y 150.



### Ejercicio 3

*¿Cuántos números son mayores a 20 en el vector -13432:234?*

* Crear el vector: 
  
  `vec.w <- -13432:234`

* Determinar cuales son los números del vector mayores a 20:
  
  `vec.w[vec.w>20]`

* Sumar la cantidad de números del vector mayores a 20, para responder el ejercicio: `sum(vec.w>20)`

<img width="889" height="232" alt="Evidencia ejercicio 3_Tarea 1 3" src="https://github.com/user-attachments/assets/94fd1fb7-0882-42de-be43-03fd697b77e4" />

*Figura 3*: Resultado del ejercicio 3, en el cual se determina cuantos números son mayores a 20 en un vector cuyo valor corresponde al resultado de una división.



### Ejercicio 4

*Carga en R el archivo `PracUni1Ses3/maices/meta/maizteocintle_SNP50k_meta_extended.txt` y ponlo en un objeto de R llamado meta_maiz.*

* Cargar librerias: 
  
  ```
  library(readr)
  library(rstatix)
  ```

* Definir el directorio de trabajo (directorio en el que se encuentra el archivo descargado):
  
  `Session > Set Working Directory > Choose directory`

* Utilizar la función read.delim para abrir el archivo; colocarlo en un objeto de R llamado meta_maiz: 
  
  ```
  meta_maiz <- read.delim("Maiz_U1S3.txt")
  meta_maiz
  ```

<img width="911" height="471" alt="Evidencia ejercicio 4_Tarea 1 3" src="https://github.com/user-attachments/assets/5a133c26-d5d2-4465-b2a7-2e474a1f79ba" />

*Figura 4:* Resultado del ejercicio 4, en el cual se aprende a cargar archivos dentro de R.

### Ejercicio 5

*a) Escribe un for loop para que divida 35 entre 1:10 e imprima el resultado en la consola.*

```
for (i in 1:10){ a=35/i
print(a)
}
```

<img width="850" height="203" alt="Evidencia ejercicio 5 a_Tarea 1 3" src="https://github.com/user-attachments/assets/5cc4e768-8feb-42d9-b95e-f1f9fa8df2f1" />

*Figura 5:* Resultado del ejercicio 5a, en el cual se obtiene el loop resultante de dividir 35 entre todos números del 1 al 10.

*b) Modifica el loop anterior para que haga las divisiones solo para los números nones (con un comando, NO con `c(1,3,...)`).*

* Crear un vector que contenga los números a evitar (impares): 
  
  `none <- c(1, 3, 5, 7, 9)`

* Incluir el vector en el código al igual que la función `next`, la cual sirve como indicación para saltarse dichos números en la operación:

```
for (i in 1:10) {
  if (i %in% none){
    next
  }
  print(paste(35/i))
}
```

<img width="894" height="192" alt="Evidencia ejercicio 5 b_Tarea 1 3" src="https://github.com/user-attachments/assets/5412a63c-c632-4ed9-aaa1-3cef26218ddf" />

*Figura 5:* Resultado del ejercicio 5b, en el cual se obtiene el loop resultante de dividir 35 entre todos números pares del 2 al 10.

*c) Modifica el loop anterior para que los resultados de correr todo el loop se guarden en una df de dos columnas, la primera debe tener el texto "resultado para x" (donde x es cada uno de los elementos del loop) y la segunda el resultado correspondiente a cada elemento del loop. Pista: el primer paso es crear un vector fuera del loop.*

* Crear un `data.frame` vacío: 
  
  `df <- data.frame(resulti = NULL, result35_i= NULL)`

* Crear un `data.frame` auxiliar que contendrá los nombres/comandos de las variables interés de la operación:
  
  ```
  i <- 1:10
  c = paste0("El resultado para ", i, " es: ")
  d = 35/1:10
  soc <- data.frame(result_i = c,result_35_i = d)
  ```
  
  ***NOTA***: result_i contendrá los resultados para los valores que tomará i, result_35_i contendrá los valores que tomará el loop al realizar la operación 35/i.

* Utilizar la función `rbind` para unir ambos `data.frame` en la operación:
  
  ```
  for (i in 1:10) {
    df <- rbind (df,soc)
    }
  df
  ```

<img width="963" height="479" alt="Evidencia ejercicio 5 c_Tarea 1 3" src="https://github.com/user-attachments/assets/402ca02d-017a-46da-a92d-7f7c8be6c85a" />

*Figura 5:* Resultado del ejercicio 5c, en el cual se organizan los resultados en 2 columnas; en la primera se encuentra el texto y en la segunda el resultado numérico de la operación.




