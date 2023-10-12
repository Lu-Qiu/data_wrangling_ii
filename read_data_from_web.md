Read Data From the Web
================
Lu Qiu
2023-10-12

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

Import NSDUH data

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html =
  read_html(nsduh_url)
```

``` r
marj_use_df =
  nsduh_html |>
  html_table() |>
  first() |>
  slice(-1)
```

Import star wars …

``` r
swm_url = "https://www.imdb.com/list/ls070150896/"

swm_html =
  read_html(swm_url)
```

``` r
swm_title_vec =
  swm_html |>
  html_element('.lister-item-header a') |>
  html_text()

swm_gross_rev_vec =
  swm_html |> 
  html_elements(".text-muted .ghost~ .text-muted+ span") |> 
  html_text()

swm_df = 
  tibble(
    title = swm_title_vec,
    gross_rev = swm_gross_rev_vec
  )
```

## APIs

Get water data from NYC

``` r
nyc_water_df =
  GET('https://data.cityofnewyork.us/resource/ia2d-e54m.csv') |>
  content()
```

    ## Rows: 44 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

BRFSS Data

``` r
brfss_df =
  GET('https://data.cdc.gov/resource/acme-vg9e.csv',
      query = list('$limit' = 5000)) |>
  content()
```

    ## Rows: 5000 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Try it now!

``` r
poke_df =
  GET('https://pokeapi.co/api/v2/pokemon/ditto') |>
  content()
```
