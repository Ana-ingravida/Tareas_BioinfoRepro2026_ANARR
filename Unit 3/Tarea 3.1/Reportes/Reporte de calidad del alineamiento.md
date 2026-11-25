# Reporte de calidad del alineamiento

**Fecha: 18 de noviembre de 2026**

**Autora: Ana Ramírez Rivas**

### Introducción

La secuencia paired-ended (S10_sorted_RG.bam) fue filtrada según ciertos parámetros de calidad y alineada (mapeo con BWA-MEM) con el genoma de referencia **hg19**. Utilizando la herramienta qualimap se generó un reporte técnico de calidad del alineamiento.

### Materiales y métodos

Pueden ser revisados en los siguientes enlaces:

* [Filtrado](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%202/Tarea%202.1/README.md)

* [Alineamiento]()

### Resultados

***1. Coverage across reference***

<img width="792" height="575" alt="Tuto alineamiento 5 1" src="https://github.com/user-attachments/assets/5f51787e-3a17-49e2-bef5-c60f02eb7b3d" />

*Figura 1:* En la sección superior esta la cobertura, el reporte indica una cobertura promedio de 102,08X (con una desviación estándar de 101,45X), en el gráfico se observan picos que van desde unos 39X hasta valores por encima de 150X; la cobertura se presenta como picos aislados, separados por tramos sin cobertura, esto es coherente con un panel dirigido / captura de regiones (BED) de extensión moderada (en el reporte, el tamaño total de las regiones es de 91,120 bp). En la sección inferior esta el porcentaje de GC, dentro de regiones el valor reportado es de 47,6%.

***2. Coverage Histogram***

<img width="792" height="572" alt="Tuto alineamiento 5 2" src="https://github.com/user-attachments/assets/b5a14964-86b3-414e-8582-c904b02630c8" />

*Figura 2:* El Eje X (Coverage (X)) agrupa los niveles de cobertura en intervalos o rangos, partiendo en 0 hasta 697. El Eje Y (Number of genomic locations) indica cuántas posiciones genómicas (bases) caen dentro de cada rango de cobertura. En el gráfico se observa que la mayor concentración de posiciones genómicas se ubica entre 99-122X y 152-188X (en torno a la cobertura promedio de 102,08X). Destaca una barra elevada en cobertura 0 (más de 7.000 posiciones) y un comportamiento irregular en los rangos bajos e intermedios, lo que refleja bases objetivo sin cobertura o con captura heterogénea dentro de las regiones evaluadas.

***3. Duplication Rate Histogram***

<img width="785" height="570" alt="Tuto alineamiento 5 3" src="https://github.com/user-attachments/assets/e996da49-6adb-4990-9caa-76de395f17a7" />

*Figura 3:*  El Eje X (Duplication rate) representa los valores de tasa/nivel de duplicación desde 1 hasta 50. El Eje Y (Number of loci) indica la cantidad de loci que presentan cada valor de duplicación. En el gráfico se observa que la gran mayoría de los loci se concentran en tasas bajas de duplicación, con un máximo marcado en 1 (aproximadamente 4.500 loci); a partir del valor 2 la tasa de duplicación decrece 2; en el extremo 50 aumenta discretamente (cerca de 350–375 loci). Esto podría sugerir una evaluación de la librería y el tratamiento de duplicados antes de análisis posteriores.

***4. Mapping Quality Histogram***

<img width="776" height="567" alt="Tuto alineamiento 5 4" src="https://github.com/user-attachments/assets/6a986e65-1b8a-4ab5-921b-df66f1aac85c" />

*Figura 4:* El Eje X (Mapping quality) representa los valores de calidad de mapeo en escala Phred desde 0 hasta 60. El Eje Y (Number of genomic locations) indica la cantidad de posiciones genómicas que caen en cada nivel o intervalo de calidad de mapeo. El gráfico muestra que la gran mayoría de las posiciones genómicas se concentran en la calidad máxima de mapeo de 60, en concordancia con la calidad promedio de 58,82 dentro de las regiones y lo que refleja alineamientos confiables sobre el genoma de referencia.

### Conclusiones

El alineamiento presenta una excelente tasa de mapeo (99,92% de las 63.248 lecturas mapeadas, 97,38% dentro de las regiones objetivo y calidad promedio de mapeo de 58,82). No obstante, se identificó:

* Dentro de las regiones blanco se estiman 54.142 lecturas duplicadas (87,91%), con una tasa de duplicación de 39,19%; esto podría explicarse por sobreamplificación por PCR durante la preparación de la librería o baja calidad de ADN inicial, lo que genera múltiples copias del mismo fragmento original.

* La longitud media de las lecturas es de 229,33 bp mientras que el tamaño promedio del inserto es de 250,48 bp; esto podría explicarse por fragmentación excesiva del ADN o diseño de amplicones cortos (~250 bp) secuenciados en formato 2×250 bp, al ser cada lectura casi tan larga como el inserto completo, el par R1 y R2 se podrían solapar.

* La tasa general de error es baja (0,26%), sin embargo el 61,33% de las inserciones y deleciones detectadas dentro de regiones ocurren en homopolímeros, con 1.838 deleciones  y 1.193 inserciones; esto podría explicarse por errores de polimerasa  en la preparación de la librería o durante la secuenciación en tramos repetitivos de un mismo nucleótido, o indels verdaderos de la muestra en regiones repetitivas o ligeras ambigüedades de alineamiento con BWA-MEM antes de un realineamiento local.


