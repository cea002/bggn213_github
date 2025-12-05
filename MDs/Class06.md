# Class 6: R functions
Courtney Anderson

-   [Our first (silly) function](#our-first-silly-function)
-   [A second function](#a-second-function)
-   [A protein generating function](#a-protein-generating-function)

All functions in R have at least 3 things -A **name** we pick this an
use it to call our function -Input **arguments** (there can be multiple)
-The **body** lines of R code that do the work

## Our first (silly) function

Write a function to add some numbers

``` r
add <- function(x, y=1) {
  x+y
}
```

Now we can call this funcion

``` r
add(c(10, 10), 100)
```

    [1] 110 110

## A second function

Write a function to generate random nucleotide sequences of a user
specifiec length:

The `sample` function can be helpful here

``` r
sample(c("A", "C","G","T"), size=50, replace= TRUE) 
```

     [1] "C" "C" "C" "C" "A" "A" "A" "T" "A" "T" "T" "T" "G" "G" "A" "C" "T" "T" "G"
    [20] "G" "T" "C" "G" "C" "T" "G" "G" "C" "T" "A" "A" "A" "G" "G" "A" "G" "C" "C"
    [39] "A" "A" "G" "T" "A" "C" "T" "G" "A" "A" "A" "A"

I want a 1 element long character vector that looks like this “CACGC”
not “C” “A” “C” “A” “G” “C”

``` r
generate_dna <- function(size=50) {
v <- sample(c("A", "C","G","T"), size= size, replace= TRUE) 
paste(v,collapse="")
}
```

Test it:

``` r
generate_dna(60)
```

    [1] "AAACGTACCAAGTCAATACTTACGCACGGCCGTGCCGGTCCGGAAGAACCCATTTGACCG"

=

``` r
fasta <- TRUE
if(fasta) {
  cat("HELLO You!")
} else {
  cat("No you dont!")
}
```

    HELLO You!

Add the ability to return a multi-element vector or a single element
fasta like vector.

``` r
generate_fasta <- function(size=50, fasta=TRUE) {
v <- sample(c("A", "C","G","T"), size= size, replace= TRUE) 
s <-paste(v,collapse="")
}
```

``` r
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

``` r
generate_fasta(60)
```

    [1] "GAGAGTACTAAAGCGAAAAGGGACTAGCGGTAAGTTGTCCGTGCGACAGGTACAACTCGG"

``` r
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

``` r
generate_fasta(fasta=FALSE)
```

     [1] "A" "A" "T" "A" "A" "A" "C" "C" "C" "T" "T" "T" "A" "C" "C" "G" "A" "T" "C"
    [20] "G" "C" "A" "G" "C" "T" "G" "T" "C" "C" "A" "A" "C" "T" "T" "T" "T" "A" "G"
    [39] "G" "A" "T" "T" "A" "C" "G" "A" "G" "A" "C" "G"

``` r
generate_fasta(fasta=TRUE)
```

    [1] "TGCAGGCCACTTATCCCGAGAGAAGTTTGGTGCTATTGATAGAGACAAAC"

## A protein generating function

``` r
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

``` r
generate_protein(6)
```

    [1] "YEEFVA"

Use our new`generate protein` function to make random protein seqeunces
of length 6 to 12 (i.e. one length 6, on length 7, etc. up to length
12).

One way to do this is brute force

``` r
generate_protein(6)
```

    [1] "RRKQPG"

``` r
generate_protein(7)
```

    [1] "DCSMWSI"

``` r
generate_protein(8)
```

    [1] "WFICVKLG"

``` r
generate_protein(9)
```

    [1] "TDDPLEFCV"

A second way is to use `for()` loop

``` r
lengths <- 6:12
lengths
```

    [1]  6  7  8  9 10 11 12

``` r
for(i in lengths) {
  cat(">",i, "\n", sep="")
  aa <- generate_protein(i)
  cat(aa)
  cat("\n")
}
```

    >6
    HYDGRA
    >7
    PLSKMNR
    >8
    WSDIVGFN
    >9
    SKEMEGVWY
    >10
    FEMICSISLY
    >11
    PYYDTSTLIVF
    >12
    GIVECEVAWGWV

A third and better way to solve this is to use `apply()`, specifically
the `sapply()` function in this case

``` r
sapply(6:12, generate_protein)
```

    [1] "CDHYWW"       "PPWMYQP"      "FMITQTQR"     "RLAKHGDCP"    "IHNWTRNTWT"  
    [6] "ELDIFNFFLFN"  "LFNMYDSYIRTN"
