<div class="book without-animation with-summary font-size-2 font-family-1" data-basepath=".">

<div class="book-summary">

  - [Análisis de secuencias de 16S con
    DADA2](http://www.castrolab.org/isme/dada2/dada2.html)

  - 
  - [****0.1** Acerca del curso](#workshop)

  - [****1** Introducción al análisis por 16S rRNA con DADA2 y
    mothur](#introduccion-al-analisis-por-16s-rrna-con-dada2-y-mothur)
    
      - [****1.1** Configurar sesión de R](#configurar-sesion-de-r)
      - [****1.2** DADA2](#dada2)

  - 
  - [CASTRO LAB](http://www.castrolab.org)

  - [ISME LA 2019](https://isme-la2019.org)

</div>

<div class="book-body">

<div class="body-inner">

<div class="book-header" role="navigation">

# **[Análisis de secuencias de 16S con DADA2](./)

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
unidad: **Análisis de secuencias de 16S con DADA2**
![](images/bioinformatics.png)

> **Otras unidades del curso son**:
> 
> [**Introducción a R: Manipulación de datos y
> visualización**](http://www.castrolab.org/isme/introR/introR.html)
> ![](images/r.png)
> 
> [**Introducción a phyloseq y a análisis de
> diversidad**](http://www.castrolab.org/isme/biodiversity/biodiversity.html)
> ![](images/bacteria.png)
> 
> [**Búsqueda de genes de interés en datos de metagenómica
> *shotgun***](http://www.castrolab.org/isme/gene_search/gene_search.html)
> ![](images/gene_search.png)
> 
> [**Visualización y curación de genomas ensamblados desde metagenomas
> (MAGs)**](http://www.castrolab.org/isme/mags/mags.html)
> ![](images/genome.png)
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

<div id="introduccion-al-analisis-por-16s-rrna-con-dada2-y-mothur" class="section level1">

# <span class="header-section-number">1</span> Introducción al análisis por 16S rRNA con DADA2 y mothur

Lo primero que tenemos que hacer es instalar y cargar los paquetes que
vamos a usar en esta sección. Algunos están alojados en el repositorio
[CRAN](https://cran.rediris.es), otros en el repositorio
[Bioconductor](https://www.bioconductor.org), y otros en el repositorio
[GitHub](https://github.com).

<div id="configurar-sesion-de-r" class="section level2">

## <span class="header-section-number">1.1</span> Configurar sesión de R

Para trabajar en R, hay tres primeros pasos que debemos seguir: (1)
cargar a los paquetes necesarios a la sesión actual, (2) configurar el
directorio de trabajo, (3) importar datos de entrada a la sesión actual.

Los siguientes 3 *scripts* te mostrarán una manera eficiente de instalar
y cargar la lista de paquetes según su repositorio de origen, ya que
cada repositorio tiene su propia función para instlar sus paquetes.

  - Primero, enlistar los paquetes necesarios en diferentes vectores
    dependiendo de su repositorio de origen.

<div class="sourceCode">

``` sourceCode r
# Definir paquetes
## Repositorio CRAN
cran_packages <- c("bookdown", "knitr", "tidyverse", "plyr", "grid", "gridExtra", "kableExtra", "xtable", "ggpubr")
## Repositorio Bioconductor
bioc_packages <- c("phyloseq", "dada2", "DECIPHER", "phangorn", "ggpubr", "BiocManager","DESeq2", "microbiome", "philr")
## Repositorio GitHub
git_source <- c("twbattaglia/btools", "gmteunisse/Fantaxtic", "MadsAlbertsen/ampvis2", "opisthokonta/tsnemicrobiota")
# fuente/nombre del paquete
git_packages <- c("btools", "fantaxtic", "ampvis2", "tsnemicrobiota") # nombre del paquete
```

</div>

  - Segundo, instalar los paquetes definidos arriba usando la función
    correspondiente a cada repositorio.

<div class="sourceCode">

``` sourceCode r
# Instalar paquetes CRAN
.inst <- cran_packages %in% installed.packages()
if(any(!.inst)) {
  install.packages(cran_packages[!.inst])
}
# Intalar paquetes BioConductor
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
.inst <- bioc_packages %in% installed.packages()
if(any(!.inst)) {
  BiocManager::install(bioc_packages[!.inst])
}
# Instalar paquetes GitHub
.inst <- git_source %in% installed.packages()
if(any(!.inst)) {
  devtools::install_github(git_source[!.inst])
}
```

</div>

  - Tercero, cargar los paquetes requeridos a la sesión actual de R.

<div class="sourceCode">

``` sourceCode r
# Cargar paquetes
sapply(c(cran_packages, bioc_packages, git_packages), require, character.only = TRUE)
```

</div>

El paso de instalación de paquetes es necesario solamente una vez.
Excepto si se quiere actualizar la versión del paquete, o bien, R ha
sido desinstalado e instalado nuevamente o actualizado su versión.

Si ya tienes los paquetes instalados en tu computadora, sólo necesitas
enlistar (*1er script*) y cargar (*3er script*) los paquetes. También,
puedes cargarlos a la sesión actual de R usando la función `library()`,
así:

    # Cargar paquetes
    library(tidyverse)
    library(plyr)
    library(grid)
    library(gridExtra)
    library(kableExtr)
    library(xtable)
    library(ggpubr)
    library(phyloseq)
    library(dada2)
    library(DECIPHER)
    library(phangorn)
    library(ggpubr)
    library(BiocManager)
    library(DESeq2)
    library(microbiome)
    library(philr)
    library(btools)
    library(fantaxtic)
    library(ampvis2)
    library(tsnemicrobiota)

</div>

<div id="dada2" class="section level2">

## <span class="header-section-number">1.2</span> DADA2

> Este tutorial se basa fuertemente en el trabajo de otros
> investigadores publicado en Callahan BJ, Sankaran K, Fukuyama JA et
> al. [Bioconductor Workflow for Microbiome Data Analysis: from raw
> reads to community
> analyses](https://doi.org/10.12688/f1000research.8986.2) \[version 2;
> peer review: 3 approved\]. F1000Research 2016, 5:1492

En esta sección vamos a explorar un set de datos de 98 muestras de piel
de ballenas, jorobadas, azules y otras. Las muestras fueron obtenidas al
amplificar y secuenciar un fragmento de la región variable 4 (V4) del
gen 16S rRNA gene. Las secuencias están disponibles en este
[link](https://www.dropbox.com/sh/vkc1p96ney0x6kh/AADjo1ayo3ic5OhzNE2WRac1a?dl=0)
y también en la base de datos de NCBI SRA bajo el número de acceso
[PRJNA428495](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA428495).

Entonces, lo que vamos a hacer es correr nuestros datos según la
pipeline [DADA2](https://www.nature.com/articles/nmeth.3869). DADA2
ofrece ventajas con respecto a la estrategia de formar clusters (OTUs)
en varios aspectos que incluyen mayor resolución, nombres consistentes
entre diferentes análisis, mejor estimación de abundancias relativas,
etc. Los siguientes artículos ofrecen una discusión más detallada al
respecto ( [artículo](https://www.nature.com/articles/ismej2017119) y
[artículo](http://fiererlab.org/2017/05/02/lumping-versus-splitting-is-it-time-for-microbial-ecologists-to-abandon-otus/)).

  - Lo primero que vamos a hacer es *descargar* las **reads** que están
    contenidas en la carpeta `whale_pe/`, descárgala
    [AQUÍ](https://www.dropbox.com/s/es3ehm61z0hlmb6/whale_pe.zip?dl=0).

  - Ahora, vamos a configurar el directorio donde están las **reads**:

<div class="sourceCode">

``` sourceCode r
set.seed(100) 
# Configuramos un número de semilla para que los resultados 
# sean comparables entre estudiantes

# Simplemente cambiamos este path al que corresponda según donde están las reads
miseq_path <- "~/Dropbox/CastroLab_database/workshops_data/whale_pe/"
list.files(miseq_path)
```

</div>

Asegúrate de modificar la ruta:
`~/Dropbox/CastroLab_database/workshops_data/whale_pe/` a la ruta de
directorios según tu computadora.

  - En DADA2 las reads se trabajan inicialmente por separado, es decir,
    la copia forward o R1 separada de la copia reverse o R2. Por esto,
    nos tenemos que asegurar que ambos archivos estén ordenados. Luego,
    manipulamos el nombre de los archivos para generar automáticamente
    el nombre de las muestras en nuestro análisis:

<div class="sourceCode">

``` sourceCode r
fnFs <- sort(list.files(miseq_path, pattern="_1.fastq"))
fnRs <- sort(list.files(miseq_path, pattern="_2.fastq"))

# Extracción del nombre de las muestras
sampleNames <- sapply(strsplit(fnFs, "_"), `[`, 1)
# Especificamos el path completo para evitar errores de ambigüedad
fnFs <- file.path(miseq_path, fnFs)
fnRs <- file.path(miseq_path, fnRs)
```

</div>

  - El paquete DADA2 tiene muchas funciones útiles para el
    preprocesamiento de los datos. Acá lo que hacemos es visualizar el
    perfil de calidad de las muestras para cada par de 5’ a 3’:

<div class="sourceCode">

``` sourceCode r
plotQualityProfile(fnFs[1:2])
plotQualityProfile(fnRs[1:2])
```

</div>

<div class="figure">

![](images/plotQuality.png)

</div>

  - Ahora vamos a proceder con el filtrado y corte de las reads de
    acuerdo a lo observado en los gráficos de calidad. Primero creamos
    un directorio donde vamos a poner las reads una vez realizado el
    control de calidad, y luego realizamos el dicho control. En
    específico usamos los argumentos `trunLen =`, `maxN=`, `maxEE=` y
    `rm.phix=` para indicar el corte promedio de cada read, el número
    máximo de bases indeterminadas, el número máximo de errores, y si
    es que queremos remover secuencias pertenecientes al control interno
    de Illumina.

<div class="sourceCode">

``` sourceCode r
# Creamos un directorio para poner las reads "limpias"
filt_path <- file.path(miseq_path, "filtered") 
if(!file_test("-d", filt_path)) dir.create(filt_path)
filtFs <- file.path(filt_path, paste0(sampleNames, "_F_filt.fastq.gz"))
filtRs <- file.path(filt_path, paste0(sampleNames, "_R_filt.fastq.gz"))

# Y finalmente procedemos con el control de calidad
out <- filterAndTrim(fnFs, filtFs, fnRs, filtRs, truncLen=c(250,200),
              maxN=0, maxEE=c(2,2), truncQ=2, rm.phix=TRUE,
              compress=TRUE, multithread=TRUE) 
# Si usan Windows, configuren multithread=FALSE
```

</div>

  - DADA2 genera un modelo probabilístico de errores con el cual puede
    filtrar reads erróneas y así usar las restantes directamente para la
    etapa de clasificación taxonómica. Esta parte del método es la que
    nos permite tener una mayor resolución en comparación a los análisis
    basados en OTUs. Como las muestras probablemente tienen reads
    idénticas, no es eficiente usar cada una de ellas para los pasos
    río abajo. Por esto, DADA2 recomienda *de-replicar* las muestras
    para así disminuir la redundancia y avanzar eficientemente.

<div class="sourceCode">

``` sourceCode r
derepFs <- derepFastq(filtFs, verbose=TRUE)
derepRs <- derepFastq(filtRs, verbose=TRUE)

# Acá simplemente agregamos los nombres de las muestras al objeto de-replicado
names(derepFs) <- sampleNames
names(derepRs) <- sampleNames
```

</div>

  - Una vez con las muestras de-replicadas procedemos a generar un
    modelo de errores. Para mayor detalle sobre este crucial paso
    revisen el artículo original
    [aquí](https://www.nature.com/articles/nmeth.3869).

<div class="sourceCode">

``` sourceCode r
errF <- learnErrors(filtFs, multithread=TRUE)
errR <- learnErrors(filtRs, multithread=TRUE)

# Graficamos los errores para cada par
plotErrors(errF)
plotErrors(errR)
```

</div>

<div class="figure">

![](images/plotErrors.png)

</div>

Cada panel en el gráfico nos indica la frecuencia de error de
nuecleótido a nucleótido para todas las combinaciones. Naturalmente,
las bases con mayor puntaje de calidad exhiben una menor frecuencia de
error.

  - Con esta información entonces procedemos al paso más importante de
    la pipeline, i.e., la inferencia de las *Amplicon Sequence Variants*
    (ASVs).

<div class="sourceCode">

``` sourceCode r
dadaFs <- dada(derepFs, err=errF, multithread=TRUE, pool = TRUE)
dadaRs <- dada(derepRs, err=errR, multithread=TRUE, pool = TRUE)
```

</div>

  - Ahora nos queda hacer un poco de aseo: primero unimos las reads R1 y
    R2, luego generamos una tabla de secuencias que vamos a utilizar más
    tarde, y finalmente removemos secuencias artefactuales producto de
    la amplificación por PCR.

<div class="sourceCode">

``` sourceCode r
# Unimos las reads
mergers <- mergePairs(dadaFs, derepFs, dadaRs, derepRs)
# Generamos una tabla de secuencias
seqtabAll <- makeSequenceTable(mergers[!grepl("Mock", names(mergers))])
table(nchar(getSequences(seqtabAll)))
# Removemos las secuencias quiméricas
seqtabNoC <- removeBimeraDenovo(seqtabAll)
```

</div>

La tabla de secuencias sin las quimeras, es la tabla que usamos para
realizar la clasificación taxonómica. En principio, podríamos usar
cualquiera de las tres bases de datos más populares para clasificación
de secuencias del 16S rRNA, i.e., *GreenGenes*, *RDP* o *SILVA*.

  - En nuestro práctico vamos a utilizar SILVA, en particular, la
    versión 132.

  - Antes de continuar, descarga los archivos de la base de datos SILVA
    que vamos a utilizar: `silva_nr_v132_train_set.fa`
    [AQUÍ](https://www.dropbox.com/s/0yaxvi0qkuuas1w/silva_nr_v132_train_set.fa?dl=0)
    y `silva_species_assignment_v132.fa`
    [AQUÍ](https://www.dropbox.com/s/oyeh1w2w6qc83p1/silva_species_assignment_v132.fa?dl=0).

<div class="sourceCode">

``` sourceCode r
fastaRef <- "~/Dropbox/CastroLab_database/workshops_data/silva_nr_v132_train_set.fa"
taxTab <- assignTaxonomy(seqtabNoC, refFasta = fastaRef, multithread=TRUE)

# En el caso de querer agregar el rango taxonómico de especies, simplemente usamos una base de datos extra, la cual contiene esta información. 
taxTabExtra <- addSpecies(taxTab, "~/Dropbox/CastroLab_database/workshops_data/silva_species_assignment_v132.fa", verbose=TRUE)
unname(head(taxTab)) -> tabla
colnames(tabla) <- c("Kingdom", "Phylum", "Order", "Class", "Family", "Genus")
```

</div>

Ya que nuestro perfil taxonómico se construye con secuencias homólogas
del 16S rRNA gene, podemos usar estas secuencias para inferir un árbol
filogenético. Existen muchos paquetes de r que pueden hacer esto y aquí
escogemos *phangorn* para hacer una inferencia basada en Maximum
Likelihood.

  - Primero alineamos las secuencias:

<div class="sourceCode">

``` sourceCode r
seqs <- getSequences(seqtabNoC)
# Este paso propaga los nombres de las secuencias al árbol
names(seqs) <- seqs
alignment <- AlignSeqs(DNAStringSet(seqs), anchor=NA,verbose=FALSE)
```

</div>

  - Y con ese alineamiento inferimos un árbol de partida o *starting
    tree* para inicializar la búsqueda por ML. También ajustamos un
    modelo de sustitución nucleotídica para parametrizar la tasa de
    cambio de un nucleótido a otro y asó poder inferir correctamente el
    largo de las ramas y la topología del árbol.

<div class="sourceCode">

``` sourceCode r
phangAlign <- phyDat(as(alignment, "matrix"), type="DNA")
dm <- dist.ml(phangAlign)
treeNJ <- NJ(dm) 
fit = pml(treeNJ, data=phangAlign)
fitGTR <- update(fit, k=4, inv=0.2)
fitGTR <- optim.pml(fitGTR, model="GTR", optInv=TRUE, optGamma=TRUE,
        rearrangement = "stochastic", control = pml.control(trace = 0))
```

</div>

<div class="sourceCode">

``` sourceCode r
detach("package:phangorn", unload=TRUE)
```

</div>

Ahora, tenemos todos los ingredientes para formar un objeto de R que
contenga todo lo que nos importa en un experimento metagenómico, i.e.,
una tabla de cuentas que indica el número de reads por seceuncia del 16S
rRNA gene, una tabla con el linaje taxonómico de esas secuencias, un
árbol filogenético que relaciona esas secuencias entre sí, y
finalmente, una tabla con variables asociadas a nuestras muestras,
también llamada **metadata**.

  - Descarga la metadata
    [AQUÍ](https://www.dropbox.com/s/dafb0g5zn8c8n41/metadata.csv?dl=0).

  - Con estos elementos procedemos a generar un objeto **phyloseq**:

<div class="sourceCode">

``` sourceCode r
samdf <- read.csv("~/Dropbox/CastroLab_database/workshops_data/metadata.csv", header=TRUE, row.names = 1)
rownames(seqtabNoC) %in% rownames(samdf)
all(rownames(seqtabAll) %in% samdf$run)

ps <- phyloseq(otu_table(seqtabNoC, taxa_are_rows=FALSE), 
               sample_data(samdf), 
               tax_table(taxTab),
               phy_tree(fitGTR$tree))
ps <- prune_samples(sample_names(ps) != "Mock", ps) # Remover potenciales muestras sintéticas
ps
```

</div>

  - Y el producto final del análisis por DADA2 es entonces un objeto
    *phyloseq*:

| Slot           | Descripción        | Resultado                               |
| :------------- | :----------------- | :-------------------------------------- |
| otu\_table()   | OTU Table:         | \[ 1476 taxa and 98 samples \]          |
| sample\_data() | Sample Data:       | \[ 98 samples by 13 sample variables \] |
| tax\_table()   | Taxonomy Table:    | \[ 1476 taxa by 6 taxonomic ranks \]    |
| phy\_tree()    | Phylogenetic Tree: | \[ 1476 tips and 1474 internal nodes \] |

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

</div>
