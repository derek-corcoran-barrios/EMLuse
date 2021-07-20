How to use Ecological Metadata Language (EML)
================

## Test using the EML package

### First create an attribute metadatra

Create a table with the column names (attributeName) of a csv file (or
similar) and fill in some other columns, the following table is an
example

``` r
library(EML)

attributes <-tibble::tribble(
~attributeName, ~attributeDefinition,                                                 ~formatString, ~definition,        ~unit,   ~numberType,
  "run.num",    "which run number (=block). Range: 1 - 6. (integer)",                 NA,            "which run number", NA,       NA,
  "year",       "year, 2012",                                                         "YYYY",        NA,                 NA,       NA,
  "day",        "Julian day. Range: 170 - 209.",                                      "DDD",         NA,                 NA,       NA,
  "hour.min",   "hour and minute of observation. Range 1 - 2400 (integer)",           "hhmm",        NA,                 NA,       NA,
  "i.flag",     "is variable Real, Interpolated or Bad (character/factor)",           NA,            NA,                 NA,       NA,
  "variable",   "what variable being measured in what treatment (character/factor).", NA,            NA,                 NA,       NA,
  "value.i",    "value of measured variable for run.num on year/day/hour.min.",       NA,            NA,                 NA,       NA,
  "length",    "length of the species in meters (dummy example of numeric data)",     NA,            NA,                 "meter",  "real")
```

| attributeName | attributeDefinition                                                | formatString | definition       | unit  | numberType |
|:--------------|:-------------------------------------------------------------------|:-------------|:-----------------|:------|:-----------|
| run.num       | which run number (=block). Range: 1 - 6. (integer)                 | NA           | which run number | NA    | NA         |
| year          | year, 2012                                                         | YYYY         | NA               | NA    | NA         |
| day           | Julian day. Range: 170 - 209.                                      | DDD          | NA               | NA    | NA         |
| hour.min      | hour and minute of observation. Range 1 - 2400 (integer)           | hhmm         | NA               | NA    | NA         |
| i.flag        | is variable Real, Interpolated or Bad (character/factor)           | NA           | NA               | NA    | NA         |
| variable      | what variable being measured in what treatment (character/factor). | NA           | NA               | NA    | NA         |
| value.i       | value of measured variable for run.num on year/day/hour.min.       | NA           | NA               | NA    | NA         |
| length        | length of the species in meters (dummy example of numeric data)    | NA           | NA               | meter | real       |

### then generate a key for factors

``` r
i.flag <- c(R = "real",
            I = "interpolated",
            B = "bad")
variable <- c(
    control  = "no prey added",
    low      = "0.125 mg prey added ml-1 d-1",
    med.low  = "0,25 mg prey added ml-1 d-1",
    med.high = "0.5 mg prey added ml-1 d-1",
    high     = "1.0 mg prey added ml-1 d-1",
    air.temp = "air temperature measured just above all plants (1 thermocouple)",
    water.temp = "water temperature measured within each pitcher",
    par       = "photosynthetic active radiation (PAR) measured just above all plants (1 sensor)"
)

value.i <- c(
    control  = "% dissolved oxygen",
    low      = "% dissolved oxygen",
    med.low  = "% dissolved oxygen",
    med.high = "% dissolved oxygen",
    high     = "% dissolved oxygen",
    air.temp = "degrees C",
    water.temp = "degrees C",
    par      = "micromoles m-1 s-1"
)

## Write these into the data.frame format
factors <- rbind(
    data.frame(
        attributeName = "i.flag",
        code = names(i.flag),
        definition = unname(i.flag)
    ),
    data.frame(
        attributeName = "variable",
        code = names(variable),
        definition = unname(variable)
    ),
    data.frame(
        attributeName = "value.i",
        code = names(value.i),
        definition = unname(value.i)
    )
)
```

which generates the following table

| attributeName | code       | definition                                                                      |
|:--------------|:-----------|:--------------------------------------------------------------------------------|
| i.flag        | R          | real                                                                            |
| i.flag        | I          | interpolated                                                                    |
| i.flag        | B          | bad                                                                             |
| variable      | control    | no prey added                                                                   |
| variable      | low        | 0.125 mg prey added ml-1 d-1                                                    |
| variable      | med.low    | 0,25 mg prey added ml-1 d-1                                                     |
| variable      | med.high   | 0.5 mg prey added ml-1 d-1                                                      |
| variable      | high       | 1.0 mg prey added ml-1 d-1                                                      |
| variable      | air.temp   | air temperature measured just above all plants (1 thermocouple)                 |
| variable      | water.temp | water temperature measured within each pitcher                                  |
| variable      | par        | photosynthetic active radiation (PAR) measured just above all plants (1 sensor) |
| value.i       | control    | % dissolved oxygen                                                              |
| value.i       | low        | % dissolved oxygen                                                              |
| value.i       | med.low    | % dissolved oxygen                                                              |
| value.i       | med.high   | % dissolved oxygen                                                              |
| value.i       | high       | % dissolved oxygen                                                              |
| value.i       | air.temp   | degrees C                                                                       |
| value.i       | water.temp | degrees C                                                                       |
| value.i       | par        | micromoles m-1 s-1                                                              |

### we then generate an attributeList

``` r
attributeList <- set_attributes(attributes, 
                                factors, 
                                col_classes = c("character", "Date", "Date", "Date", "factor", "factor", "factor", "numeric"))
```

### We link this to the csv file

``` r
physical <- set_physical("hf205-01-TPexp1.csv")
```

### And Assemble the dataTable

``` r
dataTable <- list(
                 entityName = "hf205-01-TPexp1.csv",
                 entityDescription = "tipping point experiment 1",
                 physical = physical,
                 attributeList = attributeList)
```

## Want to learn more about EML?

-   go to this [link](https://eml.ecoinformatics.org/)

## External resources

-   Good [reproduction
    list](https://www.youtube.com/watch?v=gW_-XTwJ1OA&list=PLi1PZkcSXdAJdmXchf9jvWMPZzsMoDVmA)
    on clean data
