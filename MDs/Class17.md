# Class 17: Introduction to Genome Informatics HW
Courtney Anderson (PID:A69038035)

## Read in file and organize

``` r
df <- read.table("rs8067378_ENSG00000172057.6.txt", header = TRUE, sep = "\t")
```

``` r
df <- read.table("rs8067378_ENSG00000172057.6.txt", header = TRUE, sep = "")
```

## Assessing data

> Q13: Read this file into R and determine the sample size for each
> genotype and their corresponding median expression levels for each of
> these genotypes.

``` r
table(df$geno)
```


    A/A A/G G/G 
    108 233 121 

``` r
tapply(df$exp, df$geno, median)
```

         A/A      A/G      G/G 
    31.24847 25.06486 20.07363 

``` r
bp <- boxplot(exp ~ geno, data = df)
```

![](Class17HW.markdown_strict_files/figure-markdown_strict/unnamed-chunk-5-1.png)

``` r
bp$stats
```

             [,1]     [,2]     [,3]
    [1,] 15.42908  7.07505  6.67482
    [2,] 26.95022 20.62572 16.90256
    [3,] 31.24847 25.06486 20.07363
    [4,] 35.95503 30.55183 24.45672
    [5,] 49.39612 42.75662 33.95602

``` r
bp$stats[3, ]
```

    [1] 31.24847 25.06486 20.07363

## Making a fancier box plot

``` r
library(ggplot2)
```

    Warning: package 'ggplot2' was built under R version 4.5.2

``` r
ggplot(df, aes(x = geno, y = exp, fill = geno)) +
  geom_boxplot() +
  theme_minimal() +
  labs(
    title = "Expression by Genotype",
    x = "Genotype",
    y = "Expression Level"
  )
```

![](Class17HW.markdown_strict_files/figure-markdown_strict/unnamed-chunk-7-1.png)

> Q14: Generate a boxplot with a box per genotype, what could you infer
> from the relative expression value between A/A and G/G displayed in
> this plot? Does the SNP effect the expression of ORMDL3? I can infer
> that the A/A is expressed more frequently then G/G. Overall, I can say
> yes that SNP does affect the expression of ORMDL3; the G allele
> warrants lower expression whereas the A allele warrants higher
> expression.
