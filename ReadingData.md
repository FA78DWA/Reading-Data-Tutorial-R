-   [Downloading Data from the internet](#downloading-data-from-the-internet)
-   [Reading Excel Files](#reading-excel-files)
    -   [Using `xlsx` Package](#using-xlsx-package)
        -   [Reading specific rows and columns form the Excel file](#reading-specific-rows-and-columns-form-the-excel-file)
    -   [Using `readxl` Package](#using-readxl-package)
-   [Reading XML Files](#reading-xml-files)

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
    ## [7] "simple.xml"

Use `date()` to get the downloading date.

``` r
downloadDate <- date()
downloadDate
```

    ## [1] "Thu Mar 02 09:49:50 2017"

Reading Excel Files
===================

First download the file using `download.file()`

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
download.file(url, "data.xlsx",mode="wb")
date()
```

    ## [1] "Thu Mar 02 09:49:50 2017"

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

### Reading specific rows and columns form the Excel file

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

Reading XML Files
=================

XML stands for *"Extensible Markup Language"*. For more information see [wiki\_xml](https://en.wikipedia.org/wiki/XML#Key_terminology). We start by downloading and loading the `XML` package. Then reading the xml file with `xmlTreeParse`.

``` r
library(XML)

url <- "https://www.w3schools.com/xml/simple.xml"
download.file(url, "simple.xml")

xmlFile <- xmlTreeParse("simple.xml", useInternalNodes = TRUE)
## check the file class 
class(xmlFile)
```

    ## [1] "XMLInternalDocument" "XMLAbstractDocument"

Wrapping the content inside the xml file.

``` r
## get the content of the root
rootNode <- xmlRoot(xmlFile)
rootNode
```

    ## <breakfast_menu>
    ##   <food>
    ##     <name>Belgian Waffles</name>
    ##     <price>$5.95</price>
    ##     <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
    ##     <calories>650</calories>
    ##   </food>
    ##   <food>
    ##     <name>Strawberry Belgian Waffles</name>
    ##     <price>$7.95</price>
    ##     <description>Light Belgian waffles covered with strawberries and whipped cream</description>
    ##     <calories>900</calories>
    ##   </food>
    ##   <food>
    ##     <name>Berry-Berry Belgian Waffles</name>
    ##     <price>$8.95</price>
    ##     <description>Light Belgian waffles covered with an assortment of fresh berries and whipped cream</description>
    ##     <calories>900</calories>
    ##   </food>
    ##   <food>
    ##     <name>French Toast</name>
    ##     <price>$4.50</price>
    ##     <description>Thick slices made from our homemade sourdough bread</description>
    ##     <calories>600</calories>
    ##   </food>
    ##   <food>
    ##     <name>Homestyle Breakfast</name>
    ##     <price>$6.95</price>
    ##     <description>Two eggs, bacon or sausage, toast, and our ever-popular hash browns</description>
    ##     <calories>950</calories>
    ##   </food>
    ## </breakfast_menu>

**Now, start exploring**

``` r
## Get the name of the node
xmlName(rootNode)
```

    ## [1] "breakfast_menu"

``` r
## Take a look at the content of the first child
rootNode[[1]]
```

    ## <food>
    ##   <name>Belgian Waffles</name>
    ##   <price>$5.95</price>
    ##   <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
    ##   <calories>650</calories>
    ## </food>

``` r
## How many children in the node (number of food nodes)
xmlSize(rootNode)
```

    ## [1] 5

``` r
## Get the name of the first child node
xmlName(rootNode[[1]])
```

    ## [1] "food"

**We can also list the names and the sizes of the subnodes**

``` r
## number of childrens inside the first child 
xmlSize(rootNode[[1]])
```

    ## [1] 4

``` r
## Get the names of the childrens inside the first child 
xmlSApply(rootNode[[1]], xmlName)
```

    ##          name         price   description      calories 
    ##        "name"       "price" "description"    "calories"

``` r
## Extract the food "name" from all subnodes
xmlSApply(rootNode, function(x) x[[1]][[1]])
```

    ## $food
    ## Belgian Waffles 
    ## 
    ## $food
    ## Strawberry Belgian Waffles 
    ## 
    ## $food
    ## Berry-Berry Belgian Waffles 
    ## 
    ## $food
    ## French Toast 
    ## 
    ## $food
    ## Homestyle Breakfast

``` r
## Another way to get inside values, price in this case 
xmlSApply(rootNode, function(x) x[['price']][[1]])
```

    ## $food
    ## $5.95 
    ## 
    ## $food
    ## $7.95 
    ## 
    ## $food
    ## $8.95 
    ## 
    ## $food
    ## $4.50 
    ## 
    ## $food
    ## $6.95
