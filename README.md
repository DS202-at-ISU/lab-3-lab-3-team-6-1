
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

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
#Simeon
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
library(readr)
deaths <- av %>%
  pivot_longer(cols = starts_with("Death"), 
               names_to = "Time", 
               values_to = "Death") %>%
  mutate(Time = parse_number(Time))
head(deaths)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>

``` r
returns <- av %>%
  pivot_longer(cols = starts_with("Return"), 
               names_to = "Time", 
               values_to = "Return") %>%
  mutate(Time = parse_number(Time))
head(returns)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Death2 <chr>,
    ## #   Death3 <chr>, Death4 <chr>, Death5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Return <chr>

``` r
average_deaths <- deaths %>%
  filter(Death == "YES") %>%
  group_by(Name.Alias) %>%
  summarise(death_count = n()) %>%
  summarise(avg_deaths = mean(death_count, na.rm = TRUE))
average_deaths
```

    ## # A tibble: 1 × 1
    ##   avg_deaths
    ##        <dbl>
    ## 1       1.39

## Simeon

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team.5

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
library(dplyr)

avengers_died <- deaths %>%
  filter(Death == "YES") %>%
  distinct(Name.Alias)

num_avengers_died <- nrow(avengers_died)

num_avengers_died
```

    ## [1] 64

### Include your answer

“Our analysis found 64 Avengers who died at least once after joining the
team, suggesting that FiveThirtyEight’s statement may need adjustment.”

## Adam Grimm

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Of the nine Avengers we see on screen - Iron an, Hulk, Captain
> America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver, and
> The Vision - every single one of them has died at least once in the
> course of their time Avenging in the comics. In fact, Hawkey died
> twice!

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
screen_avengers <- c("Anthony Edward \"Tony\" Stark", "Robert Bruce Banner", "Steve Rogers",
                     "Thor Odinson", "Clinton Francis Barton", "Natalia Alianovna Romanova",
                     "Wanda Maximoff", "Pietro Maximoff", "The Vision")


deaths <- av %>%
  select(Name.Alias, starts_with("Death")) %>%
  pivot_longer(cols = starts_with("Death"),
               names_to = "Time", 
               values_to = "Death") %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Name.Alias %in% screen_avengers, Death == "YES")


screen_avenger_deaths <- deaths %>%
  group_by(Name.Alias) %>%
  summarise(death_count = n())


screen_avenger_deaths
```

    ## # A tibble: 7 × 2
    ##   Name.Alias                      death_count
    ##   <chr>                                 <int>
    ## 1 "Anthony Edward \"Tony\" Stark"           1
    ## 2 "Clinton Francis Barton"                  2
    ## 3 "Natalia Alianovna Romanova"              1
    ## 4 "Pietro Maximoff"                         1
    ## 5 "Robert Bruce Banner"                     1
    ## 6 "Thor Odinson"                            2
    ## 7 "Wanda Maximoff"                          1

## Bach Nguyen

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> I counted 89 total deaths — some unlucky Avengers7 are basically Meat
> Loaf with an E-ZPass — and on 57 occasions the individual made a
> comeback.

### Include code

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ purrr     1.0.2
    ## ✔ ggplot2   3.5.1     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)

deaths <- av %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    names_prefix = "Death",
    names_transform = list(Time = parse_number),
    values_to = "Death"
  ) %>%
  mutate(
    Death = case_when(
      Death == "YES" ~ "yes",
      Death == "NO"  ~ "no",
      TRUE           ~ ""
    )
  )

returns <- av %>%
  pivot_longer(
    cols = starts_with("Return"),
    names_to = "Time",
    names_prefix = "Return",
    names_transform = list(Time = parse_number),
    values_to = "Return"
  ) %>%
  mutate(
    Return = case_when(
      Return == "YES" ~ "yes",
      Return == "NO"  ~ "no",
      TRUE            ~ ""
    )
  )


deaths_summary <- deaths %>%
  group_by(Name.Alias) %>%
  summarize(Total_Deaths = sum(Death == "yes", na.rm = TRUE))


print(deaths_summary)
```

    ## # A tibble: 163 × 2
    ##    Name.Alias              Total_Deaths
    ##    <chr>                          <int>
    ##  1 ""                                 7
    ##  2 "\"Giulietta Nefaria\""            1
    ##  3 "Adam"                             1
    ##  4 "Adam Brashear"                    0
    ##  5 "Alani Ryan"                       0
    ##  6 "Alex Summers"                     0
    ##  7 "Alexis"                           0
    ##  8 "Alias: Jonas"                     1
    ##  9 "Amadeus Cho"                      0
    ## 10 "America Chavez"                   0
    ## # ℹ 153 more rows

``` r
average_deaths <- mean(deaths_summary$Total_Deaths)


cat("On average, an Avenger suffers", round(average_deaths, 2), "deaths.\n")
```

    ## On average, an Avenger suffers 0.55 deaths.

``` r
total_deaths <- av %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  filter(Death == "YES") %>%
  tally(name = "Total_Deaths")

print(total_deaths)
```

    ## # A tibble: 1 × 1
    ##   Total_Deaths
    ##          <int>
    ## 1           89

### Include your answer

The statement from FiveThirtyEight is correct. My answer is the same.

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
