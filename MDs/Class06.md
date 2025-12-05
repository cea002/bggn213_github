---
author:
- Courtney Anderson
authors:
- Courtney Anderson
title: "Class 6: R functions"
toc-title: Table of contents
---

-   [Our first (silly)
    function](#our-first-silly-function){#toc-our-first-silly-function}
-   [A second function](#a-second-function){#toc-a-second-function}
-   [A protein generating
    function](#a-protein-generating-function){#toc-a-protein-generating-function}

All functions in R have at least 3 things -A **name** we pick this an
use it to call our function -Input **arguments** (there can be multiple)
-The **body** lines of R code that do the work

## Our first (silly) function

Write a function to add some numbers

::: cell
``` {.r .cell-code}
add <- function(x, y=1) {
  x+y
}
```
:::

Now we can call this funcion

:::: cell
``` {.r .cell-code}
add(c(10, 10), 100)
```

::: {.cell-output .cell-output-stdout}
    [1] 110 110
:::
::::

## A second function

Write a function to generate random nucleotide sequences of a user
specifiec length:

The `sample` function can be helpful here

:::: cell
``` {.r .cell-code}
sample(c("A", "C","G","T"), size=50, replace= TRUE) 
```

::: {.cell-output .cell-output-stdout}
     [1] "T" "T" "T" "A" "C" "G" "G" "A" "A" "A" "G" "T" "T" "C" "G" "T" "G" "C" "G"
    [20] "G" "T" "C" "T" "A" "T" "T" "A" "G" "A" "G" "C" "C" "C" "T" "A" "G" "T" "G"
    [39] "C" "C" "T" "C" "T" "A" "A" "G" "A" "A" "C" "T"
:::
::::

I want a 1 element long character vector that looks like this "CACGC"
not "C" "A" "C" "A" "G" "C"

::: cell
``` {.r .cell-code}
generate_dna <- function(size=50) {
v <- sample(c("A", "C","G","T"), size= size, replace= TRUE) 
paste(v,collapse="")
}
```
:::

Test it:

:::: cell
``` {.r .cell-code}
generate_dna(60)
```

::: {.cell-output .cell-output-stdout}
    [1] "TAACATTACCCAAACTTTCACACCTAGTTTCACTATAATCACCCTCACGAATGAGGCCGC"
:::
::::

=

:::: cell
``` {.r .cell-code}
fasta <- TRUE
if(fasta) {
  cat("HELLO You!")
} else {
  cat("No you dont!")
}
```

::: {.cell-output .cell-output-stdout}
    HELLO You!
:::
::::

Add the ability to return a multi-element vector or a single element
fasta like vector.

::: cell
``` {.r .cell-code}
generate_fasta <- function(size=50, fasta=TRUE) {
v <- sample(c("A", "C","G","T"), size= size, replace= TRUE) 
s <-paste(v,collapse="")
}
```
:::

::: cell
``` {.r .cell-code}
generate_fasta <- function(size=50, fasta=TRUE) {
  v <- sample(c("A", "C","G","T"), size= size, replace = TRUE) 
  s <- paste(v, collapse = "")

  if(fasta) {
    return(s)
  } else {
    return(v)
  }
}
```
:::

:::: cell
``` {.r .cell-code}
generate_fasta(60)
```

::: {.cell-output .cell-output-stdout}
    [1] "TATAATCCCCAGTGACGGGGTTCCTCCAAACATACTCACGAAGATTAGTCTGGTGAAGGG"
:::
::::

::: cell
``` {.r .cell-code}
generate_fasta <- function(size=50, fasta=TRUE) {
  v <- sample(c("A", "C","G","T"), size= size, replace = TRUE) 
  s <- paste(v, collapse = "")

  if(fasta) {
    return(s)
  } else {
    return(v)
  }
}
```
:::

:::: cell
``` {.r .cell-code}
generate_fasta(fasta=FALSE)
```

::: {.cell-output .cell-output-stdout}
     [1] "T" "C" "C" "C" "A" "C" "G" "T" "G" "T" "C" "G" "A" "G" "T" "C" "G" "G" "A"
    [20] "C" "C" "T" "A" "C" "T" "G" "A" "C" "T" "T" "C" "T" "C" "T" "C" "T" "G" "T"
    [39] "A" "T" "A" "G" "C" "T" "G" "C" "A" "G" "T" "A"
:::
::::

:::: cell
``` {.r .cell-code}
generate_fasta(fasta=TRUE)
```

::: {.cell-output .cell-output-stdout}
    [1] "CTCCACGGACTCCGTCGTAAAACATCCGTTACGGAAATAGCCAGGGGGAG"
:::
::::

## A protein generating function

::: cell
``` {.r .cell-code}
generate_protein <- function(size = 50, fasta = TRUE) {
  amino_acids <- c("A", "C", "D", "E", "F", "G", "H", "I", "K", 
                   "L", "M", "N", "P", "Q", "R", "S", "T", "V", 
                   "W", "Y")
  v <- sample(amino_acids, size = size, replace = TRUE) 
  s <- paste(v, collapse = "")
  
  if (fasta) {
    return(s)
  } else {
    return(v)
  }
}
```
:::

:::: cell
``` {.r .cell-code}
generate_protein(6)
```

::: {.cell-output .cell-output-stdout}
    [1] "SWCGWA"
:::
::::

Use our new`generate protein` function to make random protein seqeunces
of length 6 to 12 (i.e. one length 6, on length 7, etc. up to length
12).

One way to do this is brute force

::::::: cell
``` {.r .cell-code}
generate_protein(6)
```

::: {.cell-output .cell-output-stdout}
    [1] "SQSPQG"
:::

``` {.r .cell-code}
generate_protein(7)
```

::: {.cell-output .cell-output-stdout}
    [1] "SRLYWWL"
:::

``` {.r .cell-code}
generate_protein(8)
```

::: {.cell-output .cell-output-stdout}
    [1] "SNSFMEFE"
:::

``` {.r .cell-code}
generate_protein(9)
```

::: {.cell-output .cell-output-stdout}
    [1] "KPSVADECE"
:::
:::::::

A second way is to use `for()` loop

:::: cell
``` {.r .cell-code}
lengths <- 6:12
lengths
```

::: {.cell-output .cell-output-stdout}
    [1]  6  7  8  9 10 11 12
:::
::::

:::: cell
``` {.r .cell-code}
for(i in lengths) {
  cat(">",i, "\n", sep="")
  aa <- generate_protein(i)
  cat(aa)
  cat("\n")
}
```

::: {.cell-output .cell-output-stdout}
    >6
    MTFTCV
    >7
    VTTFFEQ
    >8
    LSHEKWAV
    >9
    KIIELQWEG
    >10
    LAIRMFEFVT
    >11
    VLWASAVVWEV
    >12
    GKGKYQETPRGK
:::
::::

A third and better way to solve this is to use `apply()`, specifically
the `sapply()` function in this case

:::: cell
``` {.r .cell-code}
sapply(6:12, generate_protein)
```

::: {.cell-output .cell-output-stdout}
    [1] "PMYSGI"       "GECDPQY"      "GVVIMDEK"     "GKSVMSQSI"    "VEDHNVMAGC"  
    [6] "FLSMMYGHSEV"  "KYPETKYTMLYI"
:::
::::
