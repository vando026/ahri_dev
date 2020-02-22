# Getting started with the ahri library

This document serves as a short introduction to the ahri library. For
this library to work, you will need to request five default datasets
from the AHRI Data Centre. These default datasets are called:

  - RD05-99 ACDIS HIV All.dta

  - SurveillanceEpisodesBasicAgeYrHIV.dta

  - RD03-99 ACDIS WGH ALL.dta

  - RD04-99 ACDIS MGH ALL.dta

  - RD01-03 ACDIS BoundedStructures.dta

  - RD06-99 ACDIS HSE-H All.dta

Put all these default .dta (Stata) datasets in the same folder. For
example, my path to this folder is: `C:\Users\alainv\AHRI_Data`.

## Install the library

The ahri library is in development, which you can download from GitHub.
First, install the `remotes` package. Second, ask me for the GitHub
token.

``` r
library(remotes)
install_github('vando026/ahri', ref="ahri_dev", auth_token="secret_token")
library(ahri)
```

## Setting the file paths

The first thing to do set the file paths to the AHRI datasets. We use
the `setFiles` function for this and assign it to the name `getFiles`.
The `getFiles` name is required.

``` r
getFiles <- setFiles(folder="C:/Users/alainv/AHRI_Data")
```

You can inspect the file paths by printing them out:

``` r
getFiles()[1:5]
$hivfile
[1] "C:/Users/alainv/AHRI_Data/RD05-99 ACDIS HIV All.dta"

$epi_dta
[1] "C:/Users/alainv/AHRI_Data/SurveillanceEpisodesBasicAgeYrHIV.dta"

$wghfile
[1] "C:/Users/alainv/AHRI_Data/RD03-99 ACDIS WGH ALL.dta"

$mghfile
[1] "C:/Users/alainv/AHRI_Data/RD04-99 ACDIS MGH ALL.dta"

$bsifile
[1] "C:/Users/alainv/AHRI_Data/RD01-03 ACDIS BoundedStructures.dta"
```

If you ommit the folder argument then `setFiles()` will bring up a
graphical dialogue box where you can point and click your way to the
folder.

## Reading in data

Reading in some of the large AHRI datasets can take time. Two useful
functions for speeding up this process are: `readHIVData` and
`readEpisodes`. These read in the corresponding default .dta files, do
some data processing, harmonize variable names for future merging, and
save as corresponding .Rda datasets.

The `readHIVData` reads in the HIV surveillance dataset for all years,
drops indeterminate HIV tests, and drops/keeps any HIV tests from the
TasP study area. TasP or Northern PIPSA areas were added to the datasets
from 2017 onwards. If you want to include the TasP study areas then set
`dropTasP=FALSE`.

``` r
readHIV <- readHIVData(dropTasP=TRUE)
```

You need only to run the `readHIVData` function once, which will save an
.Rda dataset to the same folder with your .dta datasets. (However, if
you download an updated HIV surveillance to the default folder, then you
will need to run it again.) Thereafter, you can run `getHIV` to load in
the data quickly and repeatedly.

``` r
hiv <- getHIV()
hiv
# A tibble: 159,471 x 10
   IIntID BSIntID VisitDate  HIVResult Female   Age PIPSA          HIVNegative HIVPositive  Year
    <int>   <int> <date>         <dbl>  <int> <dbl> <chr>          <date>      <date>      <int>
 1     16    2830 2009-06-02         0      1    56 Southern PIPSA 2009-06-02  NA           2009
 2     16    2830 2011-05-23         0      1    58 Southern PIPSA 2011-05-23  NA           2011
 3     17    9274 2004-02-09         0      1    35 Southern PIPSA 2004-02-09  NA           2004
 4     17    9274 2005-06-02         0      1    36 Southern PIPSA 2005-06-02  NA           2005
 5     17    9274 2006-06-01         0      1    37 Southern PIPSA 2006-06-01  NA           2006
 6     17    9274 2010-08-19         0      1    42 Southern PIPSA 2010-08-19  NA           2010
 7     17    9274 2011-08-06         0      1    43 Southern PIPSA 2011-08-06  NA           2011
 8     17   18272 2017-10-17         1      1    49 Southern PIPSA NA          2017-10-17   2017
 9     17   18272 2018-10-03         1      1    50 Southern PIPSA NA          2018-10-03   2018
10     19    7923 2009-08-04         0      1    69 Southern PIPSA 2009-08-04  NA           2009
# ... with 159,461 more rows
```

Similarly with the large Episodes .Rda dataset, you run `readEpisodes`
once, and load its corresponding .Rda data file with `getEpisodes`.

``` r
readEpi <- readEpisodes()
```

``` r
epidat <- getEpisodes()
epidat
# A tibble: 4,063,867 x 17
   IIntID BSIntID Female   Age DoB        DoD         Year ExpDays ObservationStart ObservationEnd InMigration OutMigration Resident
    <int>   <int>  <int> <dbl> <date>     <date>     <int>   <dbl> <date>           <date>           <dbl+lbl>    <dbl+lbl> <dbl+lb>
 1     11    2830      0    54 1945-01-10 2004-12-12  2000       9 2000-01-01       2000-01-09               0            0        1
 2     11    2830      0    55 1945-01-10 2004-12-12  2000     357 2000-01-10       2000-12-31               0            0        1
 3     11    2830      0    55 1945-01-10 2004-12-12  2001       9 2001-01-01       2001-01-09               0            0        1
 4     11    2830      0    56 1945-01-10 2004-12-12  2001     356 2001-01-10       2001-12-31               0            0        1
 5     11    2830      0    56 1945-01-10 2004-12-12  2002       9 2002-01-01       2002-01-09               0            0        1
 6     11    2830      0    57 1945-01-10 2004-12-12  2002     356 2002-01-10       2002-12-31               0            0        1
 7     11    2830      0    57 1945-01-10 2004-12-12  2003       9 2003-01-01       2003-01-09               0            0        1
 8     11    2830      0    58 1945-01-10 2004-12-12  2003     356 2003-01-10       2003-12-31               0            0        1
 9     11    2830      0    58 1945-01-10 2004-12-12  2004       9 2004-01-01       2004-01-09               0            0        1
10     11    2830      0    59 1945-01-10 2004-12-12  2004     338 2004-01-10       2004-12-12               0            0        1
# ... with 4,063,857 more rows, and 4 more variables: OnART <dbl+lbl>, ARTInitiation <dbl>, EarliestARTInitDate <date>, PIPSA <chr>
```

In my experience, the Episodes dataset is a bit overwhelming, so by
default the `readEpisodes` keeps only variables related to migration,
for which this dataset is best suited. You can however include addtional
variables using the `Vars` argument. For example, let’s say you are
intersted in adding the ART variables, then:

``` r
# The Rda file is here
readEpi <- readEpisodes(Vars="OnART|ARTInitiation|EarliestARTInitDate")
```

Behind the hood, the `Vars` argument is a regular expression, hence
variable names must be separated as shown above. However, since “ART” is
the common substring, you could achieve the same outcome using:

``` r
readEpi <- readEpisodes(Vars="ART")
```

You can see the file paths and names of the .Rda datasets as follows:

``` r
getFiles()$hiv_rda
[1] "C:/Users/alainv/AHRI_Data/ACDIS_HIV_All.Rda"
getFiles()$epi_rda
[1] "C:/Users/alainv/AHRI_Data/SurveillanceEpisodesBasicAgeYrHIV.Rda"
```

# Setting the data according to paramaters

A useful function for setting the data is `setArgs`. The `setArgs`
function can set the data in a number of ways, for example, by year,
sex, and age. Let’s see how this works with the HIV surveillance
dataset, where we select years 2010 to 2018 for men and women aged 14 to
49 years.

``` r
Args <- setArgs(Years=c(2010:2018), 
  Age=list(All=c(15, 49)))
hiv <- setHIV(Args)
table(hiv$Year)

 2010  2011  2012  2013  2014  2015  2016  2017  2018 
 7978  7219  5430  6902  6473  9115 10348  7544  7833 
table(hiv$Female)

    0     1 
23447 45395 
summary(hiv$Age)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   15.0    18.0    25.0    27.3    35.0    49.0 
```

We restrict the data to men aged 15 to 25 years as follows.

``` r
Args <- setArgs(Years=c(2010:2018), 
  Age=list(Mal=c(15, 25)))
hiv <- setHIV(Args)
table(hiv$Year)

2010 2011 2012 2013 2014 2015 2016 2017 2018 
1834 1633 1279 1674 1456 2040 2190 1561 1624 
table(hiv$Female)

    0 
15291 
summary(hiv$Age)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  15.00   17.00   18.00   18.81   21.00   25.00 
```

We can set different ages for men and women:

``` r
Args <- setArgs(Years=c(2010:2018), 
  Age=list(Mal=c(15, 25), Fem=c(15, 35)))
hiv <- setHIV(Args)
#
summary(hiv$Age[hiv$Female==1])
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  15.00   18.00   22.00   23.31   28.00   35.00 
#
summary(hiv$Age[hiv$Female==0])
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  15.00   17.00   18.00   18.81   21.00   25.00 
```

We can also use the `setArgs` function for the Episodes dataset.

``` r
Args <- setArgs(Years=c(2005:2018),
  Age=list(All=c(25, 40)))
epi <- setEpisodes(Args)
#
summary(epi$Age)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  25.00   27.00   31.00   31.31   35.00   40.00 
```

You can actually set the years, age, and sex for any dataset, providing
it has the required variables. To subset by Age by Sex, the dataset must
have an `Age` and `Female` variable. The `setAge` function does this:

``` r
hiv <- getHIV()
Args <- setArgs(Age=list(Fem=c(15, 19), Mal=c(15, 25)))
newhiv <- setAge(hiv, Args)
#
summary(newhiv$Age[hiv$Female==1])
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  15.00   16.00   18.00   18.13   19.00   25.00   74158 
#
summary(newhiv$Age[hiv$Female==0])
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  15.00   16.00   18.00   18.12   19.00   25.00   36174 
```

Finally, these is a dedicated function called `setData` for data with
episodes. You may want to get the age of the participant in a given
episode, or you may want to drop all episodes outside an age range.

``` r
Args <- setArgs(Age=list(All=c(15, 19)))
epi <- setEpisodes(Args)
epi1 <- setData(epi, Args)
#
summary(epi1$Age)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  15.00   16.00   17.00   17.01   18.00   19.00 
```

Let’s pretend you just created a dataset that doesnt have age by
dataset, and all you have is the start or end date of the episode, and
the date of birth. You want to get the age, and subset by age, then
`setData` becomes handy. First, we can get the birth date of the
participant with the `getBirthDate` function. Then we pass that data to
an argument in
`setData`.

``` r
epi2 <- select(epi, IIntID, Year, Female, ObservationStart, ObservationEnd)
epi2
# A tibble: 378,549 x 5
   IIntID  Year Female ObservationStart ObservationEnd
    <int> <int>  <int> <date>           <date>        
 1     21  2005      0 2005-05-19       2005-12-31    
 2     21  2006      0 2006-01-01       2006-04-27    
 3     21  2006      0 2006-04-28       2006-12-31    
 4     21  2007      0 2007-01-01       2007-04-10    
 5     29  2009      0 2009-01-01       2009-09-05    
 6     29  2009      0 2009-09-06       2009-12-31    
 7     29  2010      0 2010-01-01       2010-09-05    
 8     29  2010      0 2010-09-06       2010-12-31    
 9     29  2011      0 2011-01-01       2011-06-01    
10     29  2011      0 2011-06-02       2011-09-05    
# ... with 378,539 more rows
# Get the birthdates
bdat <- getBirthDate()
bdat
# A tibble: 174,293 x 2
   IIntID DateOfBirth
    <int> <date>     
 1     11 1945-01-10 
 2     12 1940-12-26 
 3     13 1961-03-07 
 4     14 1974-12-24 
 5     15 1977-08-05 
 6     16 1952-11-16 
 7     17 1968-06-05 
 8     18 1979-04-05 
 9     19 1939-08-18 
10     20 1973-06-25 
# ... with 174,283 more rows
# Keep only episodes where ID is 15-19
Args <- setArgs(Age=list(All=c(15, 19)))
# Make age at the start of the episode
epi2 <- setData(epi2, Args, time2="ObservationStart", birthdate=bdat)
```

Finally, you can pass on a custom function to `setArgs` to do further
data processing. For example, let’s drop participants according to some
criteria defined in the custom function.

``` r
dropID <- function(dat) {
  dat <- mutate(dat, Val = rbinom(nrow(dat), 1, 0.4))
  filter(dat, Val==1)
}
Args <- setArgs(Age=list(All=c(15, 19)), setFun=dropID)
epi2 <- setData(epi2, Args, time2="ObservationStart", birthdate=bdat)
epi2
# A tibble: 151,596 x 8
   IIntID  Year Female ObservationStart ObservationEnd   Age AgeCat    Val
    <int> <int>  <int> <date>           <date>         <dbl> <fct>   <int>
 1     21  2006      0 2006-01-01       2006-04-27        15 [15,20)     1
 2     21  2006      0 2006-04-28       2006-12-31        15 [15,20)     1
 3     21  2007      0 2007-01-01       2007-04-10        16 [15,20)     1
 4     29  2009      0 2009-01-01       2009-09-05        15 [15,20)     1
 5     29  2010      0 2010-01-01       2010-09-05        16 [15,20)     1
 6     29  2011      0 2011-01-01       2011-06-01        17 [15,20)     1
 7     29  2011      0 2011-06-02       2011-09-05        17 [15,20)     1
 8     29  2012      0 2012-01-01       2012-09-05        18 [15,20)     1
 9     29  2012      0 2012-09-06       2012-12-31        18 [15,20)     1
10     29  2013      0 2013-01-01       2013-03-22        19 [15,20)     1
# ... with 151,586 more rows
```

This becomes useful when you have to process data in the \(i\)th
iteration of \(i = 1,\dots,K\) iterations.
