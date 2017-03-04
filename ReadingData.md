-   [Downloading Data from the internet](#downloading-data-from-the-internet)
-   [Reading Excel Files](#reading-excel-files)
    -   [Using `xlsx` Package](#using-xlsx-package)
    -   [Using `readxl` Package](#using-readxl-package)
-   [Reading XML Files](#reading-xml-files)
    -   [Using `XPath`](#using-xpath)
-   [Reading JSON Files](#reading-json-files)
    -   [Data frame to JSON](#data-frame-to-json)
    -   [JSON to Data frame](#json-to-data-frame)
-   [Using `data.table`](#using-data.table)
    -   [Subsetting Rows](#subsetting-rows)
    -   [Subsetting Columns](#subsetting-columns)
    -   [Operating on a subset of a data table](#operating-on-a-subset-of-a-data-table)
    -   [Using special variable `.N`](#using-special-variable-.n)
    -   [Create a key on your data table](#create-a-key-on-your-data-table)
    -   [Join two data tables together](#join-two-data-tables-together)
-   [Reading from mySQL](#reading-from-mysql)
-   [Reading from HDF5](#reading-from-hdf5)

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

    ## [1] "books.xml"           "data.csv"            "data.xlsx"          
    ## [4] "house_data.csv"      "ReadingData.md"      "ReadingData.nb.html"
    ## [7] "ReadingData.Rmd"     "simple.xml"

Use `date()` to get the downloading date.

``` r
downloadDate <- date()
downloadDate
```

    ## [1] "Sat Mar 04 14:04:53 2017"

Reading Excel Files
===================

First download the file using `download.file()`

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
download.file(url, "data.xlsx",mode="wb")
date()
```

    ## [1] "Sat Mar 04 14:04:53 2017"

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

You might face some difficulties when using the xlsx library. Here is the most famous error:

Error : .onLoad failed in loadNamespace() for 'rJava', details: call: inDL(x, as.logical(local), as.logical(now), ...)

error: unable to load shared object 'C:/Users/me/Documents/R/win-library/2.13/rJava/libs/x64/rJava.dll': LoadLibrary failure: %1 is not a valid Win32 application.

Error: package/namespace load failed for 'rJava'

**To solve that you will need to :**

-   download the `rJava` package (if the error remains)
-   check your R version `R.version()`, and download the corresponding java (32/64), from [here](https://www.java.com/en/download/win10.jsp).
-   Finally, check if your Java is in `Program Files` or `Program Files (x86)`. Add the path to the **jvm.dll** to your PATH in **windows Environment**. If the java file is in `Program Files (x86)`, it means you have 32-bit version, and you can change the default version of your `Rstudio` from Tools &gt;&gt; Global options to 32 bit. For more information check [this](http://stackoverflow.com/questions/7019912/using-the-rjava-package-on-win7-64-bit-with-r).

**Reading specific rows and columns form the Excel file**

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

XML stands for *"Extensible Markup Language"*. For more information see [wiki\_xml](https://en.wikipedia.org/wiki/XML#Key_terminology) and [readXML](https://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/XML.pdf). We start by downloading and loading the `XML` package. Then reading the xml file with `xmlTreeParse`.

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

Using `XPath`
-------------

`xpathApply` and `xpathSApply` provide a way to find XML nodes that match a particular criterion to identify nodes of interest within the document. The set of matching node are returned as a list. For more information type `?xpathApply` in R console. Note that `xpathSApply` is a simplified version of `xpathApply`, just like `Sapply` and `apply`.

\*\* XPath Language Notes \*\*

-   `/node` - Top level node
-   `//node` - Node at ANY level
-   `node[@attr-name]` - node that has an attribute named "attr-name"
-   `node[@attr-name='bob']` - node that has attribute named attr-name with value 'bob'
-   `node/@x` - value of attribute x in node with such attr.

``` r
## Get the food "name"
xpathApply(rootNode, "//name", xmlValue)
```

    ## [[1]]
    ## [1] "Belgian Waffles"
    ## 
    ## [[2]]
    ## [1] "Strawberry Belgian Waffles"
    ## 
    ## [[3]]
    ## [1] "Berry-Berry Belgian Waffles"
    ## 
    ## [[4]]
    ## [1] "French Toast"
    ## 
    ## [[5]]
    ## [1] "Homestyle Breakfast"

``` r
## Get the food "name" with xpathSApply
xpathSApply(rootNode, "//name", xmlValue)
```

    ## [1] "Belgian Waffles"             "Strawberry Belgian Waffles" 
    ## [3] "Berry-Berry Belgian Waffles" "French Toast"               
    ## [5] "Homestyle Breakfast"

``` r
## Get the prices
xpathApply(rootNode, "//price", xmlValue)
```

    ## [[1]]
    ## [1] "$5.95"
    ## 
    ## [[2]]
    ## [1] "$7.95"
    ## 
    ## [[3]]
    ## [1] "$8.95"
    ## 
    ## [[4]]
    ## [1] "$4.50"
    ## 
    ## [[5]]
    ## [1] "$6.95"

``` r
## Get the prices with xpathSApply
xpathSApply(rootNode, "//price", xmlValue)
```

    ## [1] "$5.95" "$7.95" "$8.95" "$4.50" "$6.95"

**Extract content by attributes**

First, we load another xml `books.xml` file that contains attributes to work on. You can find it [here](https://msdn.microsoft.com/en-us/library/ms762271(v=vs.85).aspx) or in the file directory [here](https://github.com/FA78DWA/Reading-Data-Tutorial-R).

``` r
## Load the file, and wrap into one variable
books <- xmlRoot(xmlTreeParse("books.xml", useInternalNodes = TRUE))

## Show the the first book from the books library we loaded
books[[1]]
```

    ## <book id="bk101">
    ##   <author>Gambardella, Matthew</author>
    ##   <title>XML Developer's Guide</title>
    ##   <genre>Computer</genre>
    ##   <price>44.95</price>
    ##   <publish_date>2000-10-01</publish_date>
    ##   <description>An in-depth look at creating applications 
    ##       with XML.</description>
    ## </book>

``` r
## Get the number of books inside the library
xmlSize(books)
```

    ## [1] 12

``` r
## Get the title of the "book"s that have "id" attribute. In this case all books have "id"
xpathSApply(books, "//book[@id]/title", xmlValue)
```

    ##  [1] "XML Developer's Guide"                 
    ##  [2] "Midnight Rain"                         
    ##  [3] "Maeve Ascendant"                       
    ##  [4] "Oberon's Legacy"                       
    ##  [5] "The Sundered Grail"                    
    ##  [6] "Lover Birds"                           
    ##  [7] "Splish Splash"                         
    ##  [8] "Creepy Crawlies"                       
    ##  [9] "Paradox Lost"                          
    ## [10] "Microsoft .NET: The Programming Bible" 
    ## [11] "MSXML3: A Comprehensive Guide"         
    ## [12] "Visual Studio 7: A Comprehensive Guide"

``` r
## Get the "title" of the book with "id = bk103". Note that "id" is an "attribute"
xpathSApply(books, "//book[@id='bk103']/title", xmlValue)
```

    ## [1] "Maeve Ascendant"

Reading JSON Files
==================

JSON stands for Javascript Object Notation. It has similar structure as XML but different syntax. To read JSON files we need the `jsonlite` package.

``` r
## Load the library
library(jsonlite)

## Download and read the file
jsonData <- fromJSON("https://api.github.com/users/jtleek/repos") 

## Get the names used in the JSON object
names(jsonData)
```

    ##  [1] "id"                "name"              "full_name"        
    ##  [4] "owner"             "private"           "html_url"         
    ##  [7] "description"       "fork"              "url"              
    ## [10] "forks_url"         "keys_url"          "collaborators_url"
    ## [13] "teams_url"         "hooks_url"         "issue_events_url" 
    ## [16] "events_url"        "assignees_url"     "branches_url"     
    ## [19] "tags_url"          "blobs_url"         "git_tags_url"     
    ## [22] "git_refs_url"      "trees_url"         "statuses_url"     
    ## [25] "languages_url"     "stargazers_url"    "contributors_url" 
    ## [28] "subscribers_url"   "subscription_url"  "commits_url"      
    ## [31] "git_commits_url"   "comments_url"      "issue_comment_url"
    ## [34] "contents_url"      "compare_url"       "merges_url"       
    ## [37] "archive_url"       "downloads_url"     "issues_url"       
    ## [40] "pulls_url"         "milestones_url"    "notifications_url"
    ## [43] "labels_url"        "releases_url"      "deployments_url"  
    ## [46] "created_at"        "updated_at"        "pushed_at"        
    ## [49] "git_url"           "ssh_url"           "clone_url"        
    ## [52] "svn_url"           "homepage"          "size"             
    ## [55] "stargazers_count"  "watchers_count"    "language"         
    ## [58] "has_issues"        "has_downloads"     "has_wiki"         
    ## [61] "has_pages"         "forks_count"       "mirror_url"       
    ## [64] "open_issues_count" "forks"             "open_issues"      
    ## [67] "watchers"          "default_branch"

``` r
## Nested objects, get the names inside the owner object
names(jsonData$owner)
```

    ##  [1] "login"               "id"                  "avatar_url"         
    ##  [4] "gravatar_id"         "url"                 "html_url"           
    ##  [7] "followers_url"       "following_url"       "gists_url"          
    ## [10] "starred_url"         "subscriptions_url"   "organizations_url"  
    ## [13] "repos_url"           "events_url"          "received_events_url"
    ## [16] "type"                "site_admin"

``` r
## owner login name
jsonData$owner$login
```

    ##  [1] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
    ##  [8] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
    ## [15] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
    ## [22] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
    ## [29] "jtleek" "jtleek"

Data frame to JSON
------------------

``` r
## Load iris dataset (already in R), and get the first 10 observation for simplicity
data(iris)
iris = iris[1:10,]

## convert it to JSON
myIris <- toJSON(iris, pretty = TRUE)

## Show it
cat(myIris)
```

    ## [
    ##   {
    ##     "Sepal.Length": 5.1,
    ##     "Sepal.Width": 3.5,
    ##     "Petal.Length": 1.4,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 4.9,
    ##     "Sepal.Width": 3,
    ##     "Petal.Length": 1.4,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 4.7,
    ##     "Sepal.Width": 3.2,
    ##     "Petal.Length": 1.3,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 4.6,
    ##     "Sepal.Width": 3.1,
    ##     "Petal.Length": 1.5,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 5,
    ##     "Sepal.Width": 3.6,
    ##     "Petal.Length": 1.4,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 5.4,
    ##     "Sepal.Width": 3.9,
    ##     "Petal.Length": 1.7,
    ##     "Petal.Width": 0.4,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 4.6,
    ##     "Sepal.Width": 3.4,
    ##     "Petal.Length": 1.4,
    ##     "Petal.Width": 0.3,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 5,
    ##     "Sepal.Width": 3.4,
    ##     "Petal.Length": 1.5,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 4.4,
    ##     "Sepal.Width": 2.9,
    ##     "Petal.Length": 1.4,
    ##     "Petal.Width": 0.2,
    ##     "Species": "setosa"
    ##   },
    ##   {
    ##     "Sepal.Length": 4.9,
    ##     "Sepal.Width": 3.1,
    ##     "Petal.Length": 1.5,
    ##     "Petal.Width": 0.1,
    ##     "Species": "setosa"
    ##   }
    ## ]

JSON to Data frame
------------------

``` r
## Get the iris data bak as datafame
getIrisBack <- fromJSON(myIris)

## Show the head of the dataframe
head(getIrisBack)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

Using `data.table`
==================

This package inherits from `data.frame` this means that all functions that accept `data.frame` will work on `data.table`. Also, `data.table` is much faster in data sub-setting, grouping and updating, because it is written in C.

Starting by downloading the package and load it. Then, create a `data table`

``` r
library(data.table)

#create a data table with 9 rows and 3 columns
DT <- data.table(x=rnorm(9), y=rep(c("a","b","c"), each=3), z=rnorm(9))

DT
```

    ##             x y          z
    ## 1:  0.8422003 a  0.8862962
    ## 2:  2.5427429 a  0.1217852
    ## 3: -0.1075227 a -0.4206787
    ## 4: -1.1539703 b  0.4501337
    ## 5: -0.2735542 b -0.1033612
    ## 6:  1.9510997 b -0.8047878
    ## 7:  0.2020056 c  1.0517892
    ## 8: -0.5666600 c  0.6884780
    ## 9:  0.3688554 c -0.4734842

**To see all data tables in memory call `tabels()`**

``` r
tables()
```

    ##      NAME NROW NCOL MB COLS  KEY
    ## [1,] DT      9    3  1 x,y,z    
    ## Total: 1MB

Subsetting Rows
---------------

Use `DT` from previous step.

``` r
## Get the first 2 rows from DT
DT[2,]
```

    ##           x y         z
    ## 1: 2.542743 a 0.1217852

``` r
## Get the rows with y=c
DT[DT$y=="c",]
```

    ##             x y          z
    ## 1:  0.2020056 c  1.0517892
    ## 2: -0.5666600 c  0.6884780
    ## 3:  0.3688554 c -0.4734842

``` r
## Get certain rows for example 1st, 5th, and 9th
DT[c(1,5,9),]
```

    ##             x y          z
    ## 1:  0.8422003 a  0.8862962
    ## 2: -0.2735542 b -0.1033612
    ## 3:  0.3688554 c -0.4734842

Subsetting Columns
------------------

We still using `DT`

``` r
## Get the 2nd columns from DT
DT[,2]
```

    ##    y
    ## 1: a
    ## 2: a
    ## 3: a
    ## 4: b
    ## 5: b
    ## 6: b
    ## 7: c
    ## 8: c
    ## 9: c

``` r
## Get column with its name
DT[,DT$z]
```

    ## [1]  0.8862962  0.1217852 -0.4206787  0.4501337 -0.1033612 -0.8047878
    ## [7]  1.0517892  0.6884780 -0.4734842

``` r
## Get certain columns for example 1st and 3rd
DT[,c(1,3)]
```

    ##             x          z
    ## 1:  0.8422003  0.8862962
    ## 2:  2.5427429  0.1217852
    ## 3: -0.1075227 -0.4206787
    ## 4: -1.1539703  0.4501337
    ## 5: -0.2735542 -0.1033612
    ## 6:  1.9510997 -0.8047878
    ## 7:  0.2020056  1.0517892
    ## 8: -0.5666600  0.6884780
    ## 9:  0.3688554 -0.4734842

Operating on a subset of a data table
-------------------------------------

Until now the sub-setting either rows or columns are intuitive. `DT` is a 2-dimensional array(table), and you can get a specific element using DT\[i,j\] format just like `matlab`.

But, what if we want to take the average of the columns, or do any other operation on a subset of the `DT`. In this case **DT\[i,j,by\]** comes very handy. **DT\[i,j,by\]** means Take DT subset rows by **i**, then compute **j** grouped by **by**. Examples.

``` r
## Calculate the mean of x and sum of z
DT[,list(mean(x), sum(z))]
```

    ##           V1      V2
    ## 1: 0.4227996 1.39617

``` r
## Get a table with the count of each y value
DT[,table(y)]
```

    ## y
    ## a b c 
    ## 3 3 3

``` r
## Add new column w which is z squared
DT[,w:=z^2]
```

    ##             x y          z          w
    ## 1:  0.8422003 a  0.8862962 0.78552090
    ## 2:  2.5427429 a  0.1217852 0.01483163
    ## 3: -0.1075227 a -0.4206787 0.17697060
    ## 4: -1.1539703 b  0.4501337 0.20262035
    ## 5: -0.2735542 b -0.1033612 0.01068354
    ## 6:  1.9510997 b -0.8047878 0.64768346
    ## 7:  0.2020056 c  1.0517892 1.10626061
    ## 8: -0.5666600 c  0.6884780 0.47400201
    ## 9:  0.3688554 c -0.4734842 0.22418733

``` r
## Add new column m = log(x+z+5). Note that we used {} to put in multi-line expression. Each expression ends with ';'
DT[,m:={tmp <- (x+z); log2(tmp+5)}]
```

    ##             x y          z          w        m
    ## 1:  0.8422003 a  0.8862962 0.78552090 2.750284
    ## 2:  2.5427429 a  0.1217852 0.01483163 2.938197
    ## 3: -0.1075227 a -0.4206787 0.17697060 2.160855
    ## 4: -1.1539703 b  0.4501337 0.20262035 2.103049
    ## 5: -0.2735542 b -0.1033612 0.01068354 2.208856
    ## 6:  1.9510997 b -0.8047878 0.64768346 2.619721
    ## 7:  0.2020056 c  1.0517892 1.10626061 2.644732
    ## 8: -0.5666600 c  0.6884780 0.47400201 2.356656
    ## 9:  0.3688554 c -0.4734842 0.22418733 2.291418

``` r
## BOLEAN OPERATIONS: Add new column a shows if x>=0 or <0
DT[,a:= x>=0]
```

    ##             x y          z          w        m     a
    ## 1:  0.8422003 a  0.8862962 0.78552090 2.750284  TRUE
    ## 2:  2.5427429 a  0.1217852 0.01483163 2.938197  TRUE
    ## 3: -0.1075227 a -0.4206787 0.17697060 2.160855 FALSE
    ## 4: -1.1539703 b  0.4501337 0.20262035 2.103049 FALSE
    ## 5: -0.2735542 b -0.1033612 0.01068354 2.208856 FALSE
    ## 6:  1.9510997 b -0.8047878 0.64768346 2.619721  TRUE
    ## 7:  0.2020056 c  1.0517892 1.10626061 2.644732  TRUE
    ## 8: -0.5666600 c  0.6884780 0.47400201 2.356656 FALSE
    ## 9:  0.3688554 c -0.4734842 0.22418733 2.291418  TRUE

``` r
## GROUPING: get the mean of(x+w) when a is TRUE and a is False, then add the result in new column b. Note that b has only 2 values.
DT[,b:=mean(x+w), by=a]
```

    ##             x y          z          w        m     a          b
    ## 1:  0.8422003 a  0.8862962 0.78552090 2.750284  TRUE  1.7370776
    ## 2:  2.5427429 a  0.1217852 0.01483163 2.938197  TRUE  1.7370776
    ## 3: -0.1075227 a -0.4206787 0.17697060 2.160855 FALSE -0.3093577
    ## 4: -1.1539703 b  0.4501337 0.20262035 2.103049 FALSE -0.3093577
    ## 5: -0.2735542 b -0.1033612 0.01068354 2.208856 FALSE -0.3093577
    ## 6:  1.9510997 b -0.8047878 0.64768346 2.619721  TRUE  1.7370776
    ## 7:  0.2020056 c  1.0517892 1.10626061 2.644732  TRUE  1.7370776
    ## 8: -0.5666600 c  0.6884780 0.47400201 2.356656 FALSE -0.3093577
    ## 9:  0.3688554 c -0.4734842 0.22418733 2.291418  TRUE  1.7370776

**CAUTION** Creating a copy of `DT`, then changing the original `DT` will also change the copy.

``` r
DT2 <- DT
DT[,y:=2]
```

    ## Warning in `[.data.table`(DT, , `:=`(y, 2)): Coerced 'double' RHS to
    ## 'character' to match the column's type; may have truncated precision.
    ## Either change the target column to 'double' first (by creating a new
    ## 'double' vector length 9 (nrows of entire table) and assign that; i.e.
    ## 'replace' column), or coerce RHS to 'character' (e.g. 1L, NA_[real|
    ## integer]_, as.*, etc) to make your intent clear and for speed. Or, set the
    ## column type correctly up front when you create the table and stick to it,
    ## please.

    ##             x y          z          w        m     a          b
    ## 1:  0.8422003 2  0.8862962 0.78552090 2.750284  TRUE  1.7370776
    ## 2:  2.5427429 2  0.1217852 0.01483163 2.938197  TRUE  1.7370776
    ## 3: -0.1075227 2 -0.4206787 0.17697060 2.160855 FALSE -0.3093577
    ## 4: -1.1539703 2  0.4501337 0.20262035 2.103049 FALSE -0.3093577
    ## 5: -0.2735542 2 -0.1033612 0.01068354 2.208856 FALSE -0.3093577
    ## 6:  1.9510997 2 -0.8047878 0.64768346 2.619721  TRUE  1.7370776
    ## 7:  0.2020056 2  1.0517892 1.10626061 2.644732  TRUE  1.7370776
    ## 8: -0.5666600 2  0.6884780 0.47400201 2.356656 FALSE -0.3093577
    ## 9:  0.3688554 2 -0.4734842 0.22418733 2.291418  TRUE  1.7370776

``` r
DT2
```

    ##             x y          z          w        m     a          b
    ## 1:  0.8422003 2  0.8862962 0.78552090 2.750284  TRUE  1.7370776
    ## 2:  2.5427429 2  0.1217852 0.01483163 2.938197  TRUE  1.7370776
    ## 3: -0.1075227 2 -0.4206787 0.17697060 2.160855 FALSE -0.3093577
    ## 4: -1.1539703 2  0.4501337 0.20262035 2.103049 FALSE -0.3093577
    ## 5: -0.2735542 2 -0.1033612 0.01068354 2.208856 FALSE -0.3093577
    ## 6:  1.9510997 2 -0.8047878 0.64768346 2.619721  TRUE  1.7370776
    ## 7:  0.2020056 2  1.0517892 1.10626061 2.644732  TRUE  1.7370776
    ## 8: -0.5666600 2  0.6884780 0.47400201 2.356656 FALSE -0.3093577
    ## 9:  0.3688554 2 -0.4734842 0.22418733 2.291418  TRUE  1.7370776

Using special variable `.N`
---------------------------

`.N` is an integer, length 1, containing the number of rows in the group.

``` r
set.seed(0)

#sample(x,n,T/F) takes a sample of the specified size (n) from the elements of (x) using either with or without (T/F) replacement.
x <- sample(letters[1:3], 1000, TRUE) 

## Create new DT from this sample
newDT <- data.table(x)

## Group newDT by x
newDT[ ,.N, by=x]
```

    ##    x   N
    ## 1: c 336
    ## 2: a 319
    ## 3: b 345

Create a key on your data table
-------------------------------

`setkey()` sorts a data.table and marks it as sorted (with an attribute sorted). The sorted columns are the key. The key can be any columns in any order. The columns are always sorted in ascending order.

`key()` returns the data.table's key if it exists, and NULL if none exist.

`haskey()` returns a logical TRUE/FALSE depending on whether the data.table has a key (or not).

``` r
## Create 300*2 data table 
DT <- data.table(x=rep(c("a","b","c"), each=100), y=rnorm(300))
DT #Before
```

    ##      x          y
    ##   1: a -0.6263682
    ##   2: a  0.4813353
    ##   3: a  1.6952711
    ##   4: a -1.7612263
    ##   5: a  0.1980130
    ##  ---             
    ## 296: c  0.1648914
    ## 297: c  0.8685774
    ## 298: c -1.0780345
    ## 299: c -1.2223320
    ## 300: c -0.7114447

``` r
## Set the key of this data table to be x
setkey(DT,x)

## DT after sorting
DT
```

    ##      x          y
    ##   1: a -0.6263682
    ##   2: a  0.4813353
    ##   3: a  1.6952711
    ##   4: a -1.7612263
    ##   5: a  0.1980130
    ##  ---             
    ## 296: c  0.1648914
    ## 297: c  0.8685774
    ## 298: c -1.0780345
    ## 299: c -1.2223320
    ## 300: c -0.7114447

``` r
## Get the key of DT
key(DT)
```

    ## [1] "x"

``` r
## Check if DT has a key
haskey(DT)
```

    ## [1] TRUE

``` r
## Now DT knows that x is the key and x has 3 different values (a,b,c). So, we can Subset the DT using a specific value of the key 
DT['c']
```

    ##      x            y
    ##   1: c  0.581201043
    ##   2: c  0.379293067
    ##   3: c -0.310740876
    ##   4: c  0.886390001
    ##   5: c -1.641864750
    ##   6: c -0.988563742
    ##   7: c -0.244003429
    ##   8: c  0.156056926
    ##   9: c  0.102051958
    ##  10: c -0.287458172
    ##  11: c -0.293194886
    ##  12: c  0.469477058
    ##  13: c -0.662588512
    ##  14: c -2.190372666
    ##  15: c  0.003854672
    ##  16: c  0.862942829
    ##  17: c  0.980223359
    ##  18: c -0.291724771
    ##  19: c -0.061013476
    ##  20: c  1.511868401
    ##  21: c -0.643609065
    ##  22: c -0.130950479
    ##  23: c  0.485211453
    ##  24: c -0.645182435
    ##  25: c -0.463956220
    ##  26: c  0.567249697
    ##  27: c -0.723407834
    ##  28: c  0.455884554
    ##  29: c -1.197934626
    ##  30: c -2.018180550
    ##  31: c  0.619723405
    ##  32: c -0.132857466
    ##  33: c -0.434960740
    ##  34: c -0.521754179
    ##  35: c  0.992288426
    ##  36: c -1.085422408
    ##  37: c  0.959832414
    ##  38: c -1.039010541
    ##  39: c -0.137841967
    ##  40: c -0.214708418
    ##  41: c  0.573718585
    ##  42: c -1.776449315
    ##  43: c -0.256392295
    ##  44: c -0.138550636
    ##  45: c  0.557684974
    ##  46: c  1.122346289
    ##  47: c -0.982147207
    ##  48: c  0.326905556
    ##  49: c  0.447826608
    ##  50: c  1.183724120
    ##  51: c -0.088616917
    ##  52: c  1.146997880
    ##  53: c  0.086812573
    ##  54: c -0.295005034
    ##  55: c  0.512332939
    ##  56: c  0.283313635
    ##  57: c  0.384543716
    ##  58: c  1.004804339
    ##  59: c -0.285965791
    ##  60: c -0.237339891
    ##  61: c -0.203674367
    ##  62: c  1.072472006
    ##  63: c  1.950277786
    ##  64: c  0.964454067
    ##  65: c  1.038404588
    ##  66: c  1.768873604
    ##  67: c -0.571650147
    ##  68: c -1.470282890
    ##  69: c -1.110366445
    ##  70: c  0.254403389
    ##  71: c  0.023344681
    ##  72: c -2.715925295
    ##  73: c  1.185289251
    ##  74: c -0.219755198
    ##  75: c -0.056672761
    ##  76: c  0.406564509
    ##  77: c -1.309429950
    ##  78: c -0.706691287
    ##  79: c  1.033891670
    ##  80: c  1.972760967
    ##  81: c  0.687525073
    ##  82: c  0.738242550
    ##  83: c -1.708270232
    ##  84: c  1.052619791
    ##  85: c  1.123239109
    ##  86: c -0.354592006
    ##  87: c  0.188238853
    ##  88: c  0.923800027
    ##  89: c  2.152700843
    ##  90: c -1.109739343
    ##  91: c  1.029508222
    ##  92: c  1.377154750
    ##  93: c  0.914811052
    ##  94: c  0.293623431
    ##  95: c -0.151738625
    ##  96: c  0.164891440
    ##  97: c  0.868577431
    ##  98: c -1.078034493
    ##  99: c -1.222331968
    ## 100: c -0.711444696
    ##      x            y

Join two data tables together
-----------------------------

Setting keys can also be useful to merge two data tables

``` r
## Greate 2 DTs
DT1 <- data.table(x=c('a','a','b','dt1'), y=1:4)
DT2 <- data.table(x=c('a','b','dt2'), z=5:7)

## Setting their keys
setkey(DT1,x)
setkey(DT2,x)

## Merge
merge(DT1,DT2)
```

    ##    x y z
    ## 1: a 1 5
    ## 2: a 2 5
    ## 3: b 3 6

Reading from mySQL
==================

To read data from **mySQL** data base you will need to download [mySQL](https://dev.mysql.com/downloads/windows/), and the `RMySQL` package. But, if you are on **windows** you will need some more steps. [Here](http://www.ahschulz.de/2013/07/23/installing-rmysql-under-windows/) is how to do it step by step.

After completing the configurations, start loading the library. `RMySQL` might need the `DBI` package, so yeah, download it.

``` r
library(DBI)
library(RMySQL)
```

Then we will use some MySQL data from UCSC. You can find more details about it and how to connect on the server [Here](https://genome.ucsc.edu/goldenPath/help/mysql.html) .

``` r
## Connect on the server
ucsc <- dbConnect(MySQL(), user="genome", host="genome-mysql.cse.ucsc.edu")

## Query the database with this sql command "show databases;"
result <- dbGetQuery(ucsc, "show databases;")

## Disconnect after finishing. Always do that after finishing.
dbDisconnect(ucsc)
```

    ## [1] TRUE

``` r
## The number of databases available
nrow(result)
```

    ## [1] 224

``` r
## Show the result: list of the first 10 the databases that are available on that server
result[1:10,]
```

    ##  [1] "information_schema" "ailMel1"            "allMis1"           
    ##  [4] "anoCar1"            "anoCar2"            "anoGam1"           
    ##  [7] "apiMel1"            "apiMel2"            "aplCal1"           
    ## [10] "aptMan1"

Now, we are going to connect on a specific dataset `hg19`

``` r
## Connect on the "hg19" database on the server
hg19 <- dbConnect(MySQL(), user="genome",db="hg19", host="genome-mysql.cse.ucsc.edu")

## Get the list of tabels names in this database
allTables <- dbListTables(hg19)
allTables[1:8] #show the first 8 names
```

    ## [1] "HInv"                   "HInvGeneMrna"          
    ## [3] "acembly"                "acemblyClass"          
    ## [5] "acemblyPep"             "affyCytoScan"          
    ## [7] "affyExonProbeAmbiguous" "affyExonProbeCore"

``` r
## Get the number of tables
length(allTables)
```

    ## [1] 11048

Now, we want to see the fields names inside a specific table, say **affyU133Plus2**, in the `hg19` database.

``` r
dbListFields(hg19, "affyU133Plus2")
```

    ##  [1] "bin"         "matches"     "misMatches"  "repMatches"  "nCount"     
    ##  [6] "qNumInsert"  "qBaseInsert" "tNumInsert"  "tBaseInsert" "strand"     
    ## [11] "qName"       "qSize"       "qStart"      "qEnd"        "tName"      
    ## [16] "tSize"       "tStart"      "tEnd"        "blockCount"  "blockSizes" 
    ## [21] "qStarts"     "tStarts"

The nice thing s that you can query the database with the regular **mySQL syntax**. So, if want to know the number of records (rows) in the **affyU133Plus2** table, we can use `select count(*) from affyU133Plus2` to get it from the database.

``` r
dbGetQuery(hg19, "select count(*) from affyU133Plus2")
```

    ##   count(*)
    ## 1    58463

We can also extract a specific table from the database.

``` r
affy <- dbReadTable(hg19,"affyU133Plus2")
head(affy)
```

    ##   bin matches misMatches repMatches nCount qNumInsert qBaseInsert
    ## 1 585     530          4          0     23          3          41
    ## 2 585    3355         17          0    109          9          67
    ## 3 585    4156         14          0     83         16          18
    ## 4 585    4667          9          0     68         21          42
    ## 5 585    5180         14          0    167         10          38
    ## 6 585     468          5          0     14          0           0
    ##   tNumInsert tBaseInsert strand        qName qSize qStart qEnd tName
    ## 1          3         898      -  225995_x_at   637      5  603  chr1
    ## 2          9       11621      -  225035_x_at  3635      0 3548  chr1
    ## 3          2          93      -  226340_x_at  4318      3 4274  chr1
    ## 4          3        5743      - 1557034_s_at  4834     48 4834  chr1
    ## 5          1          29      -    231811_at  5399      0 5399  chr1
    ## 6          0           0      -    236841_at   487      0  487  chr1
    ##       tSize tStart  tEnd blockCount
    ## 1 249250621  14361 15816          5
    ## 2 249250621  14381 29483         17
    ## 3 249250621  14399 18745         18
    ## 4 249250621  14406 24893         23
    ## 5 249250621  19688 25078         11
    ## 6 249250621  27542 28029          1
    ##                                                                   blockSizes
    ## 1                                                          93,144,229,70,21,
    ## 2              73,375,71,165,303,360,198,661,201,1,260,250,74,73,98,155,163,
    ## 3                 690,10,32,33,376,4,5,15,5,11,7,41,277,859,141,51,443,1253,
    ## 4 99,352,286,24,49,14,6,5,8,149,14,44,98,12,10,355,837,59,8,1500,133,624,58,
    ## 5                                       131,26,1300,6,4,11,4,7,358,3359,155,
    ## 6                                                                       487,
    ##                                                                                                  qStarts
    ## 1                                                                                    34,132,278,541,611,
    ## 2                        87,165,540,647,818,1123,1484,1682,2343,2545,2546,2808,3058,3133,3206,3317,3472,
    ## 3                   44,735,746,779,813,1190,1195,1201,1217,1223,1235,1243,1285,1564,2423,2565,2617,3062,
    ## 4 0,99,452,739,764,814,829,836,842,851,1001,1016,1061,1160,1173,1184,1540,2381,2441,2450,3951,4103,4728,
    ## 5                                                     0,132,159,1460,1467,1472,1484,1489,1497,1856,5244,
    ## 6                                                                                                     0,
    ##                                                                                                                                      tStarts
    ## 1                                                                                                             14361,14454,14599,14968,15795,
    ## 2                                     14381,14454,14969,15075,15240,15543,15903,16104,16853,17054,17232,17492,17914,17988,18267,24736,29320,
    ## 3                               14399,15089,15099,15131,15164,15540,15544,15549,15564,15569,15580,15587,15628,15906,16857,16998,17049,17492,
    ## 4 14406,20227,20579,20865,20889,20938,20952,20958,20963,20971,21120,21134,21178,21276,21288,21298,21653,22492,22551,22559,24059,24211,24835,
    ## 5                                                                         19688,19819,19845,21145,21151,21155,21166,21170,21177,21535,24923,
    ## 6                                                                                                                                     27542,

Here is another Query that select all records from **affyU133Plus2** table that have **misMatches** between 2 and 3. Remember that **misMatches** is a field (column) in this table.

note: when using `dbGetQuery` you don't need to use `fetch` because `dbGetQuery` combine `dbSendQuery`, `fetch` and `dbClearResult`. see [here](http://stackoverflow.com/questions/14726114/rmysql-fetch-cant-find-inherited-method).

``` r
## Send the query and get the result
affyMisQuery <- dbGetQuery(hg19, "select * from affyU133Plus2 where misMatches between 1 and 3")

## Calculate the quantile
quantile(affyMisQuery$misMatches)
```

    ##   0%  25%  50%  75% 100% 
    ##    1    1    2    2    3

``` r
## Clear the query after finishing
#dbClearResult(affyMisQuery)
```

To only get the first few rows from the dataset you can use `dbSendQuery` + `fetch`.

``` r
## Send the query
affyMisQuery <- dbSendQuery(hg19, "select * from affyU133Plus2 where misMatches between 1 and 3")

## Get the resut, but only the first 10 rows
affySmall <- fetch(affyMisQuery, n=10)
dim(affySmall) # gt the dimensions of this result
```

    ## [1] 10 22

``` r
## Clear the query after finishing
dbClearResult(affyMisQuery)
```

    ## [1] TRUE

**DON'T FORGET TO CLOSE THE CONNECTION**

``` r
dbDisconnect(hg19)
```

    ## [1] TRUE

Reading from HDF5
=================

HDF stands for **Hierarchical Data Format**. For more information see [here](https://www.hdfgroup.org/).

To install the `rhdf5` package, follow these steps

``` r
# source("http://bioconductor.org/biocLite.R")
# biocLite("rhdf5")
```

Then, load the library

``` r
library(rhdf5)

## Create HDF5 file
created <- h5createFile("sample.h5")

## Check if the file is created successfully
created
```

    ## [1] TRUE

**Creating groups**

Once you create the `.h5` file you can create groups and subgroups inside it.

``` r
## Group
created <- h5createGroup("sample.h5", "group1")
created <- h5createGroup("sample.h5", "group2")

##Sub-group
created <- h5createGroup("sample.h5", "group1/sub1")

## List the groups
h5ls("sample.h5")
```

    ##     group   name     otype dclass dim
    ## 0       / group1 H5I_GROUP           
    ## 1 /group1   sub1 H5I_GROUP           
    ## 2       / group2 H5I_GROUP

**Write data to groups**

Let's add some data into the groups we created.

``` r
## Create a matrix A, add it to group1
A = matrix(1:10, nrow = 5, ncol = 2)
h5write(A, "sample.h5", "group1/A")

## Create a multi-dimension array B, add it to group1/sub1
B = array(seq(0.1,2.0,by=0.1), dim=c(5,2,2))
h5write(B, "sample.h5", "group1/sub1/B")

## List the groups
h5ls("sample.h5")
```

    ##          group   name       otype  dclass       dim
    ## 0            / group1   H5I_GROUP                  
    ## 1      /group1      A H5I_DATASET INTEGER     5 x 2
    ## 2      /group1   sub1   H5I_GROUP                  
    ## 3 /group1/sub1      B H5I_DATASET   FLOAT 5 x 2 x 2
    ## 4            / group2   H5I_GROUP

**We can also add data at the top-level of the hierarchy**

For example, add a data frame as a top level group.

``` r
## Create the dataframe
df <- data.frame(1L:5L,seq(0,1,length.out=5),
  c("ab","cde","fghi","a","s"), stringsAsFactors=FALSE)

## add it. Note that you added the name dirctly without specifying groups
h5write(df,"sample.h5", "df")

## List the groups
h5ls("sample.h5")
```

    ##          group   name       otype   dclass       dim
    ## 0            /     df H5I_DATASET COMPOUND         5
    ## 1            / group1   H5I_GROUP                   
    ## 2      /group1      A H5I_DATASET  INTEGER     5 x 2
    ## 3      /group1   sub1   H5I_GROUP                   
    ## 4 /group1/sub1      B H5I_DATASET    FLOAT 5 x 2 x 2
    ## 5            / group2   H5I_GROUP

**Reading from `.hf` file**

Given `sample.h5` that we created in the last steps.

``` r
## Read matrix A
getA <- h5read("sample.h5", "group1/A")
getA
```

    ##      [,1] [,2]
    ## [1,]    1    6
    ## [2,]    2    7
    ## [3,]    3    8
    ## [4,]    4    9
    ## [5,]    5   10

``` r
## Read matrix B
getB <- h5read("sample.h5", "group1/sub1/B")
getB
```

    ## , , 1
    ## 
    ##      [,1] [,2]
    ## [1,]  0.1  0.6
    ## [2,]  0.2  0.7
    ## [3,]  0.3  0.8
    ## [4,]  0.4  0.9
    ## [5,]  0.5  1.0
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2]
    ## [1,]  1.1  1.6
    ## [2,]  1.2  1.7
    ## [3,]  1.3  1.8
    ## [4,]  1.4  1.9
    ## [5,]  1.5  2.0

**Writing to a specific data inside the `.h5` file**

We can add new data inside a given element inside the `h5` file. For example, we can add new row or column inside the A matrix that we created before.

``` r
## A matrix before
getA <- h5read("sample.h5", "group1/A")
getA
```

    ##      [,1] [,2]
    ## [1,]    1    6
    ## [2,]    2    7
    ## [3,]    3    8
    ## [4,]    4    9
    ## [5,]    5   10

``` r
## adding 10,20,30 in A[1:3,1]
h5write(c(10,20,30), "sample.h5", "group1/A", index=list(1:3,1))

## A matrix after
getA <- h5read("sample.h5", "group1/A")
getA
```

    ##      [,1] [,2]
    ## [1,]   10    6
    ## [2,]   20    7
    ## [3,]   30    8
    ## [4,]    4    9
    ## [5,]    5   10
