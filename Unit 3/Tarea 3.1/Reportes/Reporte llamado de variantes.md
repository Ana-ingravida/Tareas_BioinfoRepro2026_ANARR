# Reporte llamado de variantes

**Fecha: 18 de noviembre de 2026**

**Autora: Ana Ramírez Rivas**

### Introducción

La secuencia paired-ended (S10_sorted_RG.bam) fue filtrada según ciertos parámetros de calidad y alineada (mapeo con BWA-MEM) con el genoma de referencia **hg19**. Seguido a ello, se realizó un llamado de variantes.

### Materiales y métodos

Pueden ser revisados en los siguientes enlaces:

[Filtrado](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%202/Tarea%202.1/README.md)

[Alineamiento]()

**Listado de genes**

| Gen    | Cromosoma |
| ------ | --------- |
| ABL1   | chr9      |
| BRAF   | chr7      |
| BRCA1  | chr17     |
| BRCA2  | chr13     |
| CALR   | chr19     |
| CBL    | chr11     |
| CEBPA  | chr19     |
| CRLF2  | chrX      |
| EZH2   | chr7      |
| FLT3   | chr13     |
| IKZF1  | chr7      |
| IL7    | chr8      |
| JAK2   | chr9      |
| JAK3   | chr19     |
| KIT    | chr4      |
| KRAS   | chr12     |
| MLL    | chr11     |
| MPL    | chr1      |
| P2RY8  | chrX      |
| PAX5   | chr9      |
| PDGFRA | chr4      |
| PDGFRB | chr5      |
| PTEN   | chr10     |
| RB1    | chr13     |
| SF3B1  | chr2      |
| TP53   | chr17     |
| WT1    | chr11     |

**Tabla 1:** Listado de genes incluidos en el panel. Para el análisis de variantes se consideró el siguiente panel que abarca 27 genes (369 anotaciones) asociados tanto a neoplasias hematológicas como a tumores sólidos.

### Resultados

Se realizó el llamado de variantes con dos filtros (DP<10 y QD<2.0), con esta información se generó la siguiente tabla:

| Categoría                            | SNP's | INDEL's | Total |
| ------------------------------------ | ----- | ------- | ----- |
| Variantes detectadas (RAW)           | 7     | 2       | 83    |
| Variantes con FILTER= (DP<10)        | -     | -       | 7     |
| Variantes con FILTER= (QD<2.0)       | -     | -       | 3     |
| Variantes que pasaron solo un filtro | 0     | 0       | 0     |
| Variantes que pasaron ambos filtros  | -     | -       | 10    |

**Tabla 2**: Contiene el número de variantes detectadas totales, SNP's, e INDEL's.

Visualice una variante en IGV, mostrando tracks tanto para el alineamiento (bam) como las variantes detectadas (VCF).

1. Se ingresa a la plataforma [IGV web](https://igv.org/app)

2. Selecciono cromosoma 17: **Genome → Human → hg19**

3. Cargo archivos BAM: **Tracks → Local File… → S10_sorted_RG.bam** y **S10_sorted_RG.bam.bai**.

4. Cargo archivo VCF filtrado: **Tracks → Local File…** → **S10_FILTERED3_SNP.vcf**

5. Busco una ventana en que exista una variante:

<img width="1359" height="468" alt="IGV" src="https://github.com/user-attachments/assets/aacdca51-b22a-4272-b091-06abe7d0e196" />

*Figura 1*: Visualización de una variante en el gen *BRCA1*, coordenadas chr17: 41,223,094-41,223,094, tipo de variante SNP, ID del SNP rs1799966,  sustitución T>C.

Al disminuir un poco el zoom (coordenadas chr17:41,218,901-41,223,959) se llegan a visualizar 3 variantes para el gen *BRCA1*:

<img width="1155" height="407" alt="IGV2" src="https://github.com/user-attachments/assets/874d71cb-9f4a-4d20-9886-d1c6f99e1563" />

*Figura 2*: Visualización de tres variantes (SNP's) en el gen *BRCA1*. En orden de izquierda a derecha: ID del SNP rs8176234, ID del SNP rs8176233, ID del SNP rs 1799966.

| Gen     | ID del SNP (rs) | Posición                     | Ubicación                                | Cambio                             |
| ------- | --------------- | ---------------------------- | ---------------------------------------- | ---------------------------------- |
| *BRCA1* | rs8176234       | chr17: 41,219,780-41,219,780 | Intrónica                                | sustitución T>C                    |
| *BRCA1* | rs8176233       | chr17: 41,219,804-41,219,804 | Intrónica                                | sustitución T>C                    |
| *BRCA1* | rs1799966       | chr17: 41,223,094-41,223,094 | Exónica, río abajo (hacia el extremo 3') | sustitución T>C (missense_variant) |

**Tabla 3**: Contiene las especificaciones de las variantes encontradas en la ventana.

Anotación de las variantes con la herramienta en línea [ensembl](https://www.ensembl.org/).

- Se ingresa a la plataforma de ensembl.

- Ir a la sección ENSEMBL VEP.

- Selecciono Genoma → HumanGRCh37.p13.

- Cargo archivos VCF: **Variants** → **S10_FILTERED3_SNP.vcf**.

- Run.

<img width="1355" height="572" alt="ensembl" src="https://github.com/user-attachments/assets/d8763aa8-7e54-4e0d-8a03-b9b427618770" />

*Figura 3*: Visualización de la anotación de las variantes con la herramienta en línea [ensembl](https://www.ensembl.org/).

Luego de utilizar la herramienta ensembl, se descargó la tabla de variantes anotadas en formato TSV para fíltrarla, con el fin de generar una tabla que solo contenga variantes con un valor distinto a "benign" en la columna "CLIN_SIG" o un valor de CAAD > 20. Debido a que la tabla inicial no contenia las columnas de significancia clínica ni valor de CAAD, se realizó el filtrado en base a "Consequence", "IMPACT" y "Existing_variation" en R.

Script:

```
#Set working directory -> C:/Users/cosit/Desktop/Bioinformatica/Tarea_3.1"

#Llamar librerias
library(readr)
library(dplyr)
library(tidyverse)

#Leer el archivo
VEP <- read_tsv("S10_VEP.tsv.gz")
VEP

tabla_consecuencias <- VEP %>%
  count(IMPACT, Consequence, sort = TRUE)

print(tabla_consecuencias, n = Inf)

VEP_filtrado <- VEP %>%
  filter(
    IMPACT %in% c("HIGH", "MODERATE"),
    !str_detect(Consequence, "synonymous_variant|intron_variant|upstream_gene_variant|downstream_gene_variant")
  )

# Ver tabla resultante
VEP_filtrado %>%
  select(Uploaded_variation, SYMBOL, Consequence, IMPACT, HGVSc, HGVSp)
```

Resultado:

<img width="848" height="550" alt="Tabla Vep 1" src="https://github.com/user-attachments/assets/67271ad0-79bb-4c38-9673-0b1ba78db743" />

<img width="849" height="546" alt="Tabla Vep 2" src="https://github.com/user-attachments/assets/20a812d5-076d-458e-bf2e-73ece338f31c" />

<img width="850" height="188" alt="Tabla Vep 3" src="https://github.com/user-attachments/assets/1520aa41-653e-4ca6-9bf9-83a109a9bb19" />

*Figura 4*: Tabla generada al aplicar los filtros descritos previamente al archivo generado con la herramienta ensembl. 59 variantes pasaron el filtro, todas son variantes de tipo missense y de impacto moderado a excepción de la variante n°9 que es de tipo pérdida del inicio y de impacto alto.

### Conclusiones

El presente reporte contempla el llamado y anotación de variantes para la muestra S10_sorted_RG.bam alineada contra hg19 (GRCh37.p13) sobre un panel de genes asociados a neoplasias hematológicas y tumores sólidos (369 anotaciones; 27 genes). Se aplicaron filtros de calidad (DP<10 y QD<2.0) para filtrar las variantes. 

Mediante la plataforma IGV se observaron variantes en el gen *BRCA1*  (identificando una variante de tipo missense y dos variantes intrónicas). 

Al utilizar la herramienta Ensembl VEP se anotaron las variantes de la muestra S10. Al filtrar en R por IMPACT (HIGH/MODERATE) y excluir consecuencias de bajo impacto o no codificantes ante la ausencia de columnas de significancia clínica o CADD en la salida inicial, se retuvieron 59 variantes, predominando las de tipo missense de impacto moderado y destacando una variante de alto impacto por posible pérdida del codón de inicio (start_lost). 

En base a todo lo anterior es posible decir que la muestra S10 existen variantes con potencial patogénico en el desarrollo de neoplasias hematológicas y tumores sólidos.


