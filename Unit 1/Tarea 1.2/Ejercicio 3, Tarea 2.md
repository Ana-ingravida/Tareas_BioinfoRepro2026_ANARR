# Tarea 1.2

## Ejercicio 3

*Mira el siguiente script ([tomado del manual de Stacks](http://catchenlab.life.illinois.edu/stacks/manual/#phand)) y contesta lo siguiente:*

1. ¿Cuántos pasos tiene este script?

2. ¿Si quisieras correr este script y que funcionara en tu propio equipo, qué línea deberías cambiar y a qué?

3. ¿A qué equivale `$HOME`?

4. ¿Qué paso del análisis hace el programa `gsnap`?

5. ¿Qué hace en términos generales cada uno de los loops.

**Autora: Ana Ramírez**

```
#!/bin/bash 

src=$HOME/research/project 

files=”sample_01 
sample_02 
sample_03” 

#
# Align with GSnap and convert to BAM
# 
for file in $files
do
    gsnap -t 36 -n 1 -m 5 -i 2 --min-coverage=0.90 \
            -A sam -d gac_gen_broads1_e64 \
            -D ~/research/gsnap/gac_gen_broads1_e64 \
            $src/samples/${file}.fq > $src/aligned/${file}.sam
    samtools view -b -S -o $src/aligned/${file}.bam $src/aligned/${file}.sam 
    rm $src/aligned/${file}.sam 
done

#
# Run Stacks on the gsnap data; the i variable will be our ID for each sample we process.
# 
i=1 
for file in $files 
do 
    pstacks -p 36 -t bam -m 3 -i $i \
              -f $src/aligned/${file}.bam \
              -o $src/stacks/ 
    let "i+=1"; 
done 

# 
# Use a loop to create a list of files to supply to cstacks.
# 
samp="" 
for file in $files 
do 
    samp+="-s $src/stacks/$file "; 
done 

# 
# Build the catalog; the "&>>" will capture all output and append it to the Log file.
# 
cstacks -g -p 36 -b 1 -n 1 -o $src/stacks $samp &>> $src/stacks/Log 

for file in $files 
do 
    sstacks -g -p 36 -b 1 -c $src/stacks/batch_1 \
             -s $src/stacks/${file} \ 
             -o $src/stacks/ &>> $src/stacks/Log 
done 

#
# Calculate population statistics and export several output files.
# 
populations -t 36 -b 1 -P $src/stacks/ -M $src/popmap \
              -p 9 -f p_value -k -r 0.75 -s --structure --phylip --genepop
```

1- El script tiene 5 pasos, los cuales corresponden a: *Align with GSnap and convert to BAM*, *Run Stacks on the gsnap data; the i variable will be our ID for each sample we process*,  *Use a loop to create a list of files to supply to cstacks*, *Build the catalog; the "&>>" will capture all output and append it to the Log file*, *Calculate population statistics and export several output files*.

2- Para correr este script en mi propio equipo es necesario cambiar la línea: `src=$HOME/research/project` pues en esa línea de código se encuentra un directorio y archivo que no esta presente en mi computadora, por lo que debería cambiarlo por una línea de código que involucre mi directorio: `PS C:\Users\cosit\`

3-  `$HOME` equivale al directorio personal del usuario actual.

4- `gsnap` (*Genomic Short-read Nucleotide Alignment Program*) es un programa bioinformático que permite mapear y alinear lecturas de secuenciación de ácidos nucléicos a un genoma de referencia. En el script `gsnap` permite que a partir de un archivo de lecturas de una muestra, este sea alineado contra un genoma de referencia utilizando ciertos parámetros.

5- El primer loop entrega instrucciones para transformar un archivo **SAM** en archivo **BAM**, el resto entrega instrucciones para interpretar el archivo y extraer la información relevante para el estudio.
