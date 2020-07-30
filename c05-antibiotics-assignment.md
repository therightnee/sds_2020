Antibiotics
================
(Your name here)
2020-

  - [Grading Rubric](#grading-rubric)
      - [Individual](#individual)
      - [Team](#team)
      - [Due Date](#due-date)
  - [Visualization](#visualization)
      - [Purpose: Compare Effectiveness](#purpose-compare-effectiveness)
      - [Purpose: Categorize Bacteria](#purpose-categorize-bacteria)
  - [References](#references)

*Purpose*: To create an effective visualization, we need to keep our
*purpose* firmly in mind. There are many different ways to visualize
data, and the only way we can judge efficacy is with respect to our
purpose.

In this challenge you’ll visualize the same data in two different ways,
aimed at two different purposes.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table\!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category    | Unsatisfactory                                                                   | Satisfactory                                                               |
| ----------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Effort      | Some task **q**’s left unattempted                                               | All task **q**’s attempted                                                 |
| Observed    | Did not document observations                                                    | Documented observations based on analysis                                  |
| Supported   | Some observations not supported by analysis                                      | All observations supported by analysis (table, graph, etc.)                |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Team

<!-- ------------------------- -->

| Category   | Unsatisfactory                                                                                   | Satisfactory                                       |
| ---------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| Documented | No team contributions to Wiki                                                                    | Team contributed to Wiki                           |
| Referenced | No team references in Wiki                                                                       | At least one reference in Wiki to member report(s) |
| Relevant   | References unrelated to assertion, or difficult to find related analysis based on reference text | Reference text clearly points to relevant analysis |

## Due Date

<!-- ------------------------- -->

All the deliverables stated in the rubrics above are due on the day of
the class discussion of that exercise. See the
[Syllabus](https://docs.google.com/document/d/1jJTh2DH8nVJd2eyMMoyNGroReo0BKcJrz1eONi3rPSc/edit?usp=sharing)
for more information.

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.0
    ## ✓ tidyr   1.1.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ───────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(ggrepel)
library(stringr)
library(wesanderson)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Parsed with column specification:
    ## cols(
    ##   bacteria = col_character(),
    ##   penicillin = col_double(),
    ##   streptomycin = col_double(),
    ##   neomycin = col_double(),
    ##   gram = col_character()
    ## )

``` r
glimpse(df_antibiotics)
```

    ## Rows: 16
    ## Columns: 5
    ## $ bacteria     <chr> "Aerobacter aerogenes", "Brucella abortus", "Bacillus an…
    ## $ penicillin   <dbl> 870.000, 1.000, 0.001, 0.005, 100.000, 850.000, 800.000,…
    ## $ streptomycin <dbl> 1.00, 2.00, 0.01, 11.00, 0.40, 1.20, 5.00, 0.10, 2.00, 0…
    ## $ neomycin     <dbl> 1.600, 0.020, 0.007, 10.000, 0.100, 1.000, 2.000, 0.100,…
    ## $ gram         <chr> "negative", "negative", "positive", "positive", "negativ…

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
| :------------------------------ | ---------: | -----------: | -------: | :------- |
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

## Purpose: Compare Effectiveness

<!-- ------------------------- -->

**q1** Create a visualization of `df_antibiotics` that helps you to
compare the effectiveness of the three antibiotics across all the
bacteria reported. Can you make any broad statements about antibiotic
effectiveness?

``` r
## TASK: Create your visualization
df_antibiotics %>% 
  filter(penicillin <= 0.1) %>%
  ggplot(aes(x = bacteria, y = penicillin, color = gram)) +
  geom_point(size = 3) +
  geom_text(aes(label=penicillin), nudge_y = 0.002) +
  scale_x_discrete(labels = function(bacteria) str_wrap(bacteria, width = 10)) + 
  scale_color_manual(values=c("#999999", "#E69F00"))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1-task-1.png)<!-- -->

``` r
df_antibiotics %>% 
  filter(streptomycin <= 0.1) %>%
  ggplot(aes(x = bacteria, y = streptomycin, color = gram)) +
  geom_point(size = 3) +
  geom_text(aes(label=streptomycin), nudge_y = 0.005) +
  scale_x_discrete(labels = function(bacteria) str_wrap(bacteria, width = 10)) + 
  scale_color_manual(values=c("#999999", "#E69F00"))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1-task-2.png)<!-- -->

``` r
df_antibiotics %>% 
  filter(neomycin < 0.1) %>%
  ggplot(aes(x = bacteria, y = neomycin, color = gram)) +
  geom_point(size = 3) + 
  geom_text(aes(label=neomycin), nudge_y = 0.005) +
  scale_x_discrete(labels = function(bacteria) str_wrap(bacteria, width = 10)) + 
  scale_color_manual(values=c("#999999", "#E69F00"))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1-task-3.png)<!-- -->

``` r
###list the count for ineffective responses###
```

**Observations**:

  - Record your observations here
  - Can you make any broad statements about antibiotic effectiveness?

## Purpose: Categorize Bacteria

<!-- ------------------------- -->

The *genus* of a living organism is a human categorization, based on
various characteristics of the organism. Since these categories are
based on numerous factors, we will tend to see clusters if we visualize
data according to relevant variables. We can use these visuals to
categorize observations, and to question whether given categories are
reasonable\!

**q2** Create a visualization of `df_antibiotics` that helps you to
categorize bacteria according to the variables in the data. Document
your observations on how how clusters of bacteria in the variables do—or
don’t—align with their *genus* classification.

``` r
## TASK: Create your visualization

df_genus <-
  df_antibiotics %>%
    separate(bacteria, c("genus", "family"))
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [10].

``` r
df_genus %>% 
  group_by(genus) %>%
  tally()
```

    ## # A tibble: 12 x 2
    ##    genus              n
    ##    <chr>          <int>
    ##  1 Aerobacter         1
    ##  2 Bacillus           1
    ##  3 Brucella           1
    ##  4 Diplococcus        1
    ##  5 Escherichia        1
    ##  6 Klebsiella         1
    ##  7 Mycobacterium      1
    ##  8 Proteus            1
    ##  9 Pseudomonas        1
    ## 10 Salmonella         2
    ## 11 Staphylococcus     2
    ## 12 Streptococcus      3

``` r
df_genus %>%
  filter(neomycin > 1 | penicillin > 1 | streptomycin > 1) %>%
  group_by(genus)
```

    ## # A tibble: 11 x 6
    ## # Groups:   genus [10]
    ##    genus         family         penicillin streptomycin neomycin gram    
    ##    <chr>         <chr>               <dbl>        <dbl>    <dbl> <chr>   
    ##  1 Aerobacter    aerogenes         870              1       1.6  negative
    ##  2 Brucella      abortus             1              2       0.02 negative
    ##  3 Diplococcus   pneumonia           0.005         11      10    positive
    ##  4 Escherichia   coli              100              0.4     0.1  negative
    ##  5 Klebsiella    pneumoniae        850              1.2     1    negative
    ##  6 Mycobacterium tuberculosis      800              5       2    negative
    ##  7 Proteus       vulgaris            3              0.1     0.1  negative
    ##  8 Pseudomonas   aeruginosa        850              2       0.4  negative
    ##  9 Salmonella    schottmuelleri     10              0.8     0.09 negative
    ## 10 Streptococcus hemolyticus         0.001         14      10    positive
    ## 11 Streptococcus viridans            0.005         10      40    positive

``` r
df_genus %>%
  filter(neomycin < 1 & penicillin < 1 & streptomycin < 1) %>%
  group_by(genus)
```

    ## # A tibble: 3 x 6
    ## # Groups:   genus [2]
    ##   genus          family    penicillin streptomycin neomycin gram    
    ##   <chr>          <chr>          <dbl>        <dbl>    <dbl> <chr>   
    ## 1 Bacillus       anthracis      0.001         0.01    0.007 positive
    ## 2 Staphylococcus albus          0.007         0.1     0.001 positive
    ## 3 Staphylococcus aureus         0.03          0.03    0.001 positive

**Observations**:

  - Document your observations here\!

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” /American Scientist/ (2009)
[link](https://www.americanscientist.org/article/thats-funny)
