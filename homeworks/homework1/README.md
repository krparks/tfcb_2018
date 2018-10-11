TFCB 2018: Homework 1
=====================

Oct 4, 2018

*Due 12pm, Oct 11, 2018*

-   [Problem 1](#problem-1)
-   [Problem 2](#problem-2)
-   [Problem 3](#problem-3)
-   [Problem 4](#problem-4)
-   [Problem 5](#problem-5)
-   [Problem 6](#problem-6)
-   [Problem 7](#problem-7)
-   [Problem 8](#problem-8)
-   [Problem 9](#problem-9)

In this homework, we will learn how to analyze a recently published deep sequencing
dataset using `tidyverse` functions.

In the process, we will learn some new functions in `tidyverse` and apply
them to our data analysis.

``` r
library(tidyverse)
```

## Problem 1

**10 points**

Provide a &lt;100 char description and a URL reference for each of these functions.

1. `!`
2. `is.na`
3. `is.numeric`
4. `anti_join`
5. `desc`
6. `slice`
7. `all_vars`
8. `funs`
9. `filter_if`
10. `mutate_if`

## Problem 2

**15 points**

Add a comment above each code line below explaining what the code line does or why
that code line is necessary.

Keep each comment to less than 2 lines per line of code and < 80 chars per line.

``` r
annotations <- read_tsv("ftp://ftp.ebi.ac.uk/pub/databases/genenames/new/tsv/locus_groups/protein-coding_gene.txt") %>% 
  select(ensembl_gene_id, symbol, name, gene_family, ccds_id) %>% 
  filter(!is.na(ccds_id)) %>% 
  print()
```

``` r
data <- read_tsv("ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE89nnn/GSE89183/suppl/GSE89183_Counts.txt.gz") %>% 
  rename(ensembl_gene_id = `ENSEMBL gene`) %>%
  filter_if(is.numeric, all_vars(. > 50)) %>% 
  mutate_if(is.numeric, funs(. / median(.))) %>% 
  print()
```

## Problem 3

**15 points**

Correct errors in the code below.

``` r
lfc <- data %>% 
  mutate(mean_rpl5_te = ((CD34_shRPL5_RPF_1 + CD34_shRPL5_RPF_2) / 
                            (CD34_shRPL5_RNA_1 + CD34_shRPL5_RNA_2)) %>% 
  mutate(mean_rps19_te = ((CD34_shRPS19_RPF_1 + CD34_shRPS19_RPF_2) / 
                            (CD34_shRPS19_RNA_1 + CD34_shRPS19_RNA_2)) %>% 
  mutate(mean_shluc_te = ((CD34_shLuc_RPF_1 + CD34_shLuc_RPF_2) / 
                            (CD34_shLuc_RNA_1 + CD34_shLuc_RNA_2)) %>% 
  select(ensembl_gene_id, mean_rpl5_te, mean_rps19_te) %>% 
  mutate(lfc_te_rpl5 == log2(mean_rpl5_te / mean_shluc_te),
         lfc_te_rps19 == log2(mean_rps19_te / mean_shluc_te)) %>% 
  print()
```

## Problem 4

**10 points**

Plot the log2 fold changes for the two different knockdowns of ribosomal proteins
RPL5 and RPS19 as a scatter plot against each other.

Set the two axis to have the same limits.

Set the figure to be approximately square.


## Problem 5

**10 points**

Create a new dataframe called `mean_lfc` from `lfc` 
containing a new column called `avg_lfc`.
`avg_lfc` should be the average of the log2 fold-change in TE (`lfc_te`) upon
knockdown of RPL5 and RPS19.

Then select only the gene id column and the new column that you just created.


## Problem 6

**10 points**

Join the above `mean_lfc` dataframe with the `annotations` dataframe created
at the top of the document.


## Problem 7

**10 points**

Select only the bottom 10 genes with the lowest `avg_lfc` and display the
gene `symbol`, gene `name` and `avg_lfc` for these genes.

5 bonus points if you use one of the new functions you learned above!


## Problem 8

**10 points**

Select only the top 10 genes with the highest `avg_lfc` and display the 
gene `symbol`, gene `name` and `avg_lfc` for these genes.

5 bonus points if you use another new function you learned above!


## Problem 9

**10 points**

1. Which is the paper from which this dataset is taken?

2. Which figure in the paper is the closest to your analysis in Problem 7?
