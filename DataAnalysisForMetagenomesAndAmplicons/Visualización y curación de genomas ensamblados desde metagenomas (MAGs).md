<div class="book without-animation with-summary font-size-2 font-family-1" data-basepath=".">

<div class="book-summary">

  - [Visualización y curación de
    MAGs](http://castrolab.org/isme/mags/mags.html)

  - 
  - [****0.1** Acerca del curso](#workshop)

  - [****1** MAGs (*Metagenome-assembled
    genomes*)](#mags-metagenome-assembled-genomes)

  - [****2** Datos de entrada](#datos-de-entrada)

  - [****3** *Binning*](#binning)

  - [****4** Visualización de *bins*](#visualizacion-de-bins)
    
      - [****4.1** Instalar y cargar las paquetes de
        R](#instalar-y-cargar-las-paquetes-de-r)
      - [****4.2** Cargar archivos de
        entrada](#cargar-archivos-de-entrada)
          - [****4.2.1** Obteniendo el contenido
            GC](#obteniendo-el-contenido-gc)
          - [****4.2.2** Obteniendo la
            cobertura](#obteniendo-la-cobertura)
          - [****4.2.3** Obteniendo la información de los
            *bins*](#obteniendo-la-informacion-de-los-bins)
          - [****4.2.4** Visualización de los
            *bins*](#visualizacion-de-los-bins)
          - [****4.2.5** Mejoramiento de los
            *bins*](#mejoramiento-de-los-bins)
          - [****4.2.6** Agregando información
            extra](#agregando-informacion-extra)
      - [****4.3** Obteniendo información
        taxonómica](#obteniendo-informacion-taxonomica)
      - [****4.4** Visualizar grupos
        taxonómicos](#visualizar-grupos-taxonomicos)
      - [****4.5** Usando marcadores para mejorar los
        *bins*](#usando-marcadores-para-mejorar-los-bins)

  - [****5** Abundancia de *bins* y comunidades
    microbianas](#abundancia-de-bins-y-comunidades-microbianas)

  - 
  - [CASTRO LAB](http://www.castrolab.org)

  - [ISME LA 2019](https://isme-la2019.org)

</div>

<div class="book-body">

<div class="body-inner">

<div class="book-header" role="navigation">

# **[Visualización y curación de genomas ensamblados desde metagenomas (MAGs)](./)

</div>

<div class="page-wrapper" tabindex="-1" role="main">

<div class="page-inner">

<div id="section-" class="section normal">

<div id="header">

*ISME Latin America, Septiembre 2019*

</div>

<div id="workshop" class="section level2">

## <span class="header-section-number">0.1</span> Acerca del curso

El tutorial a continuación fue creado especialmente para guiar el
trabajo práctico del [**curso pre-congreso ISME Latin
America 2019**](https://isme-la2019.org/curso-pre-congreso/): **Análisis
de datos bioinformáticos para metagenomas y amplicones usando R**. A
realizarse el próximo 9 y 10 de septiembre en la [Universidad Técnica
Federico Santa María](https://www.usm.cl), Valparaíso, Chile.

![](images/back.png) [**Ir a la página de inicio del
curso**](http://castrolab.org/isme/bienvenida_WorkshopISME.html)

El curso cuenta con **6 unidades**. Ahora, usted se encuentra en la
unidad: **Visualización y curación de genomas ensamblados desde
metagenomas (MAGs)** ![](images/genome.png)

> **Otras unidades del curso son**:
> 
> [**Introducción a R: Manipulación de datos y
> visualización**](http://www.castrolab.org/isme/introR/introR.html)
> ![](images/r.png)
> 
> [**Análisis de secuencias de 16S con
> DADA2**](http://www.castrolab.org/isme/dada2/dada2.html)
> ![](images/bioinformatics.png)
> 
> [**Introducción a phyloseq y a análisis de
> diversidad**](http://www.castrolab.org/isme/biodiversity/biodiversity.html)
> ![](images/bacteria.png)
> 
> [**Búsqueda de genes de interés en datos de metagenómica
> *shotgun***](http://www.castrolab.org/isme/gene_search/gene_search.html)
> ![](images/gene_search.png)
> 
> [**Redes de co-ocurrencia de
> microorganismos**](http://www.castrolab.org/isme/microbial_networks/microbial_networks.html)
> ![](images/network.png)

-----

  - Profesor: **Dr. Eduardo Castro-Nallar** (<eduardo.castro@unab.cl>)

  - Ayudantes:
    
      - **Dr. Florence Gutzwiller** (<florence.gutzwiller@gmail.com>)
      - **M.Sc. Katterinne N. Mendez** (<mendez.katterinne@gmail.com>)

**[CASTRO LAB](http://www.castrolab.org)**

[Centro de Bioinformática y Biología Integrativa](http://www.cbib.cl) |
[Universidad Andrés Bello](https://www.unab.cl)

![](images/UNAB_CBIB_horizontal.png)

[Santiago, Chile](https://goo.gl/maps/BCe9hWcRZJKkhaDP8)

</div>

<div id="mags-metagenome-assembled-genomes" class="section level1">

# <span class="header-section-number">1</span> MAGs (*Metagenome-assembled genomes*)

La extracción de genomas microbianos directamente desde metagenomas
*shotgun* tiene muchas ventajas porque no depende de ninguna base de
datos, lo que permite la identificación de nuevos microorganismos. Sin
embargo, encontrar *reads* que vengan del mismo genoma en un metagenoma
complejo es complicado, debido a que la información es limitada con
respecto a qué características utilizar para separar *reads*.  
Para identificar genomas en metagenomas, varios programas han sido
desarrollados. Estos programas agrupan *contigs* de características
similares en *bins* (a partir de *contigs* que vienen de un ensamblaje
*de novo*). Los programas de *binning* en general aprovechan dos tipos
de información: **cobertura** (*coverage*) y **composición**.

**Cobertura** se refiere a cuántas *reads* mapean en contra de un
*contig* dado y es la característica más intuituva de entender porque
organismos diferentes deberían tener coberturas diferentes. *Contigs*
que vienen de los mismos microorganismos deberían tener más o menos la
misma abundancia. Naturalmente, existen limitaciones intrínsecas debido
a que la cobertura, y en consecuencia la abundancia de cada *contig*,
está sesgada por la secuenciación.

La **composición** se basa en el hecho de que la composición
nucleotídica de un microorganismo, el porcentaje de cada nucleótido (y
sus combinaciones) en el genoma es similar intra especie y difiere entre
especies. El porcentaje de GC en bacterias puede variar de menos del 20%
hasta más del 70%. Podemos evaluar composición de diferentes maneras: el
porcentaje GC, frecuencia de di-, tri- o tetra-nucleótidos, etc. La
frecuencia de tetrámeros, por ejemplo, evaluaría las veces que se
detectan combinaciones de cuatro nucleótidos en un contig o genoma. Una
limitación de este método es que aunque la composición es relativamente
estable en promedio en un genoma, esta también varía localmente lo que
junto a *contigs* de variadas longitudes pueden llegar a confundir a un
algortimo de *binning*.

Algunos programas utilizan una de estas características para separar
genomas de metagenomes, sin embargo programas más modernos utilizan
ambas para tratar de agrupar *contigs* de manera correcta y recuperar
genomas de alta calidad.

**La calidad de los metagenomas al igual que la profundidad tiene que
ser alta**. Además, la longitud de los *contigs* tiene un efecto
importante debido a que las características de **cobertura y composición
pueden ser mejor estimadas a partir de *contigs* largos**, y así
aproximar los valores para la especie microbiana en cuestión.

</div>

<div id="datos-de-entrada" class="section level1">

# <span class="header-section-number">2</span> Datos de entrada

En esta unidad vamos a usar datos preliminares de **comunidades
microbianas de la rizósfera de plantas Antárticas**. Un primer análisis
de estos datos ya ha sido publicado donde puedes encontrar más detalles
sobre las muestras y sobre cómo descargar el *raw data*.

> Molina-Montenegro, M.A., Ballesteros, G.I., Castro-Nallar, E.,
> Meneses, C., Gallardo-Cerda, J. and Torres-Díaz, C., 2019. [A first
> insight into the structure and function of rhizosphere microbiota in
> Antarctic plants using shotgun
> metagenomics](https://link.springer.com/article/10.1007/s00300-019-02556-7).
> Polar Biology, pp.1-11.

![](images/_DSC3922.jpg)

Solamente dos plantas vasculares nativas existen en la Antártica:
*Colobanthus quitensis* y *Deschampsia antarctica*. En ambas plantas se
ha demostrado que organismos asociados a la raíz tienen un impacto sobre
su habilidad para tolerar condiciones extremas, tales como baja
disponibilidad de agua y alta salinidad.

Ambas plantas ocurren en zonas que en verano están libres de hielo y
nieve, principalmente en la Península Antártica, sin embargo su
distribución no es idéntica. *D. antarctica* principalmente crece sola
en áreas de alta exposición al viento y al *spray* del océano, mientras
que *C. quitensis* aparentemente favorece lugares mejor protegidos donde
pueda tener acceso a agua dulce. Sin embargo, *C. quitensis* puede
también ser encontrada en lugares más expuestos siempre y cuando crezca
en asociación con *D. antarctica* (ver foto). Esto sugiere que las
raíces de *D. antarctica* podrían permitir *C. quitensis* tolerar de
mejor manera lugares expuestos.

<div class="figure">

![association](images/association.jpg)

association

</div>

Entonces, para estudiar la composición y estructura de las comunidades
microbianas de *C. quitensis* y *C. quitensis* en asociación con *D.
antarctica*, secuenciamos ADN de la rizósfera en ambos casos, con tres
réplicas para cada caso. En este tutorial, vamos a utilizar *contigs*
de cada muestra que ya han sido previamente agrupados en *bins*. Las
*reads* usadas para el ensamblaje fueron obtenidas a partir de
secuenciación en la plataforma Illumina MiSeq. En la siguiente sección
se describen los pasos realizados para procesar el *raw data* hasta
llegar a los datos de entrada de este tutorial.

</div>

<div id="binning" class="section level1">

# <span class="header-section-number">3</span> *Binning*

Como en todos los proyectos de secuenciación, el primer paso fue
realizar un control de calidad de las *reads*, cortar adaptadores, y
remover secuencias de baja calidad usando **PRINSEQ-lite** (version
0.20.4).

Configuramos los parámetros de acuerdo al largo de las reads y su
calidad: longitud mínima de 50 bases (`-min_len 50`), calidad promedio
mínima de 20 (`-min_qual_mean 20`), y una calidad promedio en ventanas
de 50 bp de 20 (`-trim_qual_window 20`; `-trim_qual_step 5`). Filtramos
*reads* con bases indeterminadas (`-ns_max_p 0`), y removemos solamente
duplicados exactos presentes dos o más veces (`-derep 1 -derep_min 2`),
ya que esperamos que la variabilidad sea alta y secuencias duplicadas
podrían ser artefactos. También removemos secuencias de baja complejidad
con un puntaje de 32 o menos (`-lc_threshold 32`). Finalmente, removemos
5 nucleótidos en el extremo 5’ y 20 en el 3’.

<div class="sourceCode">

``` sourceCode bash
for i in $list_sample
do
prinseq-lite.pl -verbose -fastq $rawdir/${i}_1.fastq -fastq2 $rawdir/${i}_2.fastq -min_len 50 -min_qual_mean 20 -ns_max_p 0 -derep 1 -derep_min 2 -lc_method dust -lc_threshold 32 -trim_left 5 -trim_right 20 -trim_qual_right 20 -trim_qual_window 20 -trim_qual_step 5 -out_format 3 -out_good $cleaneddir/${i} -out_bad null >> $cleaneddir/prinseq_${i}.log
done
```

</div>

Una vez limpias, las reads son concatenadas, *forwards* con *forwards* y
lo mismo con las *reverse* para así hacer un único co-ensamblaje usando
**metaSPAdes** (version v3.12.0). Las opciones `-t` y `-m` permiten
especificar el numero de hilos y la memoria disponible, respectivamente.

<div class="sourceCode">

``` sourceCode bash
metaspades.py -1 $filtereddir/concat_samples_1.fastq -2 $filtereddir/concat_samples_2.fastq -s $filtereddir/concat_samples_singletons.fastq -t 80 -m 1000 -o $assemblydir/concat_samples
```

</div>

**MEGAHIT** también puede ser usado para el ensamblaje, especialmente
con conjuntos de datos más grandes porque es menos demandante de memoria
RAM.

Una vez que el ensamblaje está listo, mapeamos de vuelta las reads en
contra de los contigs de cada muestra usando **Bowtie2** (version
2.3.4.2). De esta manera, para cada muestra tenemos un archivo de
alineamiento: BAM.

<div class="sourceCode">

``` sourceCode bash
  for i in $samples
do
bowtie2 -p 20 -x $assemblydir/contigs -1 $cleaneddir/${i}_1.fastq -2 $cleaneddir/${i}_2.fastq -U $cleaneddir/${i}_singleton.fastq |  samtools view -bh | samtools sort -o $bamdir/${i}.bam
done
```

</div>

Finalmente, teniendo los contigs ensamblados y los BAMs individuales de
cada muestra podemos comenzar con el *binning*. Para esto, usamos dos
programas, **MaxBin** (version 2.2.4) y **MetaBat** (version 2.12.1).
Ambos requieren una longitud de contig mayor a 1 kb. El output de ambos
programas se obtiene en formato FASTA, lo cual usaremos para las
visualizaciones más adelante en el tutorial.

MaxBin calcula la abundancia usando las *reads* para cada muestra (en
**reads\_list.txt**) y el ensamblaje.

<div class="sourceCode">

``` sourceCode bash
  for i in $samples
do
run_MaxBin.pl -verbose -thread 32 -min_contig_length 1000 -contig $assemblydir/contigs.fasta -out $bindir/maxbin_6samples -reads_list reads_list.txt
done
```

</div>

MetaBat toma como *input* los archivos BAM para calcular la abundancia
primero, y luego usa el *output* para el *binning*.

<div class="sourceCode">

``` sourceCode bash
  for i in $samples
do
jgi_summarize_bam_contig_depths --outputDepth $bamdir/depth.txt  $bamdir/*.bam
metabat2 -i $assemblydir/contigs.fasta -a $bamdir/depth.txt -o $bindir/metabat -t 20 
done
```

</div>

Finalmente, para poder agregar información taxonómica a cada contig para
fines de evaluación, necesitamos hacer una identificación con marcadores
genéticos. Para hacer esto usamos **AMPHORA2**, el cual encuentra genes
marcadores y utiliza un enfoque filogenético para asignarle taxonomía a
cada marcador.

<div class="sourceCode">

``` sourceCode bash
  for i in $samples
do
cd $amphora_dir

perl /home/user/AMPHORA2/Scripts/MarkerScanner.pl -Bacteria -DNA $contigs
perl /home/user/AMPHORA2/Scripts/MarkerAlignTrim.pl -WithReference -OutputFormat phylip
perl /home/user/AMPHORA2/Scripts/Phylotyping.pl -CPUs 2 > phylotype.result

done
```

</div>

Una vez que todo esto está listo, procedemos con la evaluación y
visualización de *contigs* y *bins*.

</div>

<div id="visualizacion-de-bins" class="section level1">

# <span class="header-section-number">4</span> Visualización de *bins*

Para visualizar la cohesión de los *bins*, usaremos las características
que hablamos más arriba que definen a los *bins*: **cobertura y
composición**. Comenzamos graficando las posiciones de cada *contig*
basándonos en cobertura y composición, y finalmente visualizamos los
diferentes *bins*.

La cobertura se expresa como número de reads mapeads por cada *contig* y
la composición según el porcentaje de GC de cada *contig*.

<div id="instalar-y-cargar-las-paquetes-de-r" class="section level2">

## <span class="header-section-number">4.1</span> Instalar y cargar las paquetes de R

Para este tutorial usaremos los paquetes `seqinr` para cargar los
archivos FASTA y obtener el contenido GC, y `plyr` para manipular las
tablas resultantes.

  - Primero instalamos los paquetes:

<div class="sourceCode">

``` sourceCode r
install.packages("seqinr")
install.packages("plyr")
install.packages("gplots")
```

</div>

  - Luego cargamos los paquetes:

<div class="sourceCode">

``` sourceCode r
library(seqinr)
library(plyr)
library(gplots)
```

</div>

</div>

<div id="cargar-archivos-de-entrada" class="section level2">

## <span class="header-section-number">4.2</span> Cargar archivos de entrada

Ahora que los paquetes están cargados, podemos cargar los datos de
entrada. Necesitas **descargar y descomprimir** el directorio de datos
desde este
[**link**](https://www.dropbox.com/s/vbbdrnswhwetgw1/mags_input_files.zip?dl=0)
y definir la ruta de directorios hasta los archivos de entrada.

<div class="sourceCode">

``` sourceCode r
base_path <- "path/to/base/"
```

</div>

<div class="sourceCode">

``` sourceCode r
base_path <- "./"
```

</div>

Primero que todo, definimos todos los directorios en los cuales los
datos están ubicados:

<div class="sourceCode">

``` sourceCode r
contig_directory <- paste( base_path, "input_files/assembly/", sep="")
stat_directoy <- paste( base_path, "input_files/bam/", sep="")
maxbin_directory <- paste( base_path, "input_files/bins/maxbin/", sep="")
metabat_directory <- paste( base_path, "input_files/bins/metabat/", sep="")
amphora_directory <- paste( base_path, "input_files/amphora/", sep="")
```

</div>

Luego, necesitamos la información de **cobertura y contenido GC para
cada *contig***.

<div id="obteniendo-el-contenido-gc" class="section level3">

### <span class="header-section-number">4.2.1</span> Obteniendo el contenido GC

El contenido GC puede ser calculado en R usando el paquete `seqinr` para
cada *contig*. Para hacer esto, necesitamos leer los contigs en R usando
la función `read.fasta`. Entonces cargamos los *contigs* a la sesión
actual de R para computar el GC. Usaremos solo contigs más largos que 1
kb.

  - Definimos la ruta a los *contigs* en formato FASTA:

<div class="sourceCode">

``` sourceCode r
contig_path <- paste(contig_directory , "contigs_1000.fasta", sep="")
contig_path
```

</div>

    ## [1] "~/Dropbox/CastroLab_database/workshops_data/mags_input_files/assembly/contigs_1000.fasta"

  - Ahora, cargamos el FASTA en un objeto llamado `contigs`:

<div class="sourceCode">

``` sourceCode r
contigs <- read.fasta(file = contig_path)
```

</div>

Ahora todos los contigs están en el objeto *contigs*. Necesitamos crear
una tabla que muestre el nombre del *contig* y su correspondiente
contenido GC. Para esto primero creamos una tabla vacía de las
dimensiones necesarias, y luego computamos el contenido GC *contig* por
*contig*. La tabla necesita dos columnas, una para el nombre del contig
y otra para el contenido GC, y tantas filas como *contigs*.

  - Vamos a calcular la longitud de un vector que contiene los nombres
    de los *contigs* y almacenaremos ese número en un objeto. Con esto
    creamos la tabla:

<div class="sourceCode">

``` sourceCode r
contig_len <- length(names(contigs))
gc_table <- as.data.frame(matrix(ncol=2, nrow=contig_len))
colnames(gc_table) <- c("contig","gc_content")
```

</div>

Para computar el contenido GC *contig* por *contig* y agregarlo a la
tabla, necesitamos usar un bucle (*loop*) de tipo for que leerá todas
las secuencias. De nuevo, usamos el objeto que contiene el número de
contigs (*contig\_len*) para determinar cuántas veces el bucle se
ejecutará:

<div class="sourceCode">

``` sourceCode r
for (i in c(1:contig_len))
{
gc_table[i,1] <- names(contigs)[i]
gc_table[i,2] <- GC(contigs[[i]])
}
```

</div>

Ahora tenemos una tabla con contenido GC para cada *contig*. Nos falta
la cobertura.

</div>

<div id="obteniendo-la-cobertura" class="section level3">

### <span class="header-section-number">4.2.2</span> Obteniendo la cobertura

Para obtener los valores de cobertura, necesitamos usar información de
`samtools` `idxstats` que está en el directorio llamado
`stat_directoy/`.

  - Primero, ya que hay un archivo por muestra, necesitamos obtener una
    lista con todos los archivos a leer:

<div class="sourceCode">

``` sourceCode r
stat_list <- list.files(stat_directoy, pattern = "\\.idxstat$")
stat_list
```

</div>

    ## [1] "G1-1.idxstat" "G1-2.idxstat" "G1-3.idxstat" "G2-1.idxstat"
    ## [5] "G2-2.idxstat" "G2-3.idxstat"

El archivo `.idzstat` viene en forma de tabla sin encabezado y contiene
los nombres de los *contigs*, la longitud del *contig*, y el número de
*reads* mapeadas y no mapeadas. El primer paso es leer la tabla y darle
nombres a las columnas. Luego, necesitamos normalizar el número de
*reads* mapeadas por la longitud del *contig* y así obtener el valor de
cobertura. Esto es porque *contigs* más largos van a tener más *reads*
mapeadas que *contigs* más cortos pero podrían tener la misma
abundancia.

  - Una vez que tenemos esto, hacemos una tabla que contenga el nombre
    del *contig* y el número de reads normalizado para cada muestra. De
    nuevo, usamos un bucle for para iterar sobre todos los archivos
    `.idxstat`:

<div class="sourceCode">

``` sourceCode r
for (i in c(1:length(stat_list)))
{
sample_stat <- read.table(paste(stat_directoy,stat_list[i], sep=""), header = F)
colnames(sample_stat) <- c("contig","length","mapped","unmapped")
sample_stat$count <- sample_stat$mapped / sample_stat$length
colnames(sample_stat)[5] <- strsplit(stat_list[i], "\\.")[[1]][1]
if (i == 1)
 {
  table_stat <- sample_stat[,c(1,5)]
 }
else
 {
  table_stat <- merge(table_stat, sample_stat[,c(1,5)])
 }
}
```

</div>

  - Revisemos que la tabla sea lo que buscábamos:

<div class="sourceCode">

``` sourceCode r
str(table_stat)
```

</div>

    ## 'data.frame':    1365347 obs. of  7 variables:
    ##  $ contig: Factor w/ 1365347 levels "*","NODE_1_length_200697_cov_10.033577",..: 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ G1-1  : num  NaN 0.032761 0.031195 0.032399 0.000398 ...
    ##  $ G1-2  : num  NaN 0.03925 0.03598 0.03947 0.00053 ...
    ##  $ G1-3  : num  NaN 0.02878 0.027853 0.029398 0.000928 ...
    ##  $ G2-1  : num  NaN 9.97e-06 2.57e-05 3.57e-05 1.87e-02 ...
    ##  $ G2-2  : num  NaN 1.49e-05 5.14e-05 0.00 3.14e-02 ...
    ##  $ G2-3  : num  NaN 0 0 0 0.0172 ...

Como se puede ver, los nombres de las columnas de las muestras contienen
un `-`, que normalmente no son bien manejados por R, ya que cuando los
mencionamos en la consola R los interpreta como una sustracción.
Entonces necesitamos un paso adicional para poder reemplazar ese
caracter por algo que R maneje mejor, por ejemplo reempalzando los `-`
por `_`.

<div class="sourceCode">

``` sourceCode r
colnames(table_stat) <- gsub("-","_",colnames(table_stat))
```

</div>

Finalmente, sería ideal una tabla que contenga información de la tabla
de contenido GC, la tabla de cuentas o número de *reads*, y el número
total de *reads*.

  - Usando lo que aprendimos en la unidad [Introducción R, manipulación
    y visualización en
    R](http://www.castrolab.org/isme/introR/introR.html), intentemos
    crear esta tabla en un objeto llamado `table_all` que tenga una
    columna para el número total de *reads* llamado `all`.

  - Una vez que esto está OK, usamos `str()` para verificar que la tabla
    resultante es lo que esperábamos.

<div class="sourceCode">

``` sourceCode r
str(table_all)
```

</div>

    ## 'data.frame':    70180 obs. of  9 variables:
    ##  $ contig    : chr  "NODE_1_length_200697_cov_10.033577" "NODE_10_length_77801_cov_9.055193" "NODE_100_length_27995_cov_9.999749" "NODE_1000_length_7547_cov_5.887213" ...
    ##  $ gc_content: num  0.533 0.564 0.543 0.666 0.664 ...
    ##  $ G1_1      : num  0.032761 0.031195 0.032399 0.000398 0 ...
    ##  $ G1_2      : num  0.03925 0.03598 0.03947 0.00053 0 ...
    ##  $ G1_3      : num  0.02878 0.027853 0.029398 0.000928 0 ...
    ##  $ G2_1      : num  9.97e-06 2.57e-05 3.57e-05 1.87e-02 1.38e-02 ...
    ##  $ G2_2      : num  1.49e-05 5.14e-05 0.00 3.14e-02 1.51e-02 ...
    ##  $ G2_3      : num  0 0 0 0.0172 0.0088 ...
    ##  $ all       : num  0.1008 0.0951 0.1013 0.0692 0.0377 ...

Ahora tenemos toda la información para hacer el gráfico base que muestre
cada *contig* en función de su contenido GC y su cobertura.

  - Miremos el gráfico base.

<div class="sourceCode">

``` sourceCode r
plot(table_all$gc_content, table_all$all, pch=20, xlab="GC content", ylab="abundance")
```

</div>

![](mags_files/figure-html/unnamed-chunk-24-1.png)

Debido a la distribución de *reads*/cuentas, necesitamos hacer una
transformación para observar las diferencias en cobertura.

  - Utilizamos entonces una escala logarítmica para una mejor
    visualización:

<div class="sourceCode">

``` sourceCode r
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
```

</div>

![](mags_files/figure-html/unnamed-chunk-25-1.png)

Para visualizar los *bins*, vamos a colorear todos los contigs según el
*bin* al cuál correspondan. Para ser capaz de hacer esto, necesitamos
una tabla que contenga los nombres de los *contigs* y el *bin* al cual
están asociados en otra columna. Como utilizamos dos métodos de
*binning*, necesitaremos una columna para cada método.

</div>

<div id="obteniendo-la-informacion-de-los-bins" class="section level3">

### <span class="header-section-number">4.2.3</span> Obteniendo la información de los *bins*

Ya que los *bins* están en formato FASTA, los podemos usar para extraer
los nombres de los *contigs* para cada *bin*. Para esto, necesitamos
leer los archivos en R y guardarlos en un objeto, y guardar solamente
las líneas que corresponden a los nombres de los *contigs*.

Vamos a usar la funcción `readLines()` para leer el archivo y la función
`grep(pattern, object)` para seleccionar las líneas deseadas, basándose
en un patrón. La función va a retornar el número de líneas a
seleccionar, lo cual usaremos para seleccionar un subconjunto de líneas.

  - Comencemos con los bins de `MaxBin`:

<div class="sourceCode">

``` sourceCode r
bin <- readLines(paste(maxbin_directory, "maxbin_6samples.001.fasta", sep=""))
pattern="^>"
contig_nb <- grep(pattern, bin)
bin_contigs <- bin[contig_nb]
bin_contigs <- gsub(">","",bin_contigs)
assign("maxbin_6samples.001.fasta", bin_contigs)
```

</div>

De esta forma, deberíamos obtener un objeto con el nombre del *bin*
`maxbin_6samples.001.fasta`, que contiene una lista con todos los
*contigs* que incluye.

> **Basándose en lo que hicimos en la sección previa, modifica el código
> de tal forma que lo puedas correr para todos los *bins* en el
> directorio de `maxbin`.**

> **Una vez que lo has hecho para el directorio `maxbin`, haz lo mismo
> para el de `metabat`.**

Ahora estamos un paso más cerca para fabricar la tabla que necesitamos,
con los nombres de los *contigs* y su nombre de *bin* correspondiente en
dos columnas, una para cada programa.

  - Para hacer la tabla, primero creamos una tabla vacía, después
    agregamos los nombres de los *contigs*, y finalmente para cada
    *bin*, agregamos el nombre del *bin*.

<div class="sourceCode">

``` sourceCode r
contig_to_bin <- as.data.frame(matrix(ncol = 3, nrow = contig_len))
colnames(contig_to_bin) <- c("contig","maxbin","metabat")
contig_to_bin$contig <- names(contigs)

maxbin_list <- list.files(maxbin_directory)
metabat_list <- list.files(metabat_directory)

for (i in c(1:length(maxbin_list)))
{
bin <- get(maxbin_list[i])
contig_to_bin$maxbin[contig_to_bin$contig %in% bin] <- maxbin_list[i]
}

for (i in c(1:length(metabat_list)))
{
bin <- get(metabat_list[i])
contig_to_bin$metabat[contig_to_bin$contig %in% bin] <- metabat_list[i]
}

str(contig_to_bin)
```

</div>

    ## 'data.frame':    70180 obs. of  3 variables:
    ##  $ contig : chr  "NODE_1_length_200697_cov_10.033577" "NODE_2_length_102735_cov_9.657090" "NODE_3_length_99345_cov_9.186706" "NODE_4_length_95814_cov_9.273468" ...
    ##  $ maxbin : chr  "maxbin_6samples.002.fasta" "maxbin_6samples.045.fasta" "maxbin_6samples.045.fasta" "maxbin_6samples.045.fasta" ...
    ##  $ metabat: chr  "metabat.18.fa" "metabat.21.fa" "metabat.21.fa" "metabat.21.fa" ...

Debido a que esta tabla contiene 70180, no es muy útil explorar las
similitudes entre *bins* creados por distintos programas.

  - Antes de comenzar con los pasos de visualización, vamos a crear otra
    tabla que nos va a mostrar la cantidad de *contigs* que se
    superponen entre ambos programas:

<div class="sourceCode">

``` sourceCode r
overlap_table <- contig_to_bin[,c(2,3)]
overlap_table <- ddply(overlap_table,.(maxbin,metabat),nrow)
colnames(overlap_table)[3] <- "contigs_nb"

head(overlap_table)
```

</div>

    ##                      maxbin       metabat contigs_nb
    ## 1 maxbin_6samples.001.fasta metabat.12.fa         19
    ## 2 maxbin_6samples.001.fasta metabat.13.fa         34
    ## 3 maxbin_6samples.001.fasta metabat.19.fa        465
    ## 4 maxbin_6samples.001.fasta          <NA>        562
    ## 5 maxbin_6samples.002.fasta metabat.11.fa         38
    ## 6 maxbin_6samples.002.fasta metabat.18.fa         83

</div>

<div id="visualizacion-de-los-bins" class="section level3">

### <span class="header-section-number">4.2.4</span> Visualización de los *bins*

  - Vamos a comenzar con una visualización simple de los *bins*
    individuales en el gráfico principal:

<div class="sourceCode">

``` sourceCode r
bin <- contig_to_bin$contig[contig_to_bin$maxbin %in% maxbin_list[1]]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
points(table_all$gc_content[table_all$contig %in% bin ], log(table_all$all[table_all$contig %in% bin ]), col="yellow", pch=20)
legend( x="bottomleft", 
        legend=c("maxbin 1"), 
        col=c("yellow"), 
        pch=20, merge=FALSE )
```

</div>

![](mags_files/figure-html/unnamed-chunk-31-1.png)

<div class="sourceCode">

``` sourceCode r
bin <- contig_to_bin$contig[contig_to_bin$metabat %in% metabat_list[1]]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
points(table_all$gc_content[table_all$contig %in% bin ], log(table_all$all[table_all$contig %in% bin ]), col="yellow", pch=20)
legend( x="bottomleft", 
        legend=c("metabat 1"), 
        col=c("yellow"), 
        pch=20, merge=FALSE )
```

</div>

![](mags_files/figure-html/unnamed-chunk-32-1.png)

> Puedes explorar los *bins* cambiando el nombre del *bin* según la
> lista con nombres. También se pueden cambiar los colores.

</div>

<div id="mejoramiento-de-los-bins" class="section level3">

### <span class="header-section-number">4.2.5</span> Mejoramiento de los *bins*

  - Vamos a comenzar con el primer *bin* de MaxBin
    `maxbin_6samples.001.fasta`.

<div class="sourceCode">

``` sourceCode r
bin <- contig_to_bin$contig[contig_to_bin$maxbin %in% maxbin_list[1]]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
points(table_all$gc_content[table_all$contig %in% bin ], log(table_all$all[table_all$contig %in% bin ]), col="yellow", pch=20)
legend( x="bottomleft", 
        legend=c("maxbin_6samples.001.fasta"), 
        col=c("yellow"), 
        pch=20, merge=FALSE )
```

</div>

![](mags_files/figure-html/unnamed-chunk-33-1.png)

Como podemos ver, graficamos la distribución de *contigs* basándose en
el contenido GC y en la cobertura y vemos que la mayoría de los
*contigs* forman grupos compactos mientras que otros tienen bajo
contenido GC y alta cobertura. Ya que esos *contigs* parecen *outliers*
con respecto al resto, podrían potencialmente no pertenecer al *bin*.
Vamos a crear un objeto que contenga solamente esos *contigs* para
seguir examinándolos.

  - Vamos a seleccionar los *contigs* que pertenecen al *bin*
    `maxbin_6samples.001.fasta` y que tienen un contenido GC de menos de
    0.6. Además agreguémos color a esos contigs para poder observarlo
    mejor:

<div class="sourceCode">

``` sourceCode r
contigs_low_gc <- table_all[table_all$contig %in% maxbin_6samples.001.fasta & table_all$gc_content < 0.6,1]

bin <- contig_to_bin$contig[contig_to_bin$maxbin %in% maxbin_list[1]]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
points(table_all$gc_content[table_all$contig %in% bin ], log(table_all$all[table_all$contig %in% bin ]), col="yellow", pch=20)
points(table_all$gc_content[table_all$contig %in% contigs_low_gc ], log(table_all$all[table_all$contig %in% contigs_low_gc ]), col="blue", pch=20)
legend( x="bottomleft", 
        legend=c("maxbin_6samples.001.fasta", "outliers (maxbin_6samples.001.fasta)"), 
        col=c("yellow","blue"), 
        pch=20, merge=FALSE )
```

</div>

![](mags_files/figure-html/unnamed-chunk-34-1.png)

  - Ahora, como podemos ver en la tabla `overlap_table`, el otro
    programa generó un *bin* que incluye muchos de los mismos *contigs*
    que `maxbin_6samples.001.fasta`, el cuál es `metabat.19.fa`:

<div class="sourceCode">

``` sourceCode r
overlap_table[overlap_table$maxbin %in% "maxbin_6samples.001.fasta",]
```

</div>

    ##                      maxbin       metabat contigs_nb
    ## 1 maxbin_6samples.001.fasta metabat.12.fa         19
    ## 2 maxbin_6samples.001.fasta metabat.13.fa         34
    ## 3 maxbin_6samples.001.fasta metabat.19.fa        465
    ## 4 maxbin_6samples.001.fasta          <NA>        562

  - Veamos cómo ambos *bins* se superponen:

<div class="sourceCode">

``` sourceCode r
bin1 <- contig_to_bin$contig[contig_to_bin$maxbin %in% "maxbin_6samples.001.fasta"]
bin2 <- contig_to_bin$contig[contig_to_bin$metabat %in% "metabat.19.fa"]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")

points(table_all$gc_content[table_all$contig %in% bin1 ], log(table_all$all[table_all$contig %in% bin1 ]), col="yellow", pch=20)

points(table_all$gc_content[table_all$contig %in% bin2 ], log(table_all$all[table_all$contig %in% bin2 ]), col="red", pch=20)
legend( x="bottomleft", 
        legend=c("maxbin_6samples.001.fasta", "metabat.19.fa"), 
        col=c("yellow","red"), 
        pch=20, merge=FALSE )
```

</div>

![](mags_files/figure-html/unnamed-chunk-36-1.png)

Podemos ver que el *bin* de MetaBat parece más compacto que el de
MaxBin. Sin embargo, hay información que no estamos usando aún en esta
evaluación. La abundancia o cobertura proviene de la suma de seis
muestras independientes. Esto significa que hay más información que es
muestra-específica que no se ve por la manera en que hemos graficado.
Esta información podría ser relevante para el proceso de *binning*.

  - Para revelar esto, dividamos la información en nuestro gráfico en
    seis gráficos independientes:

<div class="sourceCode">

``` sourceCode r
par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)
points(table_all[table_all$contig %in% bin1 ,2], log(table_all[table_all$contig %in% bin1,i]), col="yellow", pch=20)
points(table_all[table_all$contig %in% bin2 ,2], log(table_all[table_all$contig %in% bin2,i]), col="red", pch=20)
legend( x="bottomleft", 
        legend=c("maxbin_6samples.001.fasta", "metabat.19.fa"), 
        col=c("yellow","red"), 
        pch=20, merge=FALSE, cex = 1.3 )
}
```

</div>

![](images/plot_1.png)

Podemos ver algo nuevo aquí. Aunque los *bins* se ven mayoritariamente
compactos en el primer gráfico, en estos nuevos gráficos vemos que las
formas cambian drásticamente. Esto es porque las variaciones en
abundancia y/o en cobertura se enmascaran cuando se suman las
abundancias de otras muestras, especialmente cuando graficamos en escala
logarítmica.

En el bin `metabat.19.fa` (en rojo), podemos ver que en las primeras
tres muestras algunos *contigs* parecen tener más baja cobertura que los
demás, mientras que en las tres muestras de abajo el *bin* aparece
dividido en *contigs* de alta y baja cobertura.

  - Vamos a comenzar por enfocarnos en los *contigs* con baja cobertura
    en las primeras muestras. Tal como hicimos antes, vamos a generar un
    objeto con esos *contigs* y vamos a usar un color para destacarlos:

<div class="sourceCode">

``` sourceCode r
contigs_low_cov <- table_all[table_all$contig %in% metabat.19.fa & log(table_all$G1_1) < -4 ,1]

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]) , cex.lab=1.5, cex.main=2)
points(table_all[table_all$contig %in% bin1 ,2], log(table_all[table_all$contig %in% bin1,i]), col="yellow", pch=20)
points(table_all[table_all$contig %in% bin2 ,2], log(table_all[table_all$contig %in% bin2,i]), col="red", pch=20)
points(table_all[table_all$contig %in% contigs_low_gc ,2], log(table_all[table_all$contig %in% contigs_low_gc ,i]), col="blue", pch=20)
points(table_all[table_all$contig %in%  contigs_low_cov ,2], log(table_all[table_all$contig %in% contigs_low_cov ,i]), col="green", pch=20)
legend( x="bottomleft", 
        legend=c("maxbin_6samples.001.fasta", "outliers (maxbin_6samples.001.fasta)","metabat.19.fa","outliers (metabat.19.fa)"), 
        col=c("yellow","blue", "red", "green"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_2.png)

Ahora podemos ver que los *contigs* de baja cobertura en las primeras
tres muestras son los mismos que tienen más abundancias en las tres
últimas muestras. Como se comportan tan direfentemente al resto de los
contigs, es más seguro removerlos del *bin* para evitar contaminación.

Este pequeño ejercicio muestra la importancia de considerar la cobertura
por muestra para tomar decisiones respecto a un *bin*. Ahora, vamos a
remover los *outliers* azules y verdes, y combinar el resto en un nuevo
y más cohesionado *bin*:

<div class="sourceCode">

``` sourceCode r
remove <- unique(c(contigs_low_gc,contigs_low_cov))
new_bin <- unique(c(bin1,bin2))
new_bin <- new_bin[! new_bin %in% remove]

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)
points(table_all[table_all$contig %in% new_bin ,2], log(table_all[table_all$contig %in% new_bin,i]), col="yellow", pch=20)

legend( x="bottomleft", 
        legend=c("new_bin"), 
        col=c("yellow"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_3.png)

Ahora que hemos mejorado nuestro primer *bin*, echemos un vistazo a otro
*bin*: `maxbin_6samples.002.fasta`

> **Revisa la tabla que muestra la superposición de *contigs* entre
> programas para ver con qué otro *bin* comparte *contigs*.**

> **Genera un gráfico que muestre ambos *bins* coloreados, con un
> gráfico para cada muestra.**

![](images/plot_4.png)

Podemos ver que el primer *bin*, `maxbin_6samples.002.fasta`, parece
dividido en dos, un *bin* arriba que se superpone con el segundo *bin*,
`metabat.18.fa`, y otro *bin*. Podemos también ver que muy pocos
*contigs* aparecen en las tres últimas muestras, lo que sugiere que la
abundancia de estos *contigs* es cercana a cero.

  - Basándonos en las superposiciones de *bins* y en la separación entre
    los dos *clusters* de *contigs*, vamos a separar el primer y segundo
    *bin* para estudiarlos más en detalle e individualmente.

<div class="sourceCode">

``` sourceCode r
new_bin2 <- table_all[table_all$contig %in% maxbin_6samples.002.fasta & log(table_all$G1_1) < -4 ,1]
new_bin3 <- maxbin_6samples.002.fasta[! maxbin_6samples.002.fasta %in% new_bin2]

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)
points(table_all[table_all$contig %in% new_bin2 ,2], log(table_all[table_all$contig %in% new_bin2,i]), col="dodgerblue", pch=20)
points(table_all[table_all$contig %in% new_bin3 ,2], log(table_all[table_all$contig %in% new_bin3,i]), col="cadetblue1", pch=20)

points(table_all[table_all$contig %in% bin4 ,2], log(table_all[table_all$contig %in% bin4,i]), col="darkorchid1", pch=20)

legend( x="bottomleft", 
        legend=c("new_bin2", "new_bin3","metabat.18.fa"), 
        col=c("dodgerblue","cadetblue1","darkorchid1"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_5.png)

  - Ahora podemos unir `metabat.18.fa` con `new_bin3`:

<div class="sourceCode">

``` sourceCode r
new_bin3 <- unique(c(new_bin3,metabat.18.fa))

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)
points(table_all[table_all$contig %in% new_bin2 ,2], log(table_all[table_all$contig %in% new_bin2,i]), col="dodgerblue", pch=20)
points(table_all[table_all$contig %in% new_bin3 ,2], log(table_all[table_all$contig %in% new_bin3,i]), col="darkorchid1", pch=20)
legend( x="bottomleft", 
        legend=c("new_bin2", "new_bin3"), 
        col=c("dodgerblue","darkorchid1"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_6.png)

Ahora, necesitamos determinar qué haremos con los *contigs* que tienen
una abundancia distinta de cero en las tres últimas muestras.

  - Comencemos por crear un objeto con esos *contigs* en al menos una de
    las tres últimas muestras, para luego destacarlos con un color
    diferente:

<div class="sourceCode">

``` sourceCode r
contigs_non_zero <- table_all[log(table_all$G2_1) > -Inf | log(table_all$G2_2) > -Inf | log(table_all$G2_3) > -Inf  ,1]

contigs_non_zero_2 <- contigs_non_zero[contigs_non_zero %in% new_bin2]
contigs_non_zero_3 <- contigs_non_zero[contigs_non_zero %in% new_bin3]

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)
points(table_all[table_all$contig %in% new_bin2 ,2], log(table_all[table_all$contig %in% new_bin2,i]), col="dodgerblue", pch=20)
points(table_all[table_all$contig %in% new_bin3 ,2], log(table_all[table_all$contig %in% new_bin3,i]), col="darkorchid1", pch=20)
points(table_all[table_all$contig %in% contigs_non_zero_2 ,2], log(table_all[table_all$contig %in% contigs_non_zero_2,i]), col="green", pch=20)
points(table_all[table_all$contig %in% contigs_non_zero_3 ,2], log(table_all[table_all$contig %in% contigs_non_zero_3,i]), col="red", pch=20)

legend( x="bottomleft", 
        legend=c("new_bin2", "new_bin3", "non_zero (new_bin2)", "non_zero (new_bin3)"), 
        col=c("dodgerblue","darkorchid1", "green","red"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_7.png)

Como se aprecia en el gráfico, los *contigs* con abundancia distinta a
cero en las tres últimas muestras están dentro del grupo de *contigs*
como se ve en las otras muestras.

En este tipo de situaciones, uno tiene que decidir si es más conservador
y dejar la menor cantidad de potenciales contaminantes (con el riesgo de
obtener un genoma incompleto), o tratar de obtener el genoma más
completo pero corriendo el riesgo de tomar algún *contig* que no
corresponda a ese genoma. Sin embargo, debemos tener en mente que estos
datos son preliminares, con baja cobertura. Además, los *bins*
seleccionados para explorar deben ser los de mejor calidad, con alta
completitud de genoma y virtualmente sin contaminación. Finalmente,
recuperar genomas de metagenomas o MAGs no es una tarea sencilla ni
automática, por lo tanto la calidad de los datos, la profundidad de
secuenciación, y la longitud de los contigs son factores muy relevantes.

</div>

<div id="agregando-informacion-extra" class="section level3">

### <span class="header-section-number">4.2.6</span> Agregando información extra

Aunque a veces trabajamos con MAGs para no tener que usar bases de
datos, estas bases de datos todavía nos pueden ser de ayuda para evaluar
la calidad de los *bins*, mejorar su composición, y agregar información
taxonómica si es posible.

Para poder usar este tipo de información, ejecutamos previamente el
programa `AMPHORA2`, un programa que busca marcadores filogenéticos de
procariontes para inferir taxonomía. Los resultados de este programa nos
ayudarán a evaluar si contigs con la misma información taxonómica
pertenecen al mismo *bin*. Esto complementa lo que hemos aprendido hasta
ahora con la visualización de cobertura y contenido GC.

</div>

</div>

<div id="obteniendo-informacion-taxonomica" class="section level2">

## <span class="header-section-number">4.3</span> Obteniendo información taxonómica

Vamos a leer los resultados de `AMPHORA2` desde su directorio. Estos
resultados están almacenados en una tabla con encabezado. Si comparamos
los nombres de los *contigs* de la tabla de `AMPHORA2` con las otras
tablas, vamos a ver que `AMPHORA2` agrega un número a cada entrada
porque puede haber más de un marcador por *contig*. \* Para poder usar
estos resultados, necesitamos hacer coincidir los nombres de los
*contigs* entre las tablas, removiendo lo que agrega `AMPHORA2`.

<div class="sourceCode">

``` sourceCode r
amphora_results <- read.table( paste(amphora_directory, "phylotype.result", sep=""), header = T , sep="\t" , fill=T)
amphora_results[,1] <- as.character(amphora_results[,1])
amphora_results[,1] <- gsub("(.*)_.*","\\1",amphora_results[,1])
```

</div>

  - Para confirmar si hay más de un marcador por *contig*, necesitamos
    comparar la longitud de la columna *contigs* en la tabla
    `amphora_results` en contra de la longitud de los elementos únicos
    en la misma columna:

<div class="sourceCode">

``` sourceCode r
length(amphora_results$Query)
```

</div>

    ## [1] 1524

<div class="sourceCode">

``` sourceCode r
length(unique(amphora_results$Query))
```

</div>

    ## [1] 1039

Vemos que las longitudes difieren, lo que indica que algunos contigs
tienen más de un marcador con más de una identificación. Sin embargo,
queremos una identificación por *contig* así que necesitamos resolver
esas instancias de *contigs* con más de un marcador.

  - Vamos a tomar cada uno de esos *contigs* desde la tabla
    `amphora_results` y vamos a transferir la identificación a otra
    tabla. Hay tres posibilidades: el *contig* tiene un único marcador
    por lo que se transfiere una sola identificación; el *contig* tiene
    más de un marcador pero todos los marcadores indican la misma
    taxonomía por lo que colapsamos las filas en la nueva tabla; y
    finalmente, los diferentes marcadores indican diferente taxonomía,
    en cual caso nos quedamos con el nivel taxonómico donde coinciden y
    agregamos NAs donde corresponda. Vamos a eliminar los valores de
    porcentaje para evitar errores.

<div class="sourceCode">

``` sourceCode r
amphora_names <- amphora_results
for (i in c(3:9))
{
amphora_names[,i] <- as.character(amphora_names[,i])
amphora_names[,i] <- vapply(strsplit(amphora_names[,i], "(", fixed = TRUE), "[", "", 1)
}

unique_contigs <- unique(amphora_names$Query)
unique_list <- vector()
  
table_markers_resolved <- as.data.frame(matrix(ncol=9, nrow=length(unique_contigs)))
colnames(table_markers_resolved) <- colnames(amphora_names)
for (i in c(1:length(unique_contigs)))
{
contig <- unique_contigs[i]
if(length(amphora_names[amphora_names$Query %in% contig,1]) == 1 )
 {
 table_markers_resolved[i,] <- amphora_names[amphora_names$Query %in% contig,]
 } else
 {
 tmp_table <- amphora_names[amphora_names$Query %in% contig,]
 table_markers_resolved$Query[i] <- contig
 table_markers_resolved$Marker[i] <- "multiple"
 for (j in c(3:9))
  {
   if ( length(unique(tmp_table[,j])) == 1)
   {
    table_markers_resolved[i,j] <- unique(tmp_table[,j])
   } else
   {
    table_markers_resolved[i,j] <- NA
   }
  }
 }
}
```

</div>

  - Ahora deberíamos haber obtenido una tabla con una entrada por
    *contig*. De nuevo, revisemos que esto sea así al comparar la
    longitud de las tablas:

<div class="sourceCode">

``` sourceCode r
length(table_markers_resolved$Query)
```

</div>

    ## [1] 1039

<div class="sourceCode">

``` sourceCode r
length(unique(table_markers_resolved$Query))
```

</div>

    ## [1] 1039

Ahora tenemos una tabla que muestra algunos *contigs* con su respectiva
identificación. Ahora generemos una tabla que tenga las identificaciones
taxonómicas pero por *bin*.

</div>

<div id="visualizar-grupos-taxonomicos" class="section level2">

## <span class="header-section-number">4.4</span> Visualizar grupos taxonómicos

De la misma forma en que visualizamos los *bins*, coloreando los
*contigs* que pertenecían a cierto *bin*, vamos a hacer los mismo pero
con los grupos taxonómicos.

  - Primero, vamos a crear un objeto con los nombres de los *contigs*
    asociados a un grupo taxonómico, para luego colorearlos en el
    gráfico base:

<div class="sourceCode">

``` sourceCode r
fam_verru <- table_markers_resolved$Query[which(table_markers_resolved$Family == "Verrucomicrobiaceae")]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
points(table_all$gc_content[table_all$contig %in% fam_verru ], log(table_all$all[table_all$contig %in% fam_verru ]), col="yellow", pch=20)
```

</div>

![](mags_files/figure-html/unnamed-chunk-55-1.png)

<div class="sourceCode">

``` sourceCode r
fam_conexi <- table_markers_resolved$Query[which(table_markers_resolved$Family == "Conexibacteraceae")]
plot(table_all$gc_content, log(table_all$all), pch=20, xlab="GC content", ylab="abundance (log)")
points(table_all$gc_content[table_all$contig %in% fam_conexi ], log(table_all$all[table_all$contig %in% fam_conexi ]), col="yellow", pch=20)
```

</div>

![](mags_files/figure-html/unnamed-chunk-55-2.png)

Podemos ver que mientras la mayoría de los *contigs* se agrupan en la
misma área del gráfico, estos no forman una agruoación tan compacta como
en el caso de los *bins*. Esto es esperado ya que una baja cantidad de
*contigs* contiene información taxonómica. Muchos *contigs* no tenían
ninguna clase de identificación taxonómica.

> **De esta misma manera, visualiza otros grupos taxonómicos, por
> ejemplo género o especie.**

</div>

<div id="usando-marcadores-para-mejorar-los-bins" class="section level2">

## <span class="header-section-number">4.5</span> Usando marcadores para mejorar los *bins*

Ahora, si visualizamos los *bins* y los marcadores al mismo tiempo,
podemos usar la información extra para tomar una desición con respecto a
cómo vamos a manejar los *bins*.

  - Como siempre, primero creamos una tabla que combine la información
    de los *bins* y de los marcadores:

<div class="sourceCode">

``` sourceCode r
bin_markers_table <- merge(contig_to_bin[,c(1:3)] , table_markers_resolved, by.x = "contig", by.y="Query")
```

</div>

  - Comencemos con el *bin* `maxbin_6samples.033.fasta`. Primero veamos
    si alguno de sus *contigs* tiene información taxonómica:

<div class="sourceCode">

``` sourceCode r
bin_markers_table[which(bin_markers_table$maxbin == "maxbin_6samples.033.fasta" ),]
```

</div>

    ##                                  contig                    maxbin
    ## 23  NODE_10864_length_2294_cov_1.762394 maxbin_6samples.033.fasta
    ## 48  NODE_12066_length_2182_cov_2.621533 maxbin_6samples.033.fasta
    ## 55  NODE_12125_length_2177_cov_2.284637 maxbin_6samples.033.fasta
    ## 74  NODE_12727_length_2125_cov_3.314976 maxbin_6samples.033.fasta
    ## 164 NODE_17079_length_1857_cov_1.734739 maxbin_6samples.033.fasta
    ## 206 NODE_18613_length_1784_cov_2.097166 maxbin_6samples.033.fasta
    ## 228 NODE_19672_length_1739_cov_2.425178 maxbin_6samples.033.fasta
    ## 249 NODE_20875_length_1694_cov_2.406345 maxbin_6samples.033.fasta
    ## 254 NODE_21149_length_1684_cov_2.442603 maxbin_6samples.033.fasta
    ## 388 NODE_30098_length_1437_cov_3.272069 maxbin_6samples.033.fasta
    ## 407 NODE_31205_length_1414_cov_2.531273 maxbin_6samples.033.fasta
    ## 411 NODE_31346_length_1411_cov_2.772124 maxbin_6samples.033.fasta
    ## 441 NODE_33183_length_1376_cov_2.800151 maxbin_6samples.033.fasta
    ## 442 NODE_33208_length_1376_cov_1.874338 maxbin_6samples.033.fasta
    ## 461 NODE_34434_length_1355_cov_2.738462 maxbin_6samples.033.fasta
    ## 501 NODE_36325_length_1325_cov_1.510236 maxbin_6samples.033.fasta
    ## 539 NODE_38482_length_1291_cov_1.766181 maxbin_6samples.033.fasta
    ## 566 NODE_39958_length_1270_cov_2.060082 maxbin_6samples.033.fasta
    ## 607 NODE_42441_length_1238_cov_1.911243 maxbin_6samples.033.fasta
    ## 641 NODE_44384_length_1214_cov_3.280414 maxbin_6samples.033.fasta
    ## 648 NODE_44978_length_1208_cov_1.472680 maxbin_6samples.033.fasta
    ## 669 NODE_46234_length_1193_cov_2.188049 maxbin_6samples.033.fasta
    ## 835 NODE_59282_length_1073_cov_1.355599 maxbin_6samples.033.fasta
    ## 873  NODE_6227_length_2980_cov_2.676581 maxbin_6samples.033.fasta
    ## 946 NODE_67665_length_1015_cov_1.400000 maxbin_6samples.033.fasta
    ## 967 NODE_69403_length_1004_cov_1.874605 maxbin_6samples.033.fasta
    ## 968 NODE_69483_length_1004_cov_1.237092 maxbin_6samples.033.fasta
    ## 992  NODE_7532_length_2725_cov_2.530712 maxbin_6samples.033.fasta
    ##           metabat   Marker Superkingdom          Phylum
    ## 23           <NA>        6     Bacteria Verrucomicrobia
    ## 48           <NA>       18     Bacteria Verrucomicrobia
    ## 55           <NA> multiple     Bacteria Verrucomicrobia
    ## 74           <NA>        3     Bacteria Verrucomicrobia
    ## 164          <NA> multiple     Bacteria Verrucomicrobia
    ## 206          <NA>        4     Bacteria Verrucomicrobia
    ## 228          <NA> multiple     Bacteria Verrucomicrobia
    ## 249          <NA>       13     Bacteria Verrucomicrobia
    ## 254          <NA>       28     Bacteria Verrucomicrobia
    ## 388          <NA> multiple     Bacteria  Proteobacteria
    ## 407          <NA>       27     Bacteria Verrucomicrobia
    ## 411          <NA> multiple     Bacteria Verrucomicrobia
    ## 441          <NA>       24     Bacteria Verrucomicrobia
    ## 442          <NA>        1     Bacteria Verrucomicrobia
    ## 461          <NA> multiple     Bacteria Verrucomicrobia
    ## 501          <NA> multiple     Bacteria Verrucomicrobia
    ## 539          <NA>       16     Bacteria  Proteobacteria
    ## 566          <NA> multiple     Bacteria            <NA>
    ## 607          <NA>       13     Bacteria Verrucomicrobia
    ## 641          <NA>       26     Bacteria Verrucomicrobia
    ## 648          <NA> multiple     Bacteria Verrucomicrobia
    ## 669          <NA>       29     Bacteria Verrucomicrobia
    ## 835          <NA>       31     Bacteria   Acidobacteria
    ## 873 metabat.15.fa        2     Bacteria Verrucomicrobia
    ## 946          <NA>       22     Bacteria   Acidobacteria
    ## 967          <NA>       12     Bacteria Verrucomicrobia
    ## 968          <NA>       31     Bacteria  Proteobacteria
    ## 992 metabat.15.fa        5     Bacteria Verrucomicrobia
    ##                   Class                   Order              Family
    ## 23     Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 48             Opitutae                    <NA>                <NA>
    ## 55     Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 74     Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 164                <NA>                    <NA>                <NA>
    ## 206    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 228                <NA>                    <NA>                <NA>
    ## 249 Methylacidiphilales    Methylacidiphilaceae   Methylacidiphilum
    ## 254    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 388  Betaproteobacteria                    <NA>                <NA>
    ## 407    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 411    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 441    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 442    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 461                <NA>                    <NA>                <NA>
    ## 501    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 539 Gammaproteobacteria         Xanthomonadales    Xanthomonadaceae
    ## 566                <NA>                    <NA>                <NA>
    ## 607 Methylacidiphilales    Methylacidiphilaceae   Methylacidiphilum
    ## 641            Opitutae                    <NA>                <NA>
    ## 648 Methylacidiphilales    Methylacidiphilaceae   Methylacidiphilum
    ## 669    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 835                <NA>                    <NA>                <NA>
    ## 873    Verrucomicrobiae      Verrucomicrobiales Verrucomicrobiaceae
    ## 946        Solibacteres          Solibacterales     Solibacteraceae
    ## 967            Opitutae                    <NA>                <NA>
    ## 968 Alphaproteobacteria Candidatus Pelagibacter                <NA>
    ## 992            Opitutae         Puniceicoccales    Puniceicoccaceae
    ##                            Genus                        Species
    ## 23                   Akkermansia        Akkermansia muciniphila
    ## 48                          <NA>                           <NA>
    ## 55                   Akkermansia        Akkermansia muciniphila
    ## 74                   Akkermansia        Akkermansia muciniphila
    ## 164                         <NA>                           <NA>
    ## 206                  Akkermansia        Akkermansia muciniphila
    ## 228                         <NA>                           <NA>
    ## 249 Methylacidiphilum infernorum                           <NA>
    ## 254                  Akkermansia        Akkermansia muciniphila
    ## 388                         <NA>                           <NA>
    ## 407                  Akkermansia        Akkermansia muciniphila
    ## 411                  Akkermansia        Akkermansia muciniphila
    ## 441                  Akkermansia        Akkermansia muciniphila
    ## 442                  Akkermansia        Akkermansia muciniphila
    ## 461                         <NA>                           <NA>
    ## 501                  Akkermansia        Akkermansia muciniphila
    ## 539            Pseudoxanthomonas   Pseudoxanthomonas suwonensis
    ## 566                         <NA>                           <NA>
    ## 607 Methylacidiphilum infernorum                           <NA>
    ## 641                         <NA>                           <NA>
    ## 648 Methylacidiphilum infernorum                           <NA>
    ## 669                  Akkermansia        Akkermansia muciniphila
    ## 835                         <NA>                           <NA>
    ## 873                  Akkermansia        Akkermansia muciniphila
    ## 946        Candidatus Solibacter Candidatus Solibacter usitatus
    ## 967                         <NA>                           <NA>
    ## 968                         <NA>                           <NA>
    ## 992             Coraliomargarita   Coraliomargarita akajimensis

Aparentemente la mayoría de los contigs apunta a la misma especie.

  - Grafiquemos el *bin* y la especie al mismo tiempo:

<div class="sourceCode">

``` sourceCode r
bin1 <- maxbin_6samples.033.fasta

sp <- "Akkermansia muciniphila"
cont_sp <- table_markers_resolved$Query[which(table_markers_resolved$Species %in% sp)]

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)

points(table_all[table_all$contig %in% bin1 ,2], log(table_all[table_all$contig %in% bin1 ,i]), col="yellow", pch=20)
points(table_all[table_all$contig %in% cont_sp ,2], log(table_all[table_all$contig %in% cont_sp ,i]), col=rgb(1, 0, 0, 0.6), pch=20)

legend( x="bottomleft", 
        legend=c("maxbin_6samples.033.fasta", "Akkermansia muciniphila"), 
        col=c("yellow","red"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_8.png)

Aunque la mayoría de los *contigs* que corresponden a *Akkermansia
muciniphila* se superponen con el *bin*, algunos caen afuera lo que
sugiere que el *bin* podría no contener todos los *contigs* de esa
especie.

  - Veamos si hay otros *bins* con esa identificación de especie:

<div class="sourceCode">

``` sourceCode r
bin_markers_table[which(bin_markers_table$Species == "Akkermansia muciniphila" ),]
```

</div>

    ##                                   contig                    maxbin
    ## 23   NODE_10864_length_2294_cov_1.762394 maxbin_6samples.033.fasta
    ## 32   NODE_11152_length_2266_cov_2.471280 maxbin_6samples.044.fasta
    ## 33   NODE_11335_length_2248_cov_1.796626 maxbin_6samples.034.fasta
    ## 55   NODE_12125_length_2177_cov_2.284637 maxbin_6samples.033.fasta
    ## 74   NODE_12727_length_2125_cov_3.314976 maxbin_6samples.033.fasta
    ## 103  NODE_13948_length_2036_cov_2.008582 maxbin_6samples.044.fasta
    ## 118  NODE_14634_length_1992_cov_2.247806 maxbin_6samples.026.fasta
    ## 133  NODE_15249_length_1956_cov_2.124671 maxbin_6samples.044.fasta
    ## 172  NODE_17534_length_1833_cov_1.943195 maxbin_6samples.034.fasta
    ## 187  NODE_17965_length_1812_cov_1.739328 maxbin_6samples.044.fasta
    ## 203   NODE_1850_length_5429_cov_2.622069 maxbin_6samples.044.fasta
    ## 206  NODE_18613_length_1784_cov_2.097166 maxbin_6samples.033.fasta
    ## 231  NODE_19860_length_1731_cov_2.409905 maxbin_6samples.044.fasta
    ## 252  NODE_21129_length_1685_cov_1.929448 maxbin_6samples.013.fasta
    ## 254  NODE_21149_length_1684_cov_2.442603 maxbin_6samples.033.fasta
    ## 356   NODE_2750_length_4433_cov_2.674966 maxbin_6samples.044.fasta
    ## 379   NODE_2940_length_4301_cov_2.014837 maxbin_6samples.044.fasta
    ## 392  NODE_30350_length_1432_cov_2.161220 maxbin_6samples.034.fasta
    ## 407  NODE_31205_length_1414_cov_2.531273 maxbin_6samples.033.fasta
    ## 411  NODE_31346_length_1411_cov_2.772124 maxbin_6samples.033.fasta
    ## 441  NODE_33183_length_1376_cov_2.800151 maxbin_6samples.033.fasta
    ## 442  NODE_33208_length_1376_cov_1.874338 maxbin_6samples.033.fasta
    ## 444  NODE_33470_length_1371_cov_2.566109 maxbin_6samples.044.fasta
    ## 455  NODE_34114_length_1361_cov_1.979326 maxbin_6samples.034.fasta
    ## 465  NODE_34826_length_1349_cov_2.642195 maxbin_6samples.044.fasta
    ## 501  NODE_36325_length_1325_cov_1.510236 maxbin_6samples.033.fasta
    ## 521  NODE_37241_length_1310_cov_1.548207 maxbin_6samples.034.fasta
    ## 555  NODE_39341_length_1279_cov_2.033497 maxbin_6samples.034.fasta
    ## 642  NODE_44504_length_1213_cov_2.187392 maxbin_6samples.044.fasta
    ## 669  NODE_46234_length_1193_cov_2.188049 maxbin_6samples.033.fasta
    ## 763  NODE_53350_length_1121_cov_3.098499 maxbin_6samples.034.fasta
    ## 780  NODE_54854_length_1108_cov_2.436847 maxbin_6samples.044.fasta
    ## 866  NODE_61580_length_1056_cov_2.140859 maxbin_6samples.034.fasta
    ## 873   NODE_6227_length_2980_cov_2.676581 maxbin_6samples.033.fasta
    ## 928  NODE_66576_length_1022_cov_1.018614 maxbin_6samples.026.fasta
    ## 1003  NODE_8161_length_2624_cov_2.351109 maxbin_6samples.044.fasta
    ## 1022  NODE_9125_length_2494_cov_2.653137 maxbin_6samples.044.fasta
    ##            metabat   Marker Superkingdom          Phylum            Class
    ## 23            <NA>        6     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 32            <NA>       20     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 33            <NA>       20     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 55            <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 74            <NA>        3     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 103           <NA>        4     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 118           <NA>       27     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 133           <NA>        1     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 172           <NA>        4     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 187           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 203  metabat.15.fa        2     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 206           <NA>        4     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 231           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 252           <NA>       14     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 254           <NA>       28     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 356  metabat.15.fa        1     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 379  metabat.15.fa multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 392           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 407           <NA>       27     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 411           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 441           <NA>       24     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 442           <NA>        1     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 444           <NA>        7     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 455           <NA>        3     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 465           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 501           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 521           <NA>       22     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 555           <NA>        1     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 642           <NA>        6     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 669           <NA>       29     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 763           <NA>        2     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 780           <NA> multiple     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 866           <NA>        2     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 873  metabat.15.fa        2     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 928           <NA>       23     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 1003 metabat.15.fa        6     Bacteria Verrucomicrobia Verrucomicrobiae
    ## 1022          <NA>       24     Bacteria Verrucomicrobia Verrucomicrobiae
    ##                   Order              Family       Genus
    ## 23   Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 32   Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 33   Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 55   Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 74   Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 103  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 118  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 133  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 172  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 187  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 203  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 206  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 231  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 252  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 254  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 356  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 379  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 392  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 407  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 411  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 441  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 442  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 444  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 455  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 465  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 501  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 521  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 555  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 642  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 669  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 763  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 780  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 866  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 873  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 928  Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 1003 Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ## 1022 Verrucomicrobiales Verrucomicrobiaceae Akkermansia
    ##                      Species
    ## 23   Akkermansia muciniphila
    ## 32   Akkermansia muciniphila
    ## 33   Akkermansia muciniphila
    ## 55   Akkermansia muciniphila
    ## 74   Akkermansia muciniphila
    ## 103  Akkermansia muciniphila
    ## 118  Akkermansia muciniphila
    ## 133  Akkermansia muciniphila
    ## 172  Akkermansia muciniphila
    ## 187  Akkermansia muciniphila
    ## 203  Akkermansia muciniphila
    ## 206  Akkermansia muciniphila
    ## 231  Akkermansia muciniphila
    ## 252  Akkermansia muciniphila
    ## 254  Akkermansia muciniphila
    ## 356  Akkermansia muciniphila
    ## 379  Akkermansia muciniphila
    ## 392  Akkermansia muciniphila
    ## 407  Akkermansia muciniphila
    ## 411  Akkermansia muciniphila
    ## 441  Akkermansia muciniphila
    ## 442  Akkermansia muciniphila
    ## 444  Akkermansia muciniphila
    ## 455  Akkermansia muciniphila
    ## 465  Akkermansia muciniphila
    ## 501  Akkermansia muciniphila
    ## 521  Akkermansia muciniphila
    ## 555  Akkermansia muciniphila
    ## 642  Akkermansia muciniphila
    ## 669  Akkermansia muciniphila
    ## 763  Akkermansia muciniphila
    ## 780  Akkermansia muciniphila
    ## 866  Akkermansia muciniphila
    ## 873  Akkermansia muciniphila
    ## 928  Akkermansia muciniphila
    ## 1003 Akkermansia muciniphila
    ## 1022 Akkermansia muciniphila

Podemos ver tres *bins* en los cuales encontramos *contigs*
identificados como *Akkermansia muciniphila*:
`maxbin_6samples.033.fasta`, `maxbin_6samples.034.fasta`, y
`maxbin_6samples.044.fasta`.

  - Vamos a comenzar por graficar los tres *bins* y las especies:

<div class="sourceCode">

``` sourceCode r
bin1 <- maxbin_6samples.033.fasta
bin2 <- maxbin_6samples.034.fasta
bin3 <- maxbin_6samples.044.fasta

sp <- "Akkermansia muciniphila"
cont_sp <- table_markers_resolved$Query[which(table_markers_resolved$Species %in% sp)]

par(mfrow=c(2,3))
for (i in c(3:8))
{
plot(table_all[,2], log(table_all[,i]), pch=20, xlab="GC content", ylab="abundance (log)", main=colnames(table_all[i]), cex.lab=1.5, cex.main=2)

points(table_all[table_all$contig %in% bin1 ,2], log(table_all[table_all$contig %in% bin1 ,i]), col="yellow", pch=20)
points(table_all[table_all$contig %in% bin2 ,2], log(table_all[table_all$contig %in% bin2 ,i]), col=rgb(0, 0, 1, 0.5), pch=20)
points(table_all[table_all$contig %in% bin3 ,2], log(table_all[table_all$contig %in% bin3 ,i]), col=rgb(0, 1, 0, 0.5), pch=20)
points(table_all[table_all$contig %in% cont_sp ,2], log(table_all[table_all$contig %in% cont_sp ,i]), col=rgb(1, 0, 0, 0.6), pch=20)
legend( x="bottomleft", 
        legend=c("maxbin_6samples.033.fasta", "maxbin_6samples.034.fasta","maxbin_6samples.044.fasta","Akkermansia muciniphila"), 
        col=c("yellow","blue","green","red"), 
        pch=20, merge=FALSE , cex=1.3)
}
```

</div>

![](images/plot_9.png)

Como se observa, los *contigs* asociados con esta especie se superponen
en los tres *bins*, los cuales a su vez son muy cercanos entre sí.

  - Podemos combinar estos *contigs* y formar uno nuevo:

<div class="sourceCode">

``` sourceCode r
new_bin4 <- unique(c(maxbin_6samples.033.fasta,maxbin_6samples.034.fasta,maxbin_6samples.044.fasta))
```

</div>

> **Usando el mismo método, échale un vistazo al *bin*
> `maxbin_6samples.011.fasta` y evalúa si se podría combinar con otros
> *bins*.**

</div>

</div>

<div id="abundancia-de-bins-y-comunidades-microbianas" class="section level1">

# <span class="header-section-number">5</span> Abundancia de *bins* y comunidades microbianas

Ahora que hemos visualizado y curado un *bin*, podemos mirarlo de nuevo
pero en un contexto más biológico. Tenemos tres muestras que provienen
de *C. quitensis* (G1\_1, G1\_2, and G1\_3) y otras tres que provienen
de plantas de *C. quitensis* que crecían en asociación con *D.
antarctica* (G2\_1, G2\_2, and G2\_3).

Para observar la abundancia de esos *bins* en las diferentes
condiciones, vamos a generar un mapa de calor o *heatmap* que represente
su abundancia en todas las muestras.

  - Primero generamos una tabla que contenga la información que
    necesitamos, i.e., abundancia del *bin* curado a partir de la
    abundancia de sus *contigs*.

<div class="sourceCode">

``` sourceCode r
table_new_bin <- as.data.frame(matrix(nrow=4, ncol=7, NA))
colnames(table_new_bin) <- c("bin","G1_1","G1_2","G1_3","G2_1","G2_2","G2_3")

bin_list <- c("new_bin","new_bin2","new_bin3","new_bin4")

for (i in c(1:length(bin_list)))
{

table_new_bin[i,1] <- bin_list[i]
curr_bin <- get(bin_list[i])
table_new_bin[i,2] <- sum(table_all[table_all$contig %in% curr_bin ,3])
table_new_bin[i,3] <- sum(table_all[table_all$contig %in% curr_bin ,4])
table_new_bin[i,4] <- sum(table_all[table_all$contig %in% curr_bin ,5])
table_new_bin[i,5] <- sum(table_all[table_all$contig %in% curr_bin ,6])
table_new_bin[i,6] <- sum(table_all[table_all$contig %in% curr_bin ,7])
table_new_bin[i,7] <- sum(table_all[table_all$contig %in% curr_bin ,8])

}

heatmap.2(as.matrix(table_new_bin[,c(2:7)]), dendrogram="none", trace="none", scale="row",labRow = c("new_bin","new_bin2","new_bin3","new_bin4"), margins = c(5, 10), Rowv=FALSE, Colv=FALSE)
```

</div>

![](mags_files/figure-html/unnamed-chunk-65-1.png)

Primero, vemos que las réplicas biológicas se comportan muy similarmente
para todos los *bins*. De hecho, las muestras G1\_1, G1\_2, y G1\_3 que
corresponden a *C. quitensis* tienen niveles similares de abundancia, de
la misma forma que las siguientes tres muestras, G2\_1, G2\_2 and G2\_3
que corresponden a *C. quitensis* en asociación con *D. antarctica*.

De los cuatro *bins* evaluados, solo uno tiene una mayor abundancia en
*C. quitensis* en asociación con *D. antarctica*. Este es el `new_bin4`
que identificamos previamente como *Akkermansia muciniphila*.  
*A. muciniphila* es una bacteria más comúnmente asociada con el
microbioma intestinal humano. Sin embargo, su alta abundancia es
consistente en todas las muestras de *C. quitensis* en asociación con
*D. antarctica*, lo cual es consistente con una señal biológica. Si
fuera contaminación, esperaríamos que surja de manera independiente a
alguna señal biológica en muestras no relacionadas. Además,
investigadores han mostrado que existen muchas similitudes entre el
microbioma intestinal y el rizosférico ( [Ramírez-Puebla et
al., 2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3536091/#B86) ).
En particular, *A. muciniphila* es conocida por expresar proteínas que
degradan mucinas en el intestino que a su vez contienen proteínas
altamente glicosiladas ( [Derrien et
al., 2008](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2258631/) ). En
la rizósfera, algunas plantas son capaces de atraer microorganismos a
través del mucilago, que también contiene una gran cantidad de proteínas
glicosiladas. Una explicación para la presencia de *A. muciniphila* en
la rizósfera de *D. antarctica* es que esta tiene un rol en mejorar la
resistencia de la planta a las condiciones extremas propias de la
Antártica.

De aquí en adelante, uno podría seguir investigando si hay algo
interesante en este MAG de *A. muciniphila* en relación a su contenido
de genes, sus funciones, y sus relaciones con otros aislados.

![](images/IMG_0902.JPG)

-----

![](images/back.png) [**Ir a la página de inicio del
curso**](http://castrolab.org/isme/bienvenida_WorkshopISME.html)

</div>

</div>

</div>

</div>

</div>

</div>

</div>
