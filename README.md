
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
av <- av %>% rowwise() %>% mutate(Death_Time = sum(c(Death1, Death2, Death3, Death4, Death5) == "YES"))
av <- av %>% rowwise() %>% mutate(Death = Death_Time > 0) %>% select(-c(Death1, Death2, Death3, Death4, Death5))
av$Death <- ifelse(av$Death == TRUE, "yes", "no")
av
```

    ## # A tibble: 173 × 18
    ## # Rowwise: 
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  3 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Richard …         612 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Steven R…        3458 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Pietro M…         769 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Wanda Ma…        1214 YES      FEMALE ""                 
    ## # ℹ 163 more rows
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Death_Time <int>,
    ## #   Death <chr>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

``` r
av <- av %>% rowwise() %>% mutate(Returns_Time = sum(c(Return1, Return2, Return3, Return4, Return5) == "YES"))
av <- av %>% rowwise() %>% mutate(Returns = Returns_Time > 0) %>% select(-c(Return1, Return2, Return3, Return4, Return5))
av$Returns <- ifelse(av$Returns == TRUE, "yes", "no")
av
```

    ## # A tibble: 173 × 15
    ## # Rowwise: 
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  3 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Richard …         612 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Steven R…        3458 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Pietro M…         769 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Wanda Ma…        1214 YES      FEMALE ""                 
    ## # ℹ 163 more rows
    ## # ℹ 9 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Notes <chr>, Death_Time <int>,
    ## #   Death <chr>, Returns_Time <int>, Returns <chr>

Based on these datasets calculate the average number of deaths an
Avenger suffers.

The average number of avenger deaths is 0.5144509.

``` r
mean(av$Death_Time)
```

    ## [1] 0.5144509

## Individually

## Blake

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team.

### Include the code

``` r
avengers.with.death = nrow(av[av$Death == "yes",])
avengers.with.death
```

    ## [1] 69

### Include your answer

We can see that the number of rows with at least one death is the same
as the number that the author found in his analysis.

## Grace

### FiveThirtyEight Statement

> I counted 89 total deaths — some unlucky Avengers7 are basically Meat
> Loaf with an E-ZPass — and on 57 occasions the individual made a
> comeback.

``` r
sum(av$Death_Time)
```

    ## [1] 89

``` r
sum(av$Returns_Time)
```

    ## [1] 57

The code shows that the total number of times the avengers have died is
indeed 89. The article is also correct claiming that there are 57
occasions of the avenger making a comeback.

## Nick

### FiveThirtyEight Statement

> There’s a 2-in-3 chance that a member of the Avengers returned from
> their first stint in the afterlife, but only a 50 percent chance they
> recovered from a second or third death.

### Included code

``` r
# number of 1st returns divided by number of 2nd deaths
 sum(nrow(av[av$Returns_Time >= 1, "Returns_Time"])) / sum(nrow(av[av$Death_Time >= 1, "Death_Time"]))
```

    ## [1] 0.6666667

``` r
# number of 2nd returns divided by number of 2nd deaths
 sum(nrow(av[av$Returns_Time >= 2, "Returns_Time"])) / sum(nrow(av[av$Death_Time >= 2, "Death_Time"]))
```

    ## [1] 0.5

``` r
# number of 3rd returns divided by number of 3rd deaths
 sum(nrow(av[av$Returns_Time >= 3, "Returns_Time"])) / sum(nrow(av[av$Death_Time >= 3, "Death_Time"]))
```

    ## [1] 0.5

``` r
# number of 3rd deaths
 nrow(av[av$Death_Time >= 3, "Death_Time"])
```

    ## [1] 2

### Our Analysis:

We find that the author was correct on all counts, though there are only
two characters which have died 3 times and one of them made a comeback
(Jocasta).
