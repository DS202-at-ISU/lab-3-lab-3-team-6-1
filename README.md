
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

> Of the nine Avengers we see on screen - Iron an, Hulk, Captain
> America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver, and
> The Vision - every single one of them has died at least once in the
> course of their time Avenging in the comics. In fact, Hawkey died
> twice!

### Include code

``` r
# Load necessary libraries
library(dplyr)
library(tidyr)
library(readr)

# Load the data
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)

# Preview the data
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
# Reshape the Death columns into Time and Death
deaths <- av %>%
  select(Name.Alias, Death1:Death5) %>%
  gather(key = "DeathEvent", value = "Death", Death1:Death5) %>%
  mutate(Time = parse_number(DeathEvent)) %>%
  select(-DeathEvent)  # Remove the original DeathEvent column

# View the reshaped deaths data
head(deaths)
```

    ##                    Name.Alias Death Time
    ## 1   Henry Jonathan "Hank" Pym   YES    1
    ## 2              Janet van Dyne   YES    1
    ## 3 Anthony Edward "Tony" Stark   YES    1
    ## 4         Robert Bruce Banner   YES    1
    ## 5                Thor Odinson   YES    1
    ## 6      Richard Milhouse Jones    NO    1

``` r
# Reshape the Return columns into Time and Return
returns <- av %>%
  select(Name.Alias, Return1:Return5) %>%
  gather(key = "ReturnEvent", value = "Return", Return1:Return5) %>%
  mutate(Time = parse_number(ReturnEvent)) %>%
  select(-ReturnEvent)  # Remove the original ReturnEvent column

# View the reshaped returns data
head(returns)
```

    ##                    Name.Alias Return Time
    ## 1   Henry Jonathan "Hank" Pym     NO    1
    ## 2              Janet van Dyne    YES    1
    ## 3 Anthony Edward "Tony" Stark    YES    1
    ## 4         Robert Bruce Banner    YES    1
    ## 5                Thor Odinson    YES    1
    ## 6      Richard Milhouse Jones           1

``` r
# Calculate the number of deaths per Avenger
average_deaths <- deaths %>%
  filter(Death == "YES") %>%  # Ensure that Death values are case-sensitive; change if necessary
  group_by(Name.Alias) %>%
  summarise(death_count = n()) %>%
  summarise(average_death = mean(death_count, na.rm = TRUE))

# Display the average number of deaths
average_deaths
```

    ## # A tibble: 1 × 1
    ##   average_death
    ##           <dbl>
    ## 1          2.27

``` r
# Check if any Avenger has more than one death
multiple_deaths <- deaths %>%
  filter(Death == "YES") %>%
  group_by(Name.Alias) %>%
  summarise(death_count = n()) %>%
  filter(death_count > 1)

# Display Avengers who died more than once
multiple_deaths
```

    ## # A tibble: 45 × 2
    ##    Name.Alias                      death_count
    ##    <chr>                                 <int>
    ##  1 ""                                        9
    ##  2 "Anthony Edward \"Tony\" Stark"           2
    ##  3 "Anthony Ludgate Druid"                   4
    ##  4 "Ares"                                    3
    ##  5 "Barbara Barton (nee Morse)"              2
    ##  6 "Benjamin Jacob Grimm"                    2
    ##  7 "Brandt"                                  2
    ##  8 "Cassie Lang"                             2
    ##  9 "Clinton Francis Barton"                  4
    ## 10 "DeMarr Davis"                            2
    ## # ℹ 35 more rows

### FiveThirtyEight Statement

Some Avengers have died multiple times, highlighting the high-risk lives
they lead.

### Include your answer

The statement is True! All of the listed Avengers have died at least
once, and Hawkeye died twice.

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
