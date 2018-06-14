
<!-- README.md is generated from README.Rmd. Please edit that file -->
<img src="inst/img/logo.png" align="center" />

ingestr
=======

[![Travis-CI Build Status](https://travis-ci.org/jpshanno/ingestr.svg?branch=master)](https://travis-ci.org/jpshanno/ingestr) [![Coverage Status](https://img.shields.io/codecov/c/github/jpshanno/ingestr/master.svg)](https://codecov.io/github/jpshanno/ingestr?branch=master)

*An R package for reading environmental data from raw formats into dataframes.*

This is project was initiated at the inagural [IMCR Hackathon](https://github.com/IMCR-Hackathon/HackathonCentral). The end product of this effort will be an R package on CRAN. The package will primarily deal with reading data from files, though there will be some utilities for initial cleanup of files such as removing blank rows and columns at the end of a CSV file.

The guiding principles of the project are that 1. All sources of environmental-related data should be easy to read directly 2. Reading in data should provide a standard output 3. Header information contained within sensor data files should be stored in a standard, easily readable format 4. Associating imported data with its original source is the first step towards good data provenance records and reproducibility

*Contributing*

Installation
------------

You can install ingestr from github with:

``` r
# install.packages("devtools")
devtools::install_github("jpshanno/ingestr")
```

Ingesting Data
--------------

Many sensors provide their output as delimited files with header information contained above the recorded data. Each ingestr function to read in data starts with `ingest_` to make autocomplete easier. Running any ingest function will read in the data and format the data into a clean R data.frame. Column names are taken directly from the data file, and users have the option to read the header information into a separate data frame in the environment where the function was called. A message and the dat.frame structure will be printed to alert the user that the data.frame was created. All data and header data that are read in will have the data source appended as a column to the data.

``` r
library(ingestr)
campbell_file <- 
  system.file("example_data",
              "campbell_scientific_tao5.dat",
              package = "ingestr")

campbell_data <- 
  ingest_campbell(file.name = campbell_file,
                  add.units = TRUE,
                  add.measurements = TRUE,
                  header.info = TRUE,
                  header.info.name = "header_campbell")
#> The metadata were returned as the data.frame header_campbell
#> 'data.frame':    1 obs. of  8 variables:
#>  $ file_type               : chr "TOA5"
#>  $ logger_name             : chr "FRF_Village"
#>  $ logger_model            : chr "CR1000"
#>  $ logger_serial_number    : int 63162
#>  $ logger_os_version       : chr "CR1000.Std.27"
#>  $ logger_program_name     : chr "CPU:Complete Station_Bridget_31Oct2014.cr1"
#>  $ logger_program_signature: int 65292
#>  $ logger_table_name       : chr "Table_24"

str(campbell_data)
#> 'data.frame':    1272 obs. of  24 variables:
#>  $ TIMESTAMP                    : POSIXct, format: "2014-11-02" "2014-11-03" ...
#>  $ RECORD                       : int  0 1 2 3 4 5 6 7 8 9 ...
#>  $ BattV_Min_Volts_Min          : num  13.5 13.2 13.2 13.3 13.4 ...
#>  $ PTemp_C_Max_Deg.C_Max        : num  0.989 11.02 12.88 8.61 4.064 ...
#>  $ PTemp_C_Min_Deg.C_Min        : num  -3.32 -1.14 1.01 1.84 1.22 ...
#>  $ AirTC_Avg_Deg.C_Avg          : num  -2.042 3.432 5.348 3.808 0.677 ...
#>  $ RH_Avg_._Avg                 : num  75 54.5 66.2 90.9 81.3 ...
#>  $ BP_kPa_Avg_kPa_Avg           : num  102 102 101 101 101 ...
#>  $ Rain_mm_Tot_mm_Tot           : num  0.73 2.482 0.146 2.044 0.438 ...
#>  $ Rain_24hr_mm_Smp             : int  0 0 0 0 0 0 0 0 0 0 ...
#>  $ WS_ms_Avg_meters.second_Avg  : num  0.266 0.843 0.579 0.919 0.698 ...
#>  $ WS_ms_S_WVT_meters.second_WVc: num  0.266 0.843 0.579 0.919 0.698 ...
#>  $ WindDir_D1_WVT_Deg_WVc       : num  167 186 176 263 293 ...
#>  $ WindDir_SD1_WVT_Deg_WVc      : num  31.3 36.9 49.3 46.9 57.8 ...
#>  $ VW_Avg_Avg                   : num  0.232 0.226 0.219 0.237 0.237 0.226 0.221 0.229 0.228 0.223 ...
#>  $ T107_C_Avg_Deg.C_Avg         : num  4.24 4.24 4.97 5.79 4.64 ...
#>  $ PAR_Den_Avg_umol.s.m.2_Avg   : num  0.031 122.7 105.3 16.6 34.72 ...
#>  $ PAR_Tot_Tot_mmol.m.2_Tot     : num  5.51e-01 1.06e+04 9.10e+03 1.43e+03 3.00e+03 ...
#>  $ SR01Up_Tot_W.m.2_Tot         : num  -542 7999 7999 7999 7999 ...
#>  $ SR01Dn_Tot_W.m.2_Tot         : num  507 7999 7999 2589 5394 ...
#>  $ IR01Up_Tot_W.m.2_Tot         : num  -7999 -7999 -7999 -208 -7999 ...
#>  $ IR01Dn_Tot_W.m.2_Tot         : num  -4385 -7999 -7999 -457 1520 ...
#>  $ NetTot_Tot_W.m.2_Tot         : num  -7999 7999 7999 7999 5115 ...
#>  $ input_source                 : chr  "C:/R_Libraries/ingestr/example_data/campbell_scientific_tao5.dat" "C:/R_Libraries/ingestr/example_data/campbell_scientific_tao5.dat" "C:/R_Libraries/ingestr/example_data/campbell_scientific_tao5.dat" "C:/R_Libraries/ingestr/example_data/campbell_scientific_tao5.dat" ...

str(header_campbell)
#> 'data.frame':    1 obs. of  8 variables:
#>  $ file_type               : chr "TOA5"
#>  $ logger_name             : chr "FRF_Village"
#>  $ logger_model            : chr "CR1000"
#>  $ logger_serial_number    : int 63162
#>  $ logger_os_version       : chr "CR1000.Std.27"
#>  $ logger_program_name     : chr "CPU:Complete Station_Bridget_31Oct2014.cr1"
#>  $ logger_program_signature: int 65292
#>  $ logger_table_name       : chr "Table_24"
```

### Incorporate File Naming Conventions as Data

### Batch Ingests

Preliminary Clean-up Utilities
------------------------------

QAQCR
-----
