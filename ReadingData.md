-   [Downloading Data from the internet](#downloading-data-from-the-internet)
-   [Reading Excel Files](#reading-excel-files)
    -   [Using `xlsx` Package](#using-xlsx-package)
        -   [You can read specific rows and columns form the Excel file](#you-can-read-specific-rows-and-columns-form-the-excel-file)
    -   [Using `readxl` Package](#using-readxl-package)

Downloading Data from the internet
==================================

Use `download.file()` that takes the url and the destination on your computer to save the file at. And note that

-   If the file url starts with **http** you can use `download.file()`.
-   It is also okay to use `download.file()` in case of **https** and **windows**.
-   But on **Mac**, you might need to set `method="curl"` to download from **https** url.
-   The download duration depends on the file size.

Example: download the 2006 microdata survey about housing for the state of Idaho.

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
download.file(url, "house_data.csv") ##save in the current dir, and name the file "data.csv"
```

List the files in the current directory

``` r
list.files("./")
```

    ## [1] "data.csv"            "data.xlsx"           "house_data.csv"     
    ## [4] "ReadingData.md"      "ReadingData.nb.html" "ReadingData.Rmd"

Use `date()` to get the downloading date.

``` r
downloadDate <- date()
downloadDate
```

    ## [1] "Wed Mar 01 13:32:02 2017"

Reading Excel Files
===================

First download the file using `download.file()`

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
download.file(url, "data.xlsx",mode="wb")
date()
```

    ## [1] "Wed Mar 01 13:32:02 2017"

Using `xlsx` Package
--------------------

You can read the file using `read.xlsx` or `read.xlsx2`. You will need to download the `xlsx` package to use them.

``` r
# Load the library
library(rJava)
library(xlsxjars)
library(xlsx)
readFile <- read.xlsx("data.xlsx", sheetIndex=1)
head(readFile)
```

    ##   Table.Name..Contract          NA.               NA..1     NA..2
    ## 1       ContractNumber ContractorId          ExpiryDate CFileName
    ## 2   GS-00P-02-BSC-0201           23 2004-09-30 00:00:00      <NA>
    ## 3   GS-00P-02-BSC-0204            5 2003-10-31 00:00:00      NULL
    ## 4   GS-00P-02-BSC-0206            6 2004-10-31 00:00:00      <NA>
    ## 5   GS-00P-02-BSC-0207            4 2006-10-31 00:00:00      <NA>
    ## 6   GS-00P-02-BSC-0209            7 2004-10-31 00:00:00      <NA>
    ##                 NA..3 NA..4 NA..5 NA..6 NA..7 NA..8 NA..9 NA..10 NA..11
    ## 1      ReactivationDt  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>   <NA>
    ## 2 2004-09-30 00:00:00  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>   <NA>
    ## 3                NULL  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>   <NA>
    ## 4 2004-11-02 00:00:00  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>   <NA>
    ## 5 2004-11-01 00:00:00  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>   <NA>
    ## 6 2004-11-01 00:00:00  <NA>  <NA>  <NA>  <NA>  <NA>  <NA>   <NA>   <NA>
    ##   NA..12 NA..13 NA..14 NA..15 NA..16 NA..17 NA..18 NA..19 NA..20 NA..21
    ## 1   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>
    ## 2   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>
    ## 3   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>
    ## 4   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>
    ## 5   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>
    ## 6   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>   <NA>
    ##   NA..22 NA..23 NA..24
    ## 1   <NA>   <NA>   <NA>
    ## 2   <NA>   <NA>   <NA>
    ## 3   <NA>   <NA>   <NA>
    ## 4   <NA>   <NA>   <NA>
    ## 5   <NA>   <NA>   <NA>
    ## 6   <NA>   <NA>   <NA>

You might face some difficulities when using the xlsx library. Here is the most famous error:

Error : .onLoad failed in loadNamespace() for 'rJava', details: call: inDL(x, as.logical(local), as.logical(now), ...)

error: unable to load shared object 'C:/Users/me/Documents/R/win-library/2.13/rJava/libs/x64/rJava.dll': LoadLibrary failure: %1 is not a valid Win32 application.

Error: package/namespace load failed for 'rJava'

**To solve that you will need to :**

-   download the `rJava` package (if the error remains)
-   check your R version `R.version()`, and download the corresponding java (32/64), from [here](https://www.java.com/en/download/win10.jsp).
-   Finally, check if your Java is in `Program Files` or `Program Files (x86)`. Add the path to the **jvm.dll** to your PATH in **windows Environment**. If the java file is in `Program Files (x86)`, it means you have 32-bit version, and you can change the default version of your `Rstudio` from Tools &gt;&gt; Global options to 32 bit. For more information check [this](http://stackoverflow.com/questions/7019912/using-the-rjava-package-on-win7-64-bit-with-r).

### You can read specific rows and columns form the Excel file

``` r
c <- 7:15
r <- 18:23
readSubset <- read.xlsx("data.xlsx", sheetIndex=1, colIndex = c, rowIndex = r)
readSubset
```

    ##     Zip CuCurrent PaCurrent PoCurrent      Contact Ext          Fax email
    ## 1 74136         0         1         0 918-491-6998   0 918-491-6659    NA
    ## 2 30329         1         0         0 404-321-5711  NA         <NA>    NA
    ## 3 74136         1         0         0 918-523-2516   0 918-523-2522    NA
    ## 4 80203         0         1         0 303-864-1919   0         <NA>    NA
    ## 5 80120         1         0         0 345-098-8890 456         <NA>    NA
    ##   Status
    ## 1      1
    ## 2      1
    ## 3      1
    ## 4      1
    ## 5      1

Using `readxl` Package
----------------------

Another way to read Excel files is to use `read_excel` from the `readxl` library. For more information check [this](http://stackoverflow.com/questions/7049272/importing-xlsx-file-into-r).

After installing the package..

``` r
library(readxl)
dataFile <- read_excel("data.xlsx")
head(dataFile)
```

    ##   Table Name: Contract           NA                              
    ## 1       ContractNumber ContractorId          ExpiryDate CFileName
    ## 2   GS-00P-02-BSC-0201           23 2004-09-30 00:00:00      <NA>
    ## 3   GS-00P-02-BSC-0204            5 2003-10-31 00:00:00      NULL
    ## 4   GS-00P-02-BSC-0206            6 2004-10-31 00:00:00      <NA>
    ## 5   GS-00P-02-BSC-0207            4 2006-10-31 00:00:00      <NA>
    ## 6   GS-00P-02-BSC-0209            7 2004-10-31 00:00:00      <NA>
    ##                                                                        
    ## 1      ReactivationDt <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 2 2004-09-30 00:00:00 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 3                NULL <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 4 2004-11-02 00:00:00 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 5 2004-11-01 00:00:00 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 6 2004-11-01 00:00:00 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ##                                                         
    ## 1 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 2 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 3 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 4 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 5 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 6 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
