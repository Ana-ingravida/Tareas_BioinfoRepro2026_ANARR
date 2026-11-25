# Reporte llamado de variantes

**Fecha: 18 de noviembre de 2026**

**Autora: Ana Ramírez Rivas**

### Introducción

La secuencia paired-ended (S10_sorted_RG.bam) fue filtrada según ciertos parámetros de calidad y alineada (mapeo con BWA-MEM) con el genoma de referencia **hg19**. Seguido a ello, se realizó un llamado de variantes.

### Materiales y métodos

Pueden ser revisados en los siguientes enlaces:

[Filtrado](https://github.com/Ana-ingravida/Tareas_BioinfoRepro2026_ANARR/blob/main/Unit%202/Tarea%202.1.md)

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

**Tabla 1:** Listado de genes incluidos en el panel. Para el análisis de variantes se consideró el siguiente panel que abarca 26 genes (369 anotaciones) asociados tanto a neoplasias hematológicas como a tumores sólidos.

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

![](C:\Users\cosit\AppData\Roaming\marktext\images\2026-11-24-23-52-10-image.png)

*Figura 1*: chr17 41223094, SNP rs 1799966, T>C. Gen *BRCA1*.

### Conclusiones

La variante en chr17:41,223,094 es exónica y se encuentra río abajo (hacia el extremo 3') respecto al inicio de transcripción de BRCA1.


