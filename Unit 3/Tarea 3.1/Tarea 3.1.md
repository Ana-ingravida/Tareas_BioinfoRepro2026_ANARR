# Tarea 3.1

### Unidad 3

#### Bioinformática e investigación reproducible para análisis genómicos

**Fecha: 18 de noviembre de 2026**

**Autora: Ana Ramírez Rivas**

#### Tutorial para el filtro y alineamiento de lecturas

1. *Realizar el alineamiento contra el genoma humano hg19 de las lecturas R1 y R2 del paciente seleccionado para la tarea de control de calidad de lecturas de secuencia.*
   
   Muestras utilizadas en el ensayo: **S10_R1_filter3.fastq.gz y S10_R2_filter3.fastq.gz**
   
   ###### Alineamiento de lecturas
   
   *1.1 Alinear lecturas contra el genoma de referencia*
   
   Se utilizó el siguiente código: 
   
   ```
   bwa mem -t 4 -M /datos/reference/genomes/hg19_reference/hg19.fasta \
   S10_R1_filter3.fastq.gz \
   S10_R2_filter3.fastq.gz \
   > S10.sam
   ```
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-18-19-38-15-image.png)
   
   *Figura 1*: Resultado al aplicar el código descrito para realizar el alineamiento contra el genoma humano hg19 de las lecturas S10_R1_filter3.fastq.gz y S10_R2_filter3.fastq.gz.
   
   *1.2 Convertir archivo sam a bam (binario)*
   
   Se obtiene un archivo bam a partir del alineamiento en formato sam.
   
   Se utilizó el siguiente código:
   
   ```
   java -jar /opt/picard/picard-2.25.2/picard.jar SamFormatConverter -I S10.sam -O S10.bam
   ```
   
   *1.3 Ordenar lecturas alineadas por posición*
   
   Obtenemos un archivo bam con las lecturas ordenadas por posición del alineamiento a la referencia.
   
   Se utilizó el siguiente código:
   
   ```
   java -jar /opt/picard/picard-2.25.2/picard.jar SortSam -I S10.bam -O S10_sorted.bam -SO coordinate
   ```
   
   *1.4 Añadir el campo Readgroup al archivo bam*
   
   Se utilizó el siguiente código:
   
   ```
   java -jar /opt/picard/picard-2.25.2/picard.jar AddOrReplaceReadGroups -I S10_sorted.bam -O S10_sorted_RG.bam -ID sample -LB Paired-end -PL Illumina -PU Unknown -SM sample
   ```
   
   *1.5 Indexar alineamiento*
   
   Se utilizó el siguiente código:
   
   ```
   samtools index S10_sorted_RG.bam
   ```

2. *Utilizando una línea de comando, encuentre la primera lectura en el archivo SAM que contenga bases enmascaradas (secuencias suavizadas por soft-clipping).*
   
   Se utilizó el siguiente código:
   
   ```
   grep -v '^@' S10.sam | awk '$6 ~ /S/ {print; exit}'
   ```
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-18-19-50-11-image.png)
   
   *Figura 2*: Resultado al aplicar el código descrito para encuentrar la primera lectura  que contenga bases enmascaradas en el archivo S10.sam.

3. *Muestre el registro de las lecturas en el archivo SAM e identifique y explique el código CIGAR de esa lectura.*
   
   Se utilizó el siguiente código:
   
   ```
   grep -v '^@' S10.sam | awk '$6 ~ /S/ {printf("QNAME=%s FLAG=%s RNAME=%s POS=%s MAPQ=%s CIGAR=%s\n",$1,$2,$3,$4,$5,$6); exit}'
   ```
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-18-20-14-32-image.png)
   
   *Figura 3*: Resultado al aplicar el código descrito para mostrar el registro de las lecturas en el archivo SAM e identificar el código CIGAR.
   
   ¿Cómo se interpreta?
   
   **M**: coincidencia/diferencia contra la referencia.
   
   **I**: Inserción en la lectura.
   
   **D**: deleción respecto a la referencia.
   
   **N**: salto en la referencia.
   
   **S**: secuencias suavizadas (*Soft clipping*).
   
   **H**: secuencias recortadas (*Hard clip*).
   
   Mi resultado: **CIGAR= 20S231M**, 20 bases fueron “suavemente recortadas” (soft-clipped) y 231 bases de la lectura están alineadas (ya sea que coincidan o se diferencien en alguna base) con la referencia.

4. *Generar un reporte técnico de calidad del alineamiento con **qualimap**.*
   
   Se cargó el módulo al usar el cluster genoma:
   
   ```
   module load qualimap
   ```
   
   Se modificó una variable de ambiente para que qualimap no utilice una opción depricada de Java:
   
   ```
   export "JAVA_OPTS=-Xms32m -Xmx1200M"
   ```
   
   Se utilizó el siguiente código: 
   
   ```
   qualimap bamqc -bam S10_sorted_RG.bam -gff ~/181004_curso_calidad_datos_NGS/regiones_blanco.bed -outdir ./S10_sorted_RG
   ```
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-16-35-31-image.png)
   
   *Figura 4*: Resultado al aplicar el código descrito para Generar un reporte técnico de calidad del alineamiento con **qualimap**.

5. *Seleccionar 4 figuras que a su juicio sean las más informativas sobre la calidad de los datos y del ensamble.*
   
   **5.1 Coverage across reference** 
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-16-58-19-image.png)
   
   *Figura 5:* El gráfico muestra que la cobertura se presenta como picos aislados, separados por tramos sin cobertura, esto es coherente con un análisis restringido a regiones blanco de captura y con una alta variabilidad entre regiones (promedio de 102,08X). También se observa que los picos de contenido GC en las posiciones cubiertas se mantienen en rangos moderados acordes al promedio de 47,6% dentro de las regiones, sin mostrar extremos de GC que indiquen un sesgo severo.
   
   **5.2 Coverage Histogram**
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-17-01-08-image.png)
   
   *Figura 6:* En el gráfico se observa que la mayor concentración de posiciones genómicas se ubica entre 99-122X y 152-188X (en torno a la cobertura promedio de 102,08X). Destaca una barra elevada en cobertura 0 (más de 7.000 posiciones) y un comportamiento irregular en los rangos bajos e intermedios, lo que refleja bases objetivo sin cobertura o con captura heterogénea dentro de las regiones evaluadas.
   
   **5.3 Duplication Rate Histogram**
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-17-03-43-image.png)
   
   *Figura 7:* En el gráfico se observa que la gran mayoría de los loci se concentran en tasas bajas de duplicación, con un máximo marcado en 1 (aproximadamente 4.500 loci); a partir del valor 2 la tasa de duplicación decrece 2; en el extremo 50 aumenta discretamente (cerca de 350–375 loci). Esto podría sugerir una evaluación de la librería y el tratamiento de duplicados antes de análisis posteriores.
   
   **5.4 Mapping Quality Histogram**
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-17-06-49-image.png)
   
   *Figura 8:* El gráfico muestra que la gran mayoría de las posiciones genómicas se concentran en la calidad máxima de mapeo de 60, en concordancia con la calidad promedio de 58,82 dentro de las regiones y lo que refleja alineamientos confiables sobre el genoma de referencia. 

6. *Incluir las figuras en la sección de Resultados de un reporte técnico. Describir cada figura con una leyenda descriptiva. Adicionalmente, en el texto de la sección, interpretar los resultados y citar cada figura. Debe referirse a la calidad de los datos y del alineamiento. Enfóquese especialmente en los posibles problemas con los datos o alineamientos. Comente potenciales razones que expliquen lo observado. Incluya una sección con las principales **Conclusiones** para la muestra.*
   
   **El Reporte técnico se encuentra aquí:** [Reporte de calidad del alineamiento](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%203/Tarea%203.1/Reportes/Reporte%20de%20calidad%20del%20alineamiento.md)

7. *Incluya el reporte completo generado con **qualimap** como anexo.*

8. *Comparta el informe en formato markdown a través de github para dar por completada esta tarea.*

**Anexo**

![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-22-25-24-image.png)

**Reporte generado con *qualimap*:** Se encuentra subido en el repositorio de github, puede acceder a través de este enlace: [Reporte](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%203/Tarea%203.1/File/qualimapReport.html)

#### Conclusiones

Un buen alineamiento al genoma de referencia es fundamental en el análisis de datos NGS porque determina la exactitud con la que cada lectura se ubica en su locus genómico real, evitando errores de mapeo que pueden generar falsos resultados en el llamado de variantes , distorsionar la estimación de cobertura o duplicados y sesgar los análisis posteriores. 

El reporte de qualimap para la muestra "S10_sorted_RG.bam" sobre las regiones blanco evidencia un mapeo global muy eficiente, no obstante, alberga aspectos que deben ser atendidos antes del análisis de variantes: una alta estimación de duplicados, un solapamiento entre lecturas pareadas y el tamaño promedio del inserto, y una alta proporción de indels en homopolímeros, por lo que sería bueno verificar el tipo de librería utilizada y mejorar filtros de calidad.

#### Tutorial para el llamado de variantes

1. *Seguir este tutorial con los datos de la muestra previamente elegida. Todas las muestras son de pacientes, para los cuales se sospechaba de una mutación patogénica. Se realizó una secuenciación de un panel de genes con equipamiento MiSeq.*
   
   **Se continua el análisis con la muestra S10**
   
   1.1 Recalibración de calidades de bases (Q) en lecturas:
   
   Se utilizó el siguiente código:
   
   ```
   java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T BaseRecalibrator -R /datos/reference/genomes/hg19_reference/hg19.fasta -I S10_sorted_RG.bam -knownSites /datos/reference/genomes/hg19_reference/dbSNP_hg19.vcf -o S10_recall_data.table
   ```
   
   1.2 Aplicar la recalibración a los datos de secuencia
   
   Se utilizó el siguiente código:
   
   ```
   java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T PrintReads -R  /datos/reference/genomes/hg19_reference/hg19.fasta -I S10_sorted_RG.bam --BQSR S10_recall_data.table -o S10_recall_reads.bam
   ```
   
   **Llamado de variantes**
   
   *1 Identificar variantes:*
   
   Se utilizó el siguiente código:
   
   ```
   java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T HaplotypeCaller -R /datos/reference/genomes/hg19_reference/hg19.fasta -I S10_recall_reads.bam --dbsnp /datos/reference/genomes/hg19_reference/dbSNP_hg19.vcf -stand_call_conf 30 -o S10_raw_variants.vcf
   ```
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-15-07-image.png)
   
   *2 Aplicar filtros a las variantes:*
   * Extraer SNP's: Se utilizó el siguiente código:
     
     ```
     java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T SelectVariants -R /datos/reference/genomes/hg19_reference/hg19.fasta -V S10_raw_variants.vcf -selectType SNP -o S10_RAW_SNP.vcf
     ```
     
     Resultado
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-15-59-image.png)
   
   * Filtrar SNP's: Se utilizó el siguiente código:
     
     ```
     java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T VariantFiltration -R /datos/reference/genomes/hg19_reference/hg19.fasta -V S10_RAW_SNP.vcf --filterExpression "DP <10" --filterName "FILTER" -o S10_FILTERED_SNP.vcf
     ```
     
     Resultado
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-17-17-image.png)
   
   * Extraer InDels: Se utilizó el siguiente código:
     
     ```
     java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T SelectVariants -R /datos/reference/genomes/hg19_reference/hg19.fasta -V S10_raw_variants.vcf -selectType INDEL -o S10_RAW_INDEL.vcf
     ```
     
     Resultado
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-19-42-image.png)
   
   * Filtrar InDels: Se utilizó el siguiente código:
     
     ```
     java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T VariantFiltration -R /datos/reference/genomes/hg19_reference/hg19.fasta -V S10_RAW_INDEL.vcf --filterExpression "DP <10" --filterName "FILTER" -o S10_FILTERED_INDEL.vcf
     ```
     
     Resultado
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-20-56-image.png)
   
   * Combinar vcfs filtrados: Se utilizó el siguiente código:
     
     ```
     java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T CombineVariants -R /datos/reference/genomes/hg19_reference/hg19.fasta --variant:foo S10_FILTERED_SNP.vcf --variant:bar S10_FILTERED_INDEL.vcf -o S10_FILTER_VARIANTS.vcf -genotypeMergeOptions PRIORITIZE -priority foo,bar
     ```
     
     Resultado
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-21-57-image.png)
2. *En materiales y métodos del reporte, indique el número de genes incluidos en el panel e incluya una tabla con la lista de genes (**consejo: revise el archivo regiones_blanco.bed**). Indique también la región genómica total (en pares de bases) cubierta por el panel, o sea, el tamaño de las regiones blanco (**consejo: revise su reporte qualimapReport.html**).*
   
   Para obtener la lista completa de genes en el archivo `regiones_blanco.bed` se utilizó el siguiente código:
   
   ```
   cut -f4 regiones_blanco.bed | sort | uniq > genes_panel.txt
   ```
   
   Para contar el número total de anotaciones se utilizó el siguiente código:
   
   ```
   wc -l genes_panel.txt
   ```
   
   Respuesta: Se encontraron 369 anotaciones en el panel `genes_panel.txt`.
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-34-12-image.png)
   
   La tabla se encuentre en *Materiales y métodos* del reporte técnico generado: [Reporte llamado de variantes](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%203/Tarea%203.1/Reportes/Reporte%20llamado%20de%20variantes.md)
3. *Realice el filtrado de variantes con dos filtros, DP<10 y uno adicional que usted proponga.*
   
   En el punto 1, se realizó el filtrado con el filtro DP<10.
   
   Se escogió como filtro adicional QD (que tendrá como nombre `filterqd`), significa Qualitiy score over depth e indica la calidad de la profundidad. Se filtrarán aquellos QD<2.0:
   
   ```
   java -jar /opt/GenomeAnalysisTK-3.7-0/GenomeAnalysisTK.jar -T VariantFiltration -R /datos/reference/genomes/hg19_reference/hg19.fasta -V S10_RAW_SNP.vcf --filterName "filter1" --filterExpression "(DP <10)" --filterName "filterqd" --filterExpression  "(QD <2.0)" -o S10_FILTERED3_SNP.vcf
   ```
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-47-07-image.png)
4. *Estime cuántas variantes son eliminadas por el filtro DP<10 solamente, y cuántas por ambos filtros.*
   
   **Filtro DP<10**: Se utilizó el siguiente código:
   
   ```
   grep "PASS" S10_FILTERED_SNP.vcf | grep -c "^[^#]"
   ```
   
   Respuesta: 76 variantes son eliminadas solamente por el filtro DP<10.
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-41-35-image.png)
   
   **Ambos filtros**: Se utilizó el siguiente código:
   
   ```
   grep "PASS" S10_FILTERED3_SNP.vcf | grep -c "^[^#]"
   ```
   
   Respuesta: 73 variantes son eliminadas solamente por ambos filtros.
   
   Resultado
   
   ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-18-50-50-image.png)
5. *Genere un reporte e incluya una tabla con el número de variantes detectadas totales, SNPs, e INDELs. Para cada caso, indicar el número de variantes filtradas y que pasaron los filtros (solo uno, y ambos).*
   * Conteo de variantes totales, SNPs e INDELs: Se utilizó el siguiente código:
     
     ```
     #Variantes totales:
     grep -v "^#" S10_RAW_SNP.vcf | wc -l
     
     #SNP's:
     grep -v "^#" S10_FILTERED_SNP.vcf | grep -v "PASS" | wc -l
     
     #INDEL's:
     grep -v "^#" S10_FILTERED_INDEL.vcf | grep -v "PASS" | wc -l
     ```
     
     Variantes totales: 83.
     
     SNP's: 7.
     
     INDEL's: 2.
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-19-26-07-image.png)
   
   * Conteo de variantes filtradas: Se utilizó el siguiente código:
     
     ```
     # Variantes con FILTER= "FILTER" (DP<10)
     grep -v "^#" S10_FILTERED_SNP.vcf | grep "FILTER" | wc -l
     
     # Variantes con FILTER= "filterqd" (QD<2.0)
     grep -v "^#" S10_FILTERED3_SNP.vcf | grep "filterqd" | wc -l
     ```
     
     Variantes con FILTER= "FILTER" (DP<10): 7.
     
     Variantes con FILTER= "filterqd" (QD<2.0): 3.
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-19-33-53-image.png)
   
   * Variantes que pasaron solo un filtro: Se utilizó el siguiente código:
     
     ```
     grep -v "^#" S10_FILTERED_SNP.vcf | cut -f1,2,4,5 | \
       grep -vxF -f <(grep -v "^#" S10_FILTERED3_SNP.vcf | cut -f1,2,4,5) | \
       wc -l
     ```
     
     Variantes que pasan "FILTER" pero fallan "filterqd": 0.
     
     ![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-19-45-22-image.png)
     
     La tabla se encuentra en *Resultados* del reporte técnico generado: [Reporte llamado de variantes](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%203/Tarea%203.1/Reportes/Reporte%20llamado%20de%20variantes.md)
6. *Visualice una variante en IGV, mostrando tracks tanto para el alineamiento (bam) como las variantes detectadas (VCF).*
7. *Asegúrese de usar un tamaño de ventana que muestre suficiente detalle como para leer la secuencia de referencia, pero sin un zoom excesivo para que se logre ver algo de contexto de secuencia. Ojalá que se vean otras variantes al rededor de la central. Incluya un track con los genes. Si no se ve ningún gen cercano a la variante, elija otra variante.*
8. *En resultados, indique en formato de tabla el número de variantes detectadas según ubicación (intrónica, río arriba, río abajo, codificante con cambio de sentido, sin sentido, etc).*
9. *Realice una anotación de las variantes con la herramienta en línea [VEP](https://grch37.ensembl.org/info/docs/tools/vep/index.html). Asegúrese de usar la versión del genoma que utilizó en el alineamiento. Incluya anotaciones de Significancia clínica y puntajes CADD. Baje la tabla de variantes anotadas en formato TXT y fíltrela (por ejemplo en R) para generar una tabla que solo contenga variantes con un valor distinto a "benign" en la columna "CLIN_SIG" o un valor de CAAD > 20. Incluya incluya la tabla filtrada en su informe (si hubo variantes que pasaron los filtros) e interprete sus resultados.*
10. *En la sección conclusiones, asegúrese de concluir algo sobre la muestra (presencia o no de mutaciones con potencial patogénico)*.
    
    ***Los items del 6 al 10 se encuentran resueltos en el reporte técnico generado:*** [Reporte llamado de variantes](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%203/Tarea%203.1/Reportes/Reporte%20llamado%20de%20variantes.md)
    
    ### Conclusiones
    
    Se documenta el flujo de filtrado de variantes en la muestra S10 (panel secuenciado en MiSeq alineado a hg19) mediante GATK 3.7; se define un panel de genes a ser aplicado en el llamado de variantes y se contabilizan variantes crudas y filtradas al aplicar los siguientes filtros DP < 10 y QD < 2.0.
    
    En conjunto con la información generada en el reporte técnico, es posible decir que la en muestra S10 existen variantes con potencial patogénico en el desarrollo de neoplasias hematológicas y tumores sólidos.
