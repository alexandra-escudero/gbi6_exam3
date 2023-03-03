---
title: "Bioinformática- GBI6"
subtitle: "EXAMEN FINAL - 2022II"
author: "Escudero Kimberly"
date: "2023-03-02"
output:
  html_document:
    toc: yes
    toc_depth: 4
    highlight: espresso
    theme: paper
    toc_float:
      collapsed: no
      smooth_scroll: yes
---



<center><img src="https://www.ikiam.edu.ec/wp-content/uploads/2021/12/logo-ikiam-1.png"></center>

## INDICACIONES

-   Coloque su apellido y nombre en el campo `author`.

-   Clone el repositorio de GitHub

-   Cree un `Project` y enlace al repositorio clonado.

-   Resuelva lo solicitado.

-   Genere un reporte en formato `.html` o `.pdf`

## Librerías requeridas

-   Asegúrese de que todas las librerías se cargan adecuadamente. Si es necesario instale las librerías utilizando el comando:


```r
install.packages("packagename")
```

-   En el caso de las librerías de Bioconductor requiere instalarlo usando la instrucción.


```r
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install(version = "3.16")
```

-   Luego debe instalar la paquetería de bioconductor, con la instrucción:


```r
BiocManager::install("packagename")
```

-   Si la librería está en desarrollo, debe tener la librería `devtools` y luego ejecutar:


```r
devtools::install_github("kassambara/ggpubr")
```

**Las librerías requeridas en esta evaluación son**:


```r
library(ggpmisc); library(ggplot2); library(plotly); library(palmerpenguins)
```

```
## Loading required package: ggpp
```

```
## Loading required package: ggplot2
```

```
## 
## Attaching package: 'ggpp'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     annotate
```

```
## 
## Attaching package: 'plotly'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
```

```
## The following object is masked from 'package:stats':
## 
##     filter
```

```
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
library(ggplot2); library(magrittr); library(ggpubr); library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.0     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ lubridate 1.9.2     ✔ tibble    3.1.8
## ✔ purrr     1.0.1     ✔ tidyr     1.3.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ ggpp::annotate()   masks ggplot2::annotate()
## ✖ tidyr::extract()   masks magrittr::extract()
## ✖ dplyr::filter()    masks plotly::filter(), stats::filter()
## ✖ dplyr::lag()       masks stats::lag()
## ✖ purrr::set_names() masks magrittr::set_names()
## ℹ Use the ]8;;http://conflicted.r-lib.org/conflicted package]8;; to force all conflicts to become errors
```

```r
library(ComplexHeatmap); library(viridis)
```

```
## Loading required package: grid
## ========================================
## ComplexHeatmap version 2.14.0
## Bioconductor page: http://bioconductor.org/packages/ComplexHeatmap/
## Github page: https://github.com/jokergoo/ComplexHeatmap
## Documentation: http://jokergoo.github.io/ComplexHeatmap-reference
## 
## If you use it in published research, please cite either one:
## - Gu, Z. Complex Heatmap Visualization. iMeta 2022.
## - Gu, Z. Complex heatmaps reveal patterns and correlations in multidimensional 
##     genomic data. Bioinformatics 2016.
## 
## 
## The new InteractiveComplexHeatmap package can directly export static 
## complex heatmaps into an interactive Shiny app with zero effort. Have a try!
## 
## This message can be suppressed by:
##   suppressPackageStartupMessages(library(ComplexHeatmap))
## ========================================
## 
## 
## Attaching package: 'ComplexHeatmap'
## 
## The following object is masked from 'package:plotly':
## 
##     add_heatmap
## 
## Loading required package: viridisLite
```

# [4.0 PUNTOS] 1. Pingüinos de Palmer

La base de datos de esta pregunta contiene distintas mediciones para tres especies de pingüinos encontrados en el archipiélago de Palmer, en la Antártica. Estas tres especies son los Chinstrap, Gentoo y Adélie.

<center><img src="https://bookdown.org/rodolfo_carvajal/apunte/figs/penguins.png"></center>

<center><img src="https://bookdown.org/rodolfo_carvajal/apunte/figs/culmen_depth.png"></center>

Puede revisar los datos de los pingüinos utilizando la instrucción `str()` o `skim()`.


```r
skimr::skim(penguins)
```


Table: Data summary

|                         |         |
|:------------------------|:--------|
|Name                     |penguins |
|Number of rows           |344      |
|Number of columns        |8        |
|_______________________  |         |
|Column type frequency:   |         |
|factor                   |3        |
|numeric                  |5        |
|________________________ |         |
|Group variables          |None     |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts                  |
|:-------------|---------:|-------------:|:-------|--------:|:---------------------------|
|species       |         0|          1.00|FALSE   |        3|Ade: 152, Gen: 124, Chi: 68 |
|island        |         0|          1.00|FALSE   |        3|Bis: 168, Dre: 124, Tor: 52 |
|sex           |        11|          0.97|FALSE   |        2|mal: 168, fem: 165          |


**Variable type: numeric**

|skim_variable     | n_missing| complete_rate|    mean|     sd|     p0|     p25|     p50|    p75|   p100|hist  |
|:-----------------|---------:|-------------:|-------:|------:|------:|-------:|-------:|------:|------:|:-----|
|bill_length_mm    |         2|          0.99|   43.92|   5.46|   32.1|   39.23|   44.45|   48.5|   59.6|▃▇▇▆▁ |
|bill_depth_mm     |         2|          0.99|   17.15|   1.97|   13.1|   15.60|   17.30|   18.7|   21.5|▅▅▇▇▂ |
|flipper_length_mm |         2|          0.99|  200.92|  14.06|  172.0|  190.00|  197.00|  213.0|  231.0|▂▇▃▅▂ |
|body_mass_g       |         2|          0.99| 4201.75| 801.95| 2700.0| 3550.00| 4050.00| 4750.0| 6300.0|▃▇▆▃▂ |
|year              |         0|          1.00| 2008.03|   0.82| 2007.0| 2007.00| 2008.00| 2009.0| 2009.0|▇▁▇▁▇ |

A continuación se muestra un ejemplo de análisis de la data de los pingüinos de Palmer:

-   En la figura `p1` se tiene un errorplot donde el largo del pico es evaluado por cada especie e internamente por sexo del ave.

-   En la figura `p2` se tiene un boxplot donde se compara el ancho del pico por cada especie.

-   En la figura `p3` se tiene una regresión lineal donde se mide el efecto de la longitud del pico sobre el ancho y se desagrega por especie(fila) e isla (columnas).

-   Finalmente la figura compuesta con las tres figuras anteriores en una estrucutura:

<center><img src="result/ejemplo1.png" width="200" height="200" alt=""></center>


```
## `geom_smooth()` using formula = 'y ~ x'
```

<img src="2022II_GBI6_ExamenFinal_files/figure-html/pressure-1.png" width="672" />

### [1.0 punto] 1.1. Interprete lo que se muestra en la figura anterior

En el primer gráfico (p1), la variable "color" está establecida como "sex", que diferencia por color según el sexo del pingüino en rosado, verde y azul. Es decir, los puntos de color diferentes representan diferentes sexos de pingüinos. En el segundo gráfico (p2), la variable "color" está establecida como "species", lo que significa que el ancho del pico se diferencia por color según la especie de pingüino. Es decir, los puntos de color diferentes representan diferentes especies de pingüinos. En el tercer gráfico (p3), la variable "color" está establecida como "factor(species)", lo que significa que cada especie de pingüino se representa con un color diferente en el gráfico de dispersión.

### [3.0 puntos] 1.2. Genere dos gráficas `p4` y `p5`donde:

-   `p4` es una regresión de `x: body_mass_g` y `y: flipper_length_mm`, que tiene inserto la ecuación de la regresión y el $R^2$. Asimismo tiene una coloración por sexo, y una separación por sexo e isla.

-   `p5` tiene un correlation plot de las variables numéricas de longitud de pico, ancho de pico, longitud de aleta y masa corporal. La figura tiene que tener la apariencia de la imagen de abajo, este se encuentra resuelto en la página de [ggcorrplot](http://www.sthda.com/english/wiki/ggcorrplot-visualization-of-a-correlation-matrix-using-ggplot2).

    


```
## Warning: `stat(eq.label)` was deprecated in ggplot2 3.4.0.
## ℹ Please use `after_stat(eq.label)` instead.
```

```
## Warning: Removed 2 rows containing non-finite values (`stat_poly_eq()`).
```

```
## Warning: Not enough data to perform fit for group 3; computing mean instead.
```

```
## Warning: Removed 2 rows containing missing values (`geom_point()`).
```

<img src="2022II_GBI6_ExamenFinal_files/figure-html/unnamed-chunk-12-1.png" width="672" />

-   Realice una composición de figuras que se indica en la figura de abajo e interprete.

<img src="2022II_GBI6_ExamenFinal_files/figure-html/unnamed-chunk-13-1.png" width="672" />


```r
penguins_clean <- na.omit(penguins)
penguins_num <- penguins_clean[, c(3, 4, 5, 6)]
penguins_corr <- cor(penguins_num)

p5 <- ggcorrplot(penguins_corr, type = "upper", hc.order = TRUE,lab = TRUE, lab_size = 10,
                 ggtheme = ggplot2::theme_gray, colors = c("green", "yellow","pink"))
p5
```

<img src="2022II_GBI6_ExamenFinal_files/figure-html/p1.1-1.png" width="672" />

**INTERPRETACIÓN:** Se muestran salidas del color, picos usando parametros he identificadores, tomando en cuenta la relación entre masa corporal y las longitudes de los pinguinos de p4 y p5.

# [4.0 PUNTOS] 2. MAPAS DE CALOR DE EXPRESIÓN GÉNICA

Los datos de expresión de genes son extensos, hay una gran cantidad de genes y asimismo una gran cantidad de muestras de tejidos o lineas celulares. En este ejemplo se desea ver el nivel de relación de las muestras de diferentes tipos de tejidos en base a las cuantificaciones de niveles de expresión genética. La data ejemplo es sintética, y están guardadas en forma de tablas y se cargan con la función `load('nombre.RData')`. Está basado en [Simple guide to heatmaps](https://davemcg.github.io/post/simple-heatmaps-with-complexheatmaps/).


```r
load('data/expression.Rdata') # carga la tabla de expression
load('data/metadata.Rdata')
str(expression)
```

```
## rowws_df [2,191 × 11] (S3: rowwise_df/tbl_df/tbl/data.frame)
##  $ Gene: chr [1:2191] "ABCA1" "ABCA10" "ABCA13" "ABCA2" ...
##  $ X10 : num [1:2191] 13.97 10.88 9.17 11.34 8.77 ...
##  $ X11 : num [1:2191] 12.24 8.81 7.63 14.98 11.64 ...
##  $ X16 : num [1:2191] 12.63 11.25 7.49 13.68 11.29 ...
##  $ X17 : num [1:2191] 13.18 9.87 8.13 12.01 8.07 ...
##  $ X18 : num [1:2191] 11.38 8.06 8.26 16.01 12.39 ...
##  $ X2  : num [1:2191] 12.33 9.35 6.43 13.44 10.13 ...
##  $ X3  : num [1:2191] 13.48 11.25 9.96 12.26 10.13 ...
##  $ X4  : num [1:2191] 11.38 8.59 7.5 16.01 11.51 ...
##  $ X9  : num [1:2191] 10.98 9.48 7.08 15.39 11.34 ...
##  $ var : num [1:2191] 1.06 1.39 1.15 3.17 2.04 ...
```

Esta data indica el nivel de expressión de los genes (filas) en cada muestra de células (columnas).


```r
# El procesamiento es:
# 1. seleccionar solamente las muestras select()
# 2. transponer t()
# 3. calcula las distancias encluideanas basado en las medidas dis()
# 4. hacer que se vuelva una matriz de tipo dataframe
expr_dist <- expression %>% select(-Gene, -var) %>% 
  t() %>% 
  dist() %>% 
  as.matrix() %>% data.frame() 
dim(expr_dist)
```

```
## [1] 9 9
```

Se realiza un gráfico de mapa de calor preliminar que permite tener un primer vistazo del nivel de relación entre cada una de las muestras en base a la distancia euclideana.


```r
Heatmap(expr_dist)
```

```
## Warning: The input is a data frame-like object, convert it to a matrix.
```

<img src="2022II_GBI6_ExamenFinal_files/figure-html/unnamed-chunk-16-1.png" width="672" />

Usualmente lo que se desea es saber si las muestras vienen de diferentes tejidos


```r
metadata_heatmap <- metadata  %>% 
  mutate(sample = paste0('X', sample)) %>% # nombres de muestras
  filter(sample %in% colnames(expr_dist)) %>% 
  dplyr::select(sample, treatment_hours, serum) %>% 
  mutate(sample=factor(sample, levels=colnames(expr_dist))) %>% 
  arrange(sample) %>%  unique() 

ha_column = HeatmapAnnotation(df = data.frame(Tiempo = metadata_heatmap$treatment_hours,
                                              Suero = metadata_heatmap$serum), 
                              col = list(Serum = c("HS" =  magma(20)[2], "HIHS" = magma(20)[3]),
                                         Time = c("24" = magma(20)[14], "48" = magma(20)[12])))

# Mapa de calor anotado en la parte superior
Heatmap(expr_dist,  col=viridis(10), 
        name = 'Distancias', top_annotation = ha_column, )
```

```
## Warning: The input is a data frame-like object, convert it to a matrix.
```

<img src="2022II_GBI6_ExamenFinal_files/figure-html/unnamed-chunk-17-1.png" width="672" />

## [1.0 punto] 2.1. Interpretación del Mapa de calor

Realice una descripción de lo que observa en el mapa de calor considerando:

-   las intensidades de color de las distancias, se deriva con mayor intensidad mientras va creciendo la gráfica

-   el tiempo de exposición al tratamiento, es menor de acuerdo a la distancia

    -   el tipo de suero has má tipo de sueros HIHS que HS

**INTERPRETACIÓN:**

La grafica presenta magnitudes diferentes entre la distancia, tiempo y sueros dentro de las muestras de un tejido.

## [3.0 puntos] 2.2. Mapa de calor artritis reumatoide.

Realice la réplica e interpretación de los niveles de expresión génica en muestras de personas que sufren de artritits reumatoide; que se muestra en la sección 5 de la página [A simple tutorial for a complex ComplexHeatmap](https://github.com/kevinblighe/E-MTAB-6141) y que se basa en el artículo [Volume 28, Issue 9, 27 August 2019, Pages 2455-2470.e5](https://www.sciencedirect.com/science/article/pii/S2211124719310071?via%3Dihub).


```r
require(RColorBrewer); require(ComplexHeatmap); require(circlize); 
```

```
## Loading required package: RColorBrewer
```

```
## Loading required package: circlize
```

```
## ========================================
## circlize version 0.4.15
## CRAN page: https://cran.r-project.org/package=circlize
## Github page: https://github.com/jokergoo/circlize
## Documentation: https://jokergoo.github.io/circlize_book/book/
## 
## If you use it in published research, please cite:
## Gu, Z. circlize implements and enhances circular visualization
##   in R. Bioinformatics 2014.
## 
## This message can be suppressed by:
##   suppressPackageStartupMessages(library(circlize))
## ========================================
```

```r
require(digest); require(cluster)
```

```
## Loading required package: digest
```

```
## Loading required package: cluster
```

Aquí se carga los datos `EMTAB6141.rdata` que se requiere para este ejercicio. Requieres usar:

-   `'mat.tsv'`

-   `'metadata.tsv'`

-   `'sig_genes.list'`


```r
# Cargue aquí sus datos
```

En la siguiente celda de código, realice la réplica del mapa de calor que se encuentra a la izquierda (`hmap1`) de esta figura:

<center><img src="https://github.com/kevinblighe/E-MTAB-6141/raw/master/README_files/figure-gfm/clusterheatmap_fig2-1.png" width="700" height="700" alt=""></center>


```r
# Escriba aquí sus códigos
```

**INTERPRETACIÓN:**

# [2 PUNTOS] REPOSITORIO GITHUB

Su repositorio de GitHub debe tener al menos los sigueites elementos:

-   Haber sido \``clonado` del repositorio del profesor.

-   Haber sido enlazado a un repositorio local (`Project`) generado en RStudio.

-   Tener el archivos .Rmd

-   Tener el archivo .HTML del examen (**MANDATORIO PARA CALIFICAR**).

-   Tener al menos 3 controles de la versión.

-   Tener un README.md con:

    -   información personal,

    -   información del equipo,

    -   los programas y paquetes utilizados, y sus respectivas versiones
