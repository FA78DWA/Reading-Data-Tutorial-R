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
    -   [Using `sqldf` package](#using-sqldf-package)
-   [Reading from HDF5](#reading-from-hdf5)
-   [Reading data from web pages](#reading-data-from-web-pages)
-   [Reading from APIs](#reading-from-apis)
    -   [Access github API](#access-github-api)
-   [Reading fixed width files](#reading-fixed-width-files)
-   [Reading `jpeg` images](#reading-jpeg-images)

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
    ## [4] "house_data.csv"      "paid.csv"            "ReadingData.md"     
    ## [7] "ReadingData.nb.html" "ReadingData.Rmd"     "simple.xml"

Use `date()` to get the downloading date.

``` r
downloadDate <- date()
downloadDate
```

    ## [1] "Wed Mar 08 11:11:51 2017"

Reading Excel Files
===================

First download the file using `download.file()`

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
download.file(url, "data.xlsx",mode="wb")
date()
```

    ## [1] "Wed Mar 08 11:11:51 2017"

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

    ##              x y           z
    ## 1:  0.21816161 a -0.19347880
    ## 2: -0.59814000 a -1.23744972
    ## 3:  0.06383391 a  0.04934657
    ## 4: -1.56283505 b  2.44343860
    ## 5: -2.04989364 b -0.68520444
    ## 6:  1.08481472 b -0.59472849
    ## 7:  0.01257747 c  0.52988607
    ## 8: -0.16857824 c -0.76810454
    ## 9: -1.16484436 c  0.16588572

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

    ##           x y        z
    ## 1: -0.59814 a -1.23745

``` r
## Get the rows with y=c
DT[DT$y=="c",]
```

    ##              x y          z
    ## 1:  0.01257747 c  0.5298861
    ## 2: -0.16857824 c -0.7681045
    ## 3: -1.16484436 c  0.1658857

``` r
## Get certain rows for example 1st, 5th, and 9th
DT[c(1,5,9),]
```

    ##             x y          z
    ## 1:  0.2181616 a -0.1934788
    ## 2: -2.0498936 b -0.6852044
    ## 3: -1.1648444 c  0.1658857

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

    ## [1] -0.19347880 -1.23744972  0.04934657  2.44343860 -0.68520444 -0.59472849
    ## [7]  0.52988607 -0.76810454  0.16588572

``` r
## Get certain columns for example 1st and 3rd
DT[,c(1,3)]
```

    ##              x           z
    ## 1:  0.21816161 -0.19347880
    ## 2: -0.59814000 -1.23744972
    ## 3:  0.06383391  0.04934657
    ## 4: -1.56283505  2.44343860
    ## 5: -2.04989364 -0.68520444
    ## 6:  1.08481472 -0.59472849
    ## 7:  0.01257747  0.52988607
    ## 8: -0.16857824 -0.76810454
    ## 9: -1.16484436  0.16588572

Operating on a subset of a data table
-------------------------------------

Until now the sub-setting either rows or columns are intuitive. `DT` is a 2-dimensional array(table), and you can get a specific element using DT\[i,j\] format just like `matlab`.

But, what if we want to take the average of the columns, or do any other operation on a subset of the `DT`. In this case **DT\[i,j,by\]** comes very handy. **DT\[i,j,by\]** means Take DT subset rows by **i**, then compute **j** grouped by **by**. Examples.

``` r
## Calculate the mean of x and sum of z
DT[,list(mean(x), sum(z))]
```

    ##            V1        V2
    ## 1: -0.4627671 -0.290409

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

    ##              x y           z           w
    ## 1:  0.21816161 a -0.19347880 0.037434047
    ## 2: -0.59814000 a -1.23744972 1.531281806
    ## 3:  0.06383391 a  0.04934657 0.002435084
    ## 4: -1.56283505 b  2.44343860 5.970392199
    ## 5: -2.04989364 b -0.68520444 0.469505119
    ## 6:  1.08481472 b -0.59472849 0.353701978
    ## 7:  0.01257747 c  0.52988607 0.280779247
    ## 8: -0.16857824 c -0.76810454 0.589984584
    ## 9: -1.16484436 c  0.16588572 0.027518073

``` r
## Add new column m = log(x+z+5). Note that we used {} to put in multi-line expression. Each expression ends with ';'
DT[,m:={tmp <- (x+z); log2(tmp+5)}]
```

    ##              x y           z           w        m
    ## 1:  0.21816161 a -0.19347880 0.037434047 2.329033
    ## 2: -0.59814000 a -1.23744972 1.531281806 1.661937
    ## 3:  0.06383391 a  0.04934657 0.002435084 2.354221
    ## 4: -1.56283505 b  2.44343860 5.970392199 2.555964
    ## 5: -2.04989364 b -0.68520444 0.469505119 1.179449
    ## 6:  1.08481472 b -0.59472849 0.353701978 2.456829
    ## 7:  0.01257747 c  0.52988607 0.280779247 2.470527
    ## 8: -0.16857824 c -0.76810454 0.589984584 2.022658
    ## 9: -1.16484436 c  0.16588572 0.027518073 2.000376

``` r
## BOLEAN OPERATIONS: Add new column a shows if x>=0 or <0
DT[,a:= x>=0]
```

    ##              x y           z           w        m     a
    ## 1:  0.21816161 a -0.19347880 0.037434047 2.329033  TRUE
    ## 2: -0.59814000 a -1.23744972 1.531281806 1.661937 FALSE
    ## 3:  0.06383391 a  0.04934657 0.002435084 2.354221  TRUE
    ## 4: -1.56283505 b  2.44343860 5.970392199 2.555964 FALSE
    ## 5: -2.04989364 b -0.68520444 0.469505119 1.179449 FALSE
    ## 6:  1.08481472 b -0.59472849 0.353701978 2.456829  TRUE
    ## 7:  0.01257747 c  0.52988607 0.280779247 2.470527  TRUE
    ## 8: -0.16857824 c -0.76810454 0.589984584 2.022658 FALSE
    ## 9: -1.16484436 c  0.16588572 0.027518073 2.000376 FALSE

``` r
## GROUPING: get the mean of(x+w) when a is TRUE and a is False, then add the result in new column b. Note that b has only 2 values.
DT[,b:=mean(x+w), by=a]
```

    ##              x y           z           w        m     a         b
    ## 1:  0.21816161 a -0.19347880 0.037434047 2.329033  TRUE 0.5134345
    ## 2: -0.59814000 a -1.23744972 1.531281806 1.661937 FALSE 0.6088781
    ## 3:  0.06383391 a  0.04934657 0.002435084 2.354221  TRUE 0.5134345
    ## 4: -1.56283505 b  2.44343860 5.970392199 2.555964 FALSE 0.6088781
    ## 5: -2.04989364 b -0.68520444 0.469505119 1.179449 FALSE 0.6088781
    ## 6:  1.08481472 b -0.59472849 0.353701978 2.456829  TRUE 0.5134345
    ## 7:  0.01257747 c  0.52988607 0.280779247 2.470527  TRUE 0.5134345
    ## 8: -0.16857824 c -0.76810454 0.589984584 2.022658 FALSE 0.6088781
    ## 9: -1.16484436 c  0.16588572 0.027518073 2.000376 FALSE 0.6088781

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

    ##              x y           z           w        m     a         b
    ## 1:  0.21816161 2 -0.19347880 0.037434047 2.329033  TRUE 0.5134345
    ## 2: -0.59814000 2 -1.23744972 1.531281806 1.661937 FALSE 0.6088781
    ## 3:  0.06383391 2  0.04934657 0.002435084 2.354221  TRUE 0.5134345
    ## 4: -1.56283505 2  2.44343860 5.970392199 2.555964 FALSE 0.6088781
    ## 5: -2.04989364 2 -0.68520444 0.469505119 1.179449 FALSE 0.6088781
    ## 6:  1.08481472 2 -0.59472849 0.353701978 2.456829  TRUE 0.5134345
    ## 7:  0.01257747 2  0.52988607 0.280779247 2.470527  TRUE 0.5134345
    ## 8: -0.16857824 2 -0.76810454 0.589984584 2.022658 FALSE 0.6088781
    ## 9: -1.16484436 2  0.16588572 0.027518073 2.000376 FALSE 0.6088781

``` r
DT2
```

    ##              x y           z           w        m     a         b
    ## 1:  0.21816161 2 -0.19347880 0.037434047 2.329033  TRUE 0.5134345
    ## 2: -0.59814000 2 -1.23744972 1.531281806 1.661937 FALSE 0.6088781
    ## 3:  0.06383391 2  0.04934657 0.002435084 2.354221  TRUE 0.5134345
    ## 4: -1.56283505 2  2.44343860 5.970392199 2.555964 FALSE 0.6088781
    ## 5: -2.04989364 2 -0.68520444 0.469505119 1.179449 FALSE 0.6088781
    ## 6:  1.08481472 2 -0.59472849 0.353701978 2.456829  TRUE 0.5134345
    ## 7:  0.01257747 2  0.52988607 0.280779247 2.470527  TRUE 0.5134345
    ## 8: -0.16857824 2 -0.76810454 0.589984584 2.022658 FALSE 0.6088781
    ## 9: -1.16484436 2  0.16588572 0.027518073 2.000376 FALSE 0.6088781

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

    ## [1] 226

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

Using `sqldf` package
---------------------

The `sqldf` package allows for execution of SQL commands on R data frames.

For this part download the [American Community Survey data](https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv) and load it into an R object called `acs`. I already downloaded it.

``` r
# library(sqldf)
# 
# acs <- read.csv("paid.csv")
# 
# ## select only the data for the probability weights pwgtp1 with ages less than 50
# qu1 <- sqldf("select pwgtp1 from acs where AGEP < 50")
# 
# ## To check our result
# R <- acs$pwgtp1[acs$AGEP < 50]
# sum(R - qu1)
# 
# ## Select the unique values from AGEP 
# qu2 <- sqldf("select distinct AGEP from acs")
# 
# ## To check our result
# u <- unique(acs$AGEP)
# sum(u - qu2)
```

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
## Read array  B
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

Reading data from web pages
===========================

We can read the `html` code from the websites and extract data from it, this is called **webscraping**. To do that, first set a connection to the `url`, then `readlines`, and finaly close the connection.

``` r
## set the connection
myConnection <- url("http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en")

## Read lines
htmlCode <- readLines(myConnection)
```

    ## Warning in readLines(myConnection): incomplete final line found on 'http://
    ## scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en'

``` r
## close the connection
close(myConnection)

## show the lines 
htmlCode
```

    ## [1] "<!doctype html><head><meta http-equiv=\"Content-Type\" content=\"text/html;charset=ISO-8859-1\"><meta http-equiv=\"X-UA-Compatible\" content=\"IE=Edge\"><meta name=\"referrer\" content=\"always\"><meta name=\"viewport\" content=\"width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=2\"><style>@viewport{width:device-width;min-zoom:1;max-zoom:2;}</style><meta name=\"format-detection\" content=\"telephone=no\"><style>html,body,form,table,div,h1,h2,h3,h4,h5,h6,img,ol,ul,li,button{margin:0;padding:0;border:0;}table{border-collapse:collapse;border-width:0;empty-cells:show;}#gs_top{position:relative;min-width:964px;-webkit-tap-highlight-color:rgba(0,0,0,0);}#gs_top>*:not(#x){-webkit-tap-highlight-color:rgba(204,204,204,.5);}.gs_el_ph #gs_top,.gs_el_ta #gs_top{min-width:300px;}#gs_top.gs_nscl{position:fixed;width:100%;}body,td,input{font-size:13px;font-family:Arial,sans-serif;line-height:1.24}body{background:#fff;color:#222;-webkit-text-size-adjust:100%;-moz-text-size-adjust:none;}.gs_gray{color:#777777}.gs_red{color:#dd4b39}.gs_grn{color:#006621}.gs_lil{font-size:11px}.gs_med{font-size:16px}.gs_hlt{font-weight:bold;}a:link{color:#1a0dab;text-decoration:none}a:visited{color:#660099;text-decoration:none}a:hover,a:active,a:hover .gs_lbl,a:active .gs_lbl,a .gs_lbl:active{text-decoration:underline;outline:none;}a:active,a:active .gs_lbl,a .gs_lbl:active{color:#d14836}.gs_a,.gs_a a:link,.gs_a a:visited{color:#006621}.gs_a a:active{color:#d14836}a.gs_fl:link,.gs_fl a:link{color:#1a0dab}a.gs_fl:visited,.gs_fl a:visited{color:#660099}a.gs_fl:active,.gs_fl a:active{color:#d14836}.gs_fl{color:#777777}.gs_ctc,.gs_ctu{vertical-align:middle;font-size:11px;font-weight:bold}.gs_ctc{color:#1a0dab}.gs_ctg,.gs_ctg2{font-size:13px;font-weight:bold}.gs_ctg{color:#1a0dab}a.gs_pda,.gs_pda a{padding:7px 0 5px 0}.gs_alrt{background:#f9edbe;border:1px solid #f0c36d;padding:0 16px;text-align:center;-webkit-box-shadow:0 2px 4px rgba(0,0,0,.2);-moz-box-shadow:0 2px 4px rgba(0,0,0,.2);box-shadow:0 2px 4px rgba(0,0,0,.2);-webkit-border-radius:2px;-moz-border-radius:2px;border-radius:2px;}.gs_spc{display:inline-block;width:12px}.gs_br{width:0;font-size:0}.gs_ibl{display:inline-block;}.gs_scl:after{content:\"\";display:table;clear:both;}.gs_ind{padding-left:8px;text-indent:-8px}.gs_ico,.gs_icm{display:inline-block;background:no-repeat url(/intl/en/scholar/images/1x/sprite_20161020.png);background-size:169px;width:21px;height:21px;}@media(-webkit-min-device-pixel-ratio:1.5),(min-resolution:144dpi){.gs_ico,.gs_icm{background-image:url(/intl/en/scholar/images/2x/sprite_20161020.png);}}.gs_el_ta .gs_nta,.gs_ota,.gs_el_ph .gs_nph,.gs_oph{display:none}.gs_el_ta .gs_ota,.gs_el_ph .gs_oph{display:inline}.gs_el_ta div.gs_ota,.gs_el_ph div.gs_oph{display:block}#gs_ftr{margin:32px 0 0 0;text-align:center;clear:both;}#gs_ftr a{display:inline-block;margin:0 12px;padding:7px 0 8px 0;}#gs_ftr a:link,#gs_ftr a:visited{color:#1a0dab}#gs_ftr a:active{color:#d14836}.gs_in_txt{color:black;background:#fff;font-size:16px;height:23px;line-height:23px;border:1px solid #d9d9d9;border-top:1px solid #c0c0c0;padding:3px 6px 1px 8px;-webkit-border-radius:1px;-moz-border-radius:1px;border-radius:1px;outline:none;vertical-align:middle;-webkit-appearance:none;-moz-appearance:none;}.gs_el_tc .gs_in_txt{font-size:18px;}.gs_in_txt:hover{border:1px solid #b9b9b9;border-top:1px solid #a0a0a0;-webkit-box-shadow:inset 0px 1px 2px rgba(0,0,0,0.1);-moz-box-shadow:inset 0px 1px 2px rgba(0,0,0,0.1);box-shadow:inset 0px 1px 2px rgba(0,0,0,0.1);}.gs_in_txte,.gs_in_txte:hover{border:1px solid #DD4B39;}.gs_in_txt:focus{border:1px solid #4d90fe;-webkit-box-shadow:inset 0px 1px 2px rgba(0,0,0,0.3);-moz-box-shadow:inset 0px 1px 2px rgba(0,0,0,0.3);box-shadow:inset 0px 1px 2px rgba(0,0,0,0.3);}.gs_in_txt:disabled{color:#b8b8b8;border-color:#f1f1f1;-webkit-box-shadow:none;-moz-box-shadow:none;box-shadow:none;}input.gs_mini{font-size:13px;height:16px;line-height:16px;padding:3px 6px;}.gs_el_tc input.gs_mini{font-size:13px;height:21px;line-height:21px;}.gs_in_txtf{margin-right:16px}.gs_in_txtm{margin-right:14px}.gs_in_txtf .gs_in_txt,.gs_in_txtm .gs_in_txt{width:100%;}.gs_in_txts{font-size:13px;line-height:18px;}button{position:relative;z-index:1;box-sizing:border-box;font-size:11px;font-weight:bold;cursor:default;height:29px;min-width:72px;overflow:visible;color:#444;border:1px solid #dcdcdc;border:1px solid rgba(0,0,0,.1);border-radius:2px;text-align:center;background-color:#f5f5f5;background-image:linear-gradient(to bottom,#f5f5f5,#f1f1f1);transition:all .218s;user-select:none;}button .gs_wr{line-height:27px;}button.gs_btn_mini{min-width:26px;height:26px;}.gs_btn_mini .gs_wr{line-height:24px;}.gs_btn_half,.gs_el_ph .gs_btn_hph{min-width:36px;}>. }}.gs_btn_slt{border-radius:2px 0 0 2px;}.gs_btn_srt{margin-left:-1px;border-radius:0 2px 2px 0;}.gs_btn_smd{margin-left:-1px;border-radius:0;}button:hover,button.gs_in_cb:hover{z-index:2;color:#222;border:1px solid #c6c6c6;box-shadow:0 1px 1px rgba(0,0,0,.1);background-color:#f8f8f8;background-image:linear-gradient(to bottom,#f8f8f8,#f1f1f1);transition:all 0s;}button.gs_sel{color:#333;border:1px solid #ccc;box-shadow:inset 0 1px 2px rgba(0,0,0,.1);background-color:#e8e8e8;background-image:linear-gradient(to bottom,#eee,#e0e0e0);}button.gs_in_cb{color:#444;border:1px solid #dcdcdc;border:1px solid rgba(0,0,0,.1);box-shadow:none;background-color:#f5f5f5;background-image:linear-gradient(to bottom,#f5f5f5,#f1f1f1);}button:active,button.gs_in_cb:active{z-index:2;color:#333;background-color:#f6f6f6;background-image:linear-gradient(to bottom,#f6f6f6,#f1f1f1);box-shadow:inset 0px 1px 2px rgba(0,0,0,.1);}button:focus,button.gs_in_cb:focus{z-index:2;outline:none;border:1px solid #4d90fe;}button::-moz-focus-inner{padding:0;border:0}button .gs_lbl{padding:0px 8px;}a.gs_in_ib{position:relative;display:inline-block;line-height:16px;padding:5px 0 6px 0;user-select:none;}a.gs_btn_mini{height:24px;line-height:24px;padding:0;}a .gs_lbl,button .gs_lbl{vertical-align:baseline;}a.gs_in_ib .gs_lbl{display:inline-block;padding-left:21px}button.gs_in_ib .gs_lbl{padding-left:29px;}button.gs_btn_mini .gs_lbl,button.gs_btn_half .gs_lbl,button.gs_btn_eml .gs_lbl{padding:11px;}.gs_el_ph .gs_btn_hph .gs_lbl,.gs_el_ph .gs_btn_cph .gs_lbl{padding:11px;font-size:0;visibility:hidden;}.gs_in_ib .gs_ico{position:absolute;top:2px;left:8px;}.gs_btn_mini .gs_ico{top:1px;left:2px;}.gs_btn_half .gs_ico,.gs_el_ph .gs_btn_hph .gs_ico{left:7px}.gs_btn_eml .gs_ico,.gs_el_ph .gs_btn_cph .gs_ico{left:25px}.gs_btn_eml.gs_btn_mnu .gs_ico{left:20px;}a.gs_in_ib .gs_ico{top:3px;left:0}a.gs_in_ib.gs_md_li .gs_ico{left:14px}.gs_el_tc a.gs_in_ib.gs_md_li .gs_ico{top:11px}a.gs_btn_mini .gs_ico{top:1px;left:0}button .gs_cb_wr{position:absolute;top:5px;left:26px;}.gs_el_tc button .gs_cb_wr{top:-3px;}.gs_in_ib .gs_ico{opacity:.55;}a.gs_in_ib .gs_ico{opacity:.65;}.gs_in_ib:hover .gs_ico{opacity:.72;}.gs_in_ib:active .gs_ico,.gs_in_ib .gs_ico:active,.gs_in_ib :active~.gs_ico{opacity:1;}.gs_in_ib:disabled .gs_ico{opacity:.28;}a.gs_in_ib.gs_dis .gs_ico{opacity:.33;}button.gs_btn_act{color:#fff;border:1px solid #3079ed;background-color:#4d90fe;background-image:linear-gradient(to bottom,#4d90fe,#4787ed);}button.gs_btn_act:hover{color:#fff;border:1px solid #2f5bb7;background-color:#357ae8;background-image:linear-gradient(to bottom,#4d90fe,#357ae8);box-shadow:inset 0 1px 1px rgba(0,0,0,.1);}button.gs_btnG{width:70px;min-width:0;}button.gs_btn_cre{color:#fff;border:1px solid transparent;background-color:#d14836;background-image:linear-gradient(to bottom,#dd4b39,#d14836);}button.gs_btn_cre .gs_lbl{text-transform:uppercase;text-shadow:0px 1px rgba(0,0,0,.1);}button.gs_btn_cre:hover{color:#fff;border:1px solid #b0281a;border-bottom:1px solid #af301f;background-color:#c53727;background-image:linear-gradient(to bottom,#dd4b39,#c53727);box-shadow:inset 0 1px 1px rgba(0,0,0,.2);}button.gs_btn_act:focus,button.gs_btn_cre:focus{box-shadow:inset 0 0 0 1px rgba(255,255,255,.5);}button.gs_btn_act:focus{border-color:#404040;}button.gs_btn_cre:focus{border-color:#d14836;}button.gs_btn_act:active{border:1px solid #315da3;background-color:#2f6de1;background-image:linear-gradient(to bottom,#4d90fe,#2f6de1);}button.gs_btn_cre:active{border:1px solid #992a1b;background-color:#b0281a;background-image:linear-gradient(to bottom,#dd4b39,#b0281a);}button.gs_btn_act:active,button.gs_btn_cre:active{box-shadow:inset 0 1px 2px rgba(0,0,0,.3);}button.gs_bsp,button.gs_bsp:hover,button.gs_bsp:active,button.gs_bsp:focus,button:disabled,button:disabled:hover,button:disabled:active{color:#b8b8b8;border:1px solid #f3f3f3;border:1px solid rgba(0,0,0,.05);background:none;box-shadow:none;z-index:0;}button.gs_btn_act:disabled{color:white;border-color:#98bcf6;background:#a6c8ff;}button.gs_btn_cre:disabled{color:white;border-color:#d8948d;background:#e8a49b;}button:disabled:active{box-shadow:inset 0 1px 2px rgba(0,0,0,.1);z-index:2;}a.gs_dis{cursor:default}.gs_ttp{position:absolute;top:100%;right:50%;z-index:10;pointer-events:none;visibility:hidden;opacity:0;transition:visibility 0s .13s,opacity .13s ease-out;}button:hover .gs_ttp,button:focus .gs_ttp,a:hover .gs_ttp,a:focus .gs_ttp{transition:visibility 0s .3s,opacity .13s ease-in .3s;visibility:visible;opacity:1;}button.gs_sel .gs_ttp{visibility:hidden;}.gs_ttp .gs_aro,.gs_ttp .gs_aru{position:absolute;top:-2px;right:-5px;width:0;height:0;line-height:0;font-size:0;border:5px solid transparent;border-top:none;border-bottom-color:#2a2a2a;z-index:1;}.gs_ttp .gs_aro{top:-3px;right:-6px;border-width:6px;border-top:none;border-bottom-color:white;}.gs_ttp .gs_txt{display:block;position:relative;top:2px;right:-50%;padding:7px 9px;background:#2a2a2a;color:white;font-size:11px;font-weight:bold;line-height:normal;white-space:nowrap;border:1px solid white;box-shadow:inset 0 1px 4px rgba(0,0,0,.2);}.gs_in_se,.gs_tan{touch-action:none;}.gs_in_se .gs_lbl{padding-left:8px;padding-right:22px;}.gs_in_se .gs_icm{position:absolute;top:8px;right:8px;width:7px;height:11px;background-position:-21px -88px;opacity:.55;}a.gs_in_se .gs_icm{opacity:.65;}.gs_in_se:hover .gs_icm{opacity:.72;}.gs_in_se:active .gs_icm,.gs_in_se .gs_icm:active,.gs_in_se :active~.gs_icm{opacity:1;}.gs_in_ib:disabled .gs_icm{opacity:.28;}.gs_el_ph .gs_btn_hph .gs_icm,.gs_el_ph .gs_btn_cph .gs_icm{display:none}.gs_btn_mnu .gs_icm{top:11px;height:7px;background-position:0 -110px;}.gs_btn_mn2 .gs_icm,.gs_btn_mn2:hover .gs_icm,.gs_btn_mn2:active .gs_icm,.gs_btn_mn2 .gs_icm:active,.gs_btn_mn2 :active~.gs_icm{background-position:-42px -44px;opacity:1;}.gs_btn_mn2 .gs_ico,.gs_btn_mn2:hover .gs_ico,.gs_btn_mn2:active .gs_ico,.gs_btn_mn2 .gs_ico:active,.gs_btn_mn2 :active~.gs_ico{opacity:1;}.gs_md_se,.gs_md_wn,.gs_md_wm{position:absolute;top:0;left:0;border:1px solid #ccc;border-color:rgba(0,0,0,.2);background:#fff;box-shadow:0 2px 4px rgba(0,0,0,.2);z-index:1100;opacity:0;}.gs_md_se,.gs_md_wn{display:none;transition:opacity .13s;}.gs_md_wm{visibility:hidden;max-height:0;transition:opacity .13s,visibility 0s .13s,max-height 0s .13s;}.gs_md_se.gs_vis,.gs_md_wn.gs_vis,.gs_md_wm.gs_vis{opacity:1;transition:all 0s;}.gs_md_wm.gs_vis{visibility:visible;max-height:10000px;}.gs_el_tc .gs_md_se,.gs_el_tc .gs_md_wn{transform-origin:100% 0;transform:scale(1,0);transition:opacity .218s ease-out,transform 0s .218s;}.gs_el_tc .gs_md_wm{transition:opacity .218s ease-out,transform 0s .218s,visibility 0s .218s,max-height 0s .218s;}.gs_el_tc .gs_md_wn.gs_ttss{transform:scale(0,1);}.gs_el_tc .gs_md_wm,.gs_el_tc .gs_md_wn.gs_ttzi,.gs_el_ph.gs_el_tc .gs_md_wp.gs_ttzi{transform-origin:50% 50%;transform:scale(0,0);}.gs_el_tc .gs_md_se.gs_vis,.gs_el_tc .gs_md_wn.gs_vis,.gs_el_tc .gs_md_wm.gs_vis{transform:scale(1,1);transition:transform .218s ease-out;}.gs_el_ph .gs_md_wmw{top:0;left:0;position:fixed;height:100%;width:100%;visibility:hidden;z-index:1100;}.gs_el_ph .gs_md_wm{padding:0;width:100%;height:100%;border:none;box-shadow:none;transform:translate(0,100%);transform:translate(0,100vh);transition:transform .218s ease-out,opacity 0s .218s,visibility 0s .218s,max-height 0s .218s;}.gs_el_ph .gs_md_wm.gs_vis{transform:translate(0,0);transition:transform .218s ease-out;}.gs_md_se{white-space:nowrap}.gs_md_se ul{list-style-type:none;word-wrap:break-word;display:inline-block;vertical-align:top;}.gs_md_seb ul{display:block}.gs_md_se li,.gs_md_li,.gs_md_li:link,.gs_md_li:visited{display:block;padding:6px 44px 6px 16px;font-size:13px;line-height:16px;color:#222;cursor:default;text-decoration:none;background:white;transition:background .13s;}a.gs_md_li:hover .gs_lbl,a.gs_md_li:active .gs_lbl{text-decoration:none}.gs_el_tc .gs_md_se li,.gs_el_tc .gs_md_li{padding-top:14px;padding-bottom:10px;}.gs_md_se li:focus,.gs_md_se li:hover,.gs_md_li:hover,.gs_md_li:focus{background:#f1f1f1;transition:background 0s;}.gs_md_se li.gs_sel{color:#dd4b39}.gs_md_wn:focus,.gs_md_se li:focus,.gs_md_li:focus{outline:none}button.gs_btnG .gs_ico,button.gs_btnG:hover .gs_ico,button.gs_btnG:active .gs_ico,button.gs_btnG .gs_ico:active,button.gs_btnG :active~.gs_ico{width:14px;height:14px;top:7px;left:27px;background-position:-113px -44px;opacity:1;}button .gs_bsc{position:absolute;top:0;right:50%;width:25px;}button .gs_bs{background:url('/intl/en/scholar/images/spinner.gif') no-repeat;position:relative;height:25px;width:25px;left:50%;display:none;}button.gs_bsp .gs_bs{display:block;}a.gs_in_cb:link,a.gs_in_cb:visited,a.gs_in_cb:active,a.gs_in_cb:hover,a.gs_in_cb.gs_dis:active .gs_lbl{cursor:default;color:#222;text-decoration:none;}.gs_in_cb,.gs_in_ra{position:relative;line-height:16px;display:inline-block;user-select:none;}.gs_in_cb.gs_md_li{padding:6px 44px 6px 16px;display:block;}.gs_in_cb input,.gs_in_ra input{position:absolute;top:1px;left:1px;width:15px;height:15px;margin:0;padding:0;opacity:0;z-index:2;}.gs_in_ra input{top:0;left:0}.gs_el_tc .gs_in_cb input{top:9px}.gs_el_tc .gs_in_ra input{top:8px}.gs_in_cb.gs_in_cbj input{top:15px;left:15px}.gs_in_cb label,.gs_in_cb .gs_lbl,.gs_in_ra label{display:inline-block;padding-left:21px;min-height:16px;}.gs_el_tc .gs_in_cb label,.gs_el_tc .gs_in_cb .gs_lbl,.gs_el_tc .gs_in_ra label{padding-top:8px;padding-bottom:5px;}.gs_in_cb.gs_in_cbj label,.gs_in_cb.gs_in_cbj .gs_lbl{padding:13px 0 12px 41px;}.gs_in_cb .gs_cbx,.gs_in_ra .gs_cbx{position:absolute}.gs_in_cb .gs_cbx{top:2px;left:2px;width:11px;height:11px;border:1px solid #c6c6c6;border-radius:1px;}.gs_md_li .gs_cbx{top:8px;left:18px}.gs_el_tc .gs_in_cb .gs_cbx{top:10px}.gs_el_tc .gs_md_li .gs_cbx{top:16px}.gs_in_cb.gs_in_cbj .gs_cbx{top:15px;left:15px}.gs_el_tc .gs_in_ra .gs_cbx{top:8px}.gs_in_ra .gs_cbx{top:0;left:0;border:1px solid #c6c6c6;width:13px;height:13px;border-radius:7px;}.gs_in_cb:hover .gs_cbx,.gs_in_ra:hover .gs_cbx{border-color:#666;box-shadow:inset 0 1px 1px rgba(0,0,0,.1);}button.gs_in_cb:hover .gs_cbx{border-color:#c6c6c6;}button.gs_in_cb:active .gs_cbx{border-color:#a6a6a6;}.gs_in_cb:focus .gs_cbx,.gs_in_cb :focus~.gs_cbx,.gs_in_ra :focus~.gs_cbx{border-color:#4d90fe;}.gs_in_cb:active .gs_cbx,.gs_in_ra:active .gs_cbx,.gs_in_cb .gs_cbx:active,.gs_in_ra .gs_cbx:active,.gs_in_cb :active~.gs_cbx,.gs_in_ra :active~.gs_cbx{border-color:#666;background:#ebebeb;}.gs_in_cb :disabled~.gs_cbx,.gs_in_ra :disabled~.gs_cbx{border-color:#f1f1f1;box-shadow:none;}.gs_in_cb :disabled~label,.gs_in_ra :disabled~label{color:#b8b8b8;}.gs_in_cb.gs_err .gs_cbx{border-color:#eda29b;}.gs_in_cb .gs_chk,.gs_in_ra .gs_chk{position:absolute;z-index:1;top:-3px;left:-2px;width:21px;height:21px;}.gs_md_li .gs_chk{top:3px;left:14px}.gs_el_tc .gs_in_cb .gs_chk{top:5px}.gs_el_tc .gs_md_li .gs_chk{top:11px}.gs_in_cb.gs_in_cbj .gs_chk{top:10px;left:11px}.gs_in_ra .gs_chk{top:4px;left:4px;width:7px;height:7px;}.gs_el_tc .gs_in_ra .gs_chk{top:12px}.gs_in_cb input:checked~.gs_chk,.gs_in_cb.gs_sel .gs_chk{background:no-repeat url(/intl/en/scholar/images/1x/sprite_20161020.png) -69px -67px;opacity:.62;}.gs_in_ra input:checked~.gs_chk{border-radius:4px;background:#666;}.gs_in_cb.gs_par .gs_chk{background:no-repeat url(/intl/en/scholar/images/1x/sprite_20161020.png) -21px -44px;opacity:.55;}@media(-webkit-min-device-pixel-ratio:1.5),(min-resolution:144dpi){.gs_in_cb input:checked~.gs_chk,.gs_in_cb.gs_sel .gs_chk,.gs_in_cb.gs_par .gs_chk{background-image:url(/intl/en/scholar/images/2x/sprite_20161020.png);background-size:169px;}}.gs_in_cb input:checked:disabled~.gs_chk{background-position:-69px -67px;opacity:.22;}.gs_ico_X{background-position:-71px 0;opacity:.55;}.gs_ico_X:hover{opacity:.72;}.gs_ico_X:active{opacity:1;}.gs_el_tc .gs_ico_Xt{-webkit-background-origin:content;background-origin:content-box;-webkit-background-clip:content;background-clip:content-box;padding:10px 6px 10px 14px;}.gs_ico_P{background-position:0 0;opacity:.55;}.gs_ico_P:hover{opacity:.72;}.gs_ico_P:active{opacity:1;}.gs_btnC .gs_ico{background-position:0 -66px;}.gs_btnM .gs_ico{background-position:-92px 0;}.gs_btnMW .gs_ico{background-position:-21px -22px;}.gs_btnSB .gs_ico{background-position:0 -44px;}.gs_btnPL .gs_ico{background-position:-148px -66px;}.gs_btnPR .gs_ico{background-position:-21px -66px;}.gs_btnDE .gs_ico{background-position:-134px 0;}.gs_btnADD .gs_ico{background-position:-92px -66px;}.gs_btnMRG .gs_ico{background-position:-113px 0;}.gs_btnDWL .gs_ico{background-position:-28px -88px;}.gs_btnMNU .gs_ico{background-position:0 -88px;}#gsc_bdy{position:relative;margin:0 auto;width:1100px;}.gs_el_me #gsc_bdy{width:1028px}.gs_el_sm #gsc_bdy{width:964px}.gs_el_ph #gsc_bdy,.gs_el_ta #gsc_bdy{width:auto;margin-top:44px;}.gsc_lcl{position:relative;margin:0 337px 0 16px;}.gs_el_sm .gsc_lcl{margin-right:321px;}.gs_el_ta .gsc_lcl,.gs_el_ph .gsc_lcl{margin:0}#gsc_prf,#gsc_a_t{border-right:1px solid #ccc}.gs_el_ta #gsc_prf,.gs_el_ta #gsc_a_t,.gs_el_ph #gsc_prf,.gs_el_ph #gsc_a_t{border:none;}#gsc_rsb{position:absolute;top:0;right:0;width:336px;}.gs_el_sm #gsc_rsb{width:320px;}.gs_el_ta #gsc_rsb{position:relative;width:49%;float:right;margin:8px 0;}.gs_el_ta .gsc_prf_ed #gsc_rsb{display:none;}.gs_el_ph #gsc_rsb{position:relative;width:100%;}#gsc_rsb_m{display:none}.gs_el_ta #gsc_rsb_m,.gs_el_ph #gsc_rsb_m{display:block;position:absolute;top:-44px;width:100%;height:43px;border-bottom:1px solid #ccc;}.gs_el_ta #gsc_rsb_icol,.gs_el_ph #gsc_rsb_icol,.gs_el_ta #gsc_rsb_f,.gs_el_ph #gsc_rsb_f,.gs_el_ta #gsc_rsb_gpl,.gs_el_ph #gsc_rsb_gpl{display:none;}.gsc_rsb_s{margin:20px 24px 24px 24px;position:relative;}.gs_el_sm .gsc_rsb_s{margin:8px 16px 16px 16px}.gs_el_ta .gsc_rsb_s{margin:8px 16px}.gs_el_ph .gsc_rsb_s{margin:8px}.gsc_rsb_h{font-weight:bold;font-size:13px;border-bottom:1px solid #ccc;padding-bottom:4px;margin-bottom:4px;}.gs_el_sm .gsc_rsb_h{margin-bottom:0;}#gsc_rsb_icol,.gs_el_ta #gsc_rsb_icol_m{display:inline-block;width:189px;height:30px;margin:8px 0;background:no-repeat url('/intl/en/scholar/images/1x/scholar_logo_30dp.png');background-size:189px 30px;}.gs_el_ta #gsc_rsb_icol_m{margin:8px 16px;position:relative;z-index:1;}@media(-webkit-min-device-pixel-ratio:1.5),(min-resolution:144dpi){#gsc_rsb_icol,.gs_el_ta #gsc_rsb_icol_m{background-image:url('/intl/en/scholar/images/2x/scholar_logo_30dp.png');}}.gs_el_ph #gsc_rsb_icol_m{color:#dd4b39;font-size:20px;line-height:43px;margin:0 8px;position:relative;z-index:1;}.gs_el_ta #gsc_rsb_f_m,.gs_el_ph #gsc_rsb_f_m{position:absolute;top:7px;right:8px;z-index:2;}#gsc_rsb_fin,#gsc_rsb_fin_m,#gsc_rsb_fbt,#gsc_rsb_fbt_m{vertical-align:top}#gsc_rsb_fin,#gsc_rsb_fin_m{width:228px;border-radius:2px 0 0 2px;}.gs_el_ta #gsc_rsb_fin_m{width:240px}.gs_el_ph #gsc_rsb_fin_m{width:170px}#gsc_rsb_fbt,#gsc_rsb_fbt_m{margin-left:-1px;border-radius:0 2px 2px 0;}.gsc_rsb_foff #gsc_rsb_fin_m{display:none;}.gs_el_tc #gsc_rsb_fin_m{display:inline-block;transform:scale(1,1);visibility:visible;transform-origin:right;transition:visibility 0s,transform .218s;}.gs_el_tc .gsc_rsb_foff #gsc_rsb_fin_m{transform:scale(0,1);visibility:hidden;transition:visibility 0s .218s,transform .218s;}.gs_el_tc #gsc_rsb_f_m.gsc_rsb_foff{z-index:0;transition:z-index 0s .218s;}#gsc_rsb_gpl{margin:16px 0;text-align:center;}#gsc_rsb_st{width:100%;max-width:320px}.gs_el_ph #gsc_rsb_st{width:288px}#gsc_rsb_t{display:none}.gsc_rsb_std{text-align:right;padding-right:8px;}.gsc_rsb_sc1{text-align:left;}.gsc_rsb_sth{font-weight:normal;padding-bottom:4px;padding-right:8px;border-bottom:1px solid #ccc;text-align:right;}th.gsc_rsb_sc1{border-bottom:1px solid #ccc;padding-bottom:4px;font-weight:bold;font-size:13px;}.gsc_rsb_f:link,.gsc_rsb_f:visited{color:#222;}#gsc_g{position:relative;width:288px;height:100px;border:1px solid white;}#gsc_g:hover{border:1px solid #ccc;background:#f5f5f5;}#gsc_g:active{border:1px solid #4d90fe;background:#f1f1f1;}.gs_el_ta #gsc_g,.gs_el_ph #gsc_g{display:none}#gsc_g_x,#gsc_g_bars{position:absolute;bottom:13px;left:0;width:288px;height:57px;}#gsc_g_x{height:13px;border-top:1px solid #777;}.gsc_g_t{position:absolute;bottom:0;color:#777;font-size:11px;}.gsc_g_a{position:absolute;bottom:13px;width:15px;background:#777;}.gsc_g_a:hover,.gsc_g_a:focus,.gsc_g_a:active{text-decoration:none;cursor:default;}.gsc_g_al{position:absolute;bottom:15px;left:7px;color:#222;background:white;font-size:11px;padding:1px;border:1px solid #777;border-radius:1px;visibility:hidden;opacity:0;transition:opacity .218s,visibility 0s .218s;}.gsc_g_a:hover .gsc_g_al,.gsc_g_a:focus .gsc_g_al,.gsc_g_a:active .gsc_g_al{visibility:visible;opacity:1;transition:all 0s;}.gsc_rsb_a{position:relative;list-style:none;}.gsc_rsb_aa:hover{background:#f1f1f1;}span.gsc_rsb_aa:hover{background:white;}.gsc_rsb_lc{margin-left:12px;font-weight:normal;}.gsc_rsb_sp{display:none}.gs_el_ph .gsc_rsb_sp,.gs_el_ta .gsc_rsb_sp{display:inline;margin-right:8px;}.gs_el_ph #gsc_rsb_co .gsc_rsb_a,.gs_el_ta #gsc_rsb_co .gsc_rsb_a{width:auto;overflow-x:hidden;white-space:nowrap;}.gs_el_ph #gsc_rsb_co .gsc_rsb_aa,.gs_el_ta #gsc_rsb_co .gsc_rsb_aa{display:inline-block;}.gs_el_ph #gsc_rsb_co .gsc_rsb_a li,.gs_el_ta #gsc_rsb_co .gsc_rsb_a li{display:inline;}.gsc_rsb_fade{display:none}.gs_el_ph #gsc_rsb_co .gsc_rsb_fade,.gs_el_ta #gsc_rsb_co .gsc_rsb_fade{position:absolute;right:0;bottom:0;width:60px;height:100%;background-image:linear-gradient(to right,rgba(255,255,255,0),rgba(255,255,255,1) 80%);}.gsc_rsb_aa{display:block;padding:4px 0;line-height:17px;}.gs_el_tc .gsc_rsb_aa{line-height:26px}.gsc_rsb_aa:hover,.gsc_rsb_aa:active{text-decoration:none;}#gsc_rsb_ssc .gsc_rsb_h{margin-bottom:0}#gsc_rsb_ssc li{position:relative;border-bottom:1px solid #f1f1f1;}.gs_el_ta #gsc_rsb_ssc,.gs_el_ph #gsc_rsb_ssc,#gsc_rsb_vc{display:none}.gs_el_ta #gsc_rsb_vc,.gs_el_ph #gsc_rsb_vc{display:block;text-align:center;background:#4d90fe;border:1px solid #ccc;margin:0 7px;}.gsc_rsb_vca:link,.gsc_rsb_vca:visited{font-size:13px;font-weight:bold;display:block;color:#eeeeee;text-decoration:underline;padding:16px 8px;}.gsc_rsb_t{position:absolute;top:4px;right:0;width:58px;height:21px;}.gs_el_tc .gsc_rsb_t{top:10px}.gsc_rsb_t .gs_ico{margin-left:8px;}#gsc_rsb_ssc .gsc_rsb_aa{padding:6px 0;margin-right:58px;word-break:break-word;}#gsc_dscl{margin:0 0 8px 0;color:#777;font-style:italic;}#gsc_ftr_h{padding-bottom:13px;}#gsc_prf{padding:24px 8px 16px 0;}.gs_el_sm #gsc_prf{padding:16px 0 8px 0;}.gs_el_ta #gsc_prf{padding:16px 0 8px 16px;}.gs_el_ph #gsc_prf{padding:16px 8px 8px 8px;}.gs_el_ta #gsc_prf_w{width:50%;float:left;}.gs_el_ta .gsc_prf_ed #gsc_prf_w{width:80%;}#gsc_art{clear:both}#gsc_prf_pu{float:left;width:150px;text-align:center;background:#f8f8f8;margin:0 8px 8px 0;}#gsc_prf_pu form{padding:8px 8px 0 8px;background:white;}.gs_el_ph #gsc_prf_pu{width:75px}.gs_el_ta #gsc_prf_pu{width:100px}.gs_el_ta #gsc_prf_pu form,.gs_el_ph #gsc_prf_pu form{padding:4px 4px 0 4px}.gs_el_ph .gsc_prf_ed #gsc_prf_pu{display:none;}#gsc_prf_pua{display:block;line-height:0;}#gsc_prf_pufi{width:0;height:0;overflow:hidden;}.gsc_prf_pufo #gsc_prf_pufi{width:auto;height:auto;overflow:visible;position:relative;z-index:1;}.gsc_prf_pufo #gsc_prf_pufi2{display:inline-block;background:#fcfcfc;padding:8px 8px 8px 0;}#gsc_prf_pufb{word-wrap:break-word;}.gsc_prf_pufo #gsc_prf_pufb{display:none;}#gsc_prf_e,.gsc_prf_ed #gsc_prf_i{display:none}.gsc_prf_ed #gsc_prf_e{display:block;max-width:400px}#gsc_prf_i,#gsc_prf_e,#gsc_prf_iv{margin:0 16px 0 166px;}.gs_el_sm #gsc_prf_i,.gs_el_sm #gsc_prf_e,.gs_el_sm #gsc_prf_iv{margin:0 8px 0 158px;}.gs_el_ph #gsc_prf_i,.gs_el_ph #gsc_prf_e{margin:0 8px 0 0;}.gs_el_ta #gsc_prf_i{margin:0 8px 0 108px;}.gs_el_ta #gsc_prf_e{margin:0 8px 0 108px;}#gsc_prf_ib{float:right;position:relative;white-space:nowrap;}.gs_el_ta #gsc_prf_ib,.gs_el_ph #gsc_prf_ib{position:absolute;top:-37px;left:0;width:200%;height:0;}.gs_el_ph #gsc_prf_ib{width:100%;}#gsc_prf_ibi,#gsc_prf_ibi .gs_ibl{padding-left:16px;}.gs_el_ta #gsc_prf_ibi,.gs_el_ph #gsc_prf_ibi{text-align:right;padding-left:0;margin-right:60px;}.gs_el_ph #gsc_prf_ibi{margin-right:52px;}.gs_el_ph #gsc_prf_ibi .gs_ibl{padding-left:8px;}#gsc_prf_in{font-size:24px;line-height:24px;padding:3px 0 7px 0;}.gs_el_ta #gsc_prf_in,.gs_el_ph #gsc_prf_in{padding:0 0 2px 0;}.gsc_prf_il{font-size:15px;line-height:18px;padding:1px 0;}#gsc_prf_iv{display:none;font-size:15px;line-height:18px;padding:1px 0;}.gs_el_ph #gsc_prf_iv{margin:16px 8px;clear:both;}.gsc_prf_why #gsc_prf_iv{display:block}#gsc_prf_iv li{margin:8px 0 0 0;}#gsc_prf_iv ul{margin:-8px 0 16px 16px;}#gsc_prf_iv button{margin-left:16px;}.gsc_prf_iev{padding:4px 0 8px 0;}.gsc_prf_iel{color:#777;}#gsc_prf_iv table{width:100%;}#gsc_prf_iv td{vertical-align:top;}.gsc_prf_el{color:#777;}.gsc_prf_ev{padding:4px 0 8px 0;}#gsc_prf_ep{padding:8px 0;}#gsc_prf_eb{padding:16px 0 32px 0;}#gsc_prf_eb button{margin-right:16px;}#gsc_fol_dd{top:29px;left:auto;right:0;text-align:left;}.gs_el_ph #gsc_fol_dd{min-width:240px;max-width:284px;right:-44px;}#gsc_fol_f,#gsc_fol_p{padding:16px}#gsc_fol_p{background:#ffc}#gsc_fol_ml{color:#777;padding-bottom:4px}#gsc_fol_cb{padding:8px 0}.gsc_fol_cr{margin:8px 0;white-space:normal}.gs_el_tc .gsc_fol_cr{margin:0}#gsc_fol_dd #gsc_fol_b{margin:0}#gsc_fol_dd #gsc_fol_x{margin:0 16px}#gsc_fol_p #gsc_fol_x{margin-right:0}#gsc_fol_ll{display:inline-block;padding:7px 0 8px 0;white-space:nowrap}#gsc_fol_hlp{margin-bottom:16px;white-space:normal;}.gs_el_ph #gsc_fol_dd #gsc_fol_b,.gs_el_ph #gsc_fol_dd #gsc_fol_x,.gs_el_ph #gsc_fol_ll{display:block;width:100%;margin:8px 0 0 0;text-align:center;}.gs_el_ph #gsc_fol_ll{background:#fcfcfc}#gsc_dd_exp-md,#gsc_dd_mor-md{top:29px;}.gs_el_ph #gsc_dd_exp-md{left:auto;right:12px;}#gsc_dd_mor-md{width:196px;white-space:normal;}.gs_el_ph #gsc_dd_mor-md{left:-84px;}#gsc_dd_mor-m,.gs_el_ph #gsc_dd_mor-s{display:block;}#gsc_dd_mor-s{display:none;border-top:1px solid #ebebeb;}#gsc_dd_mor-s .gsc_dd_mor-sel{color:#dd4b39;}#gsc_dd_mor-p{border-top:1px solid #ebebeb;padding:10px 44px 10px 16px;color:#777;}.gsc_art_sel #gsc_btn_add,.gsc_art_sel #gsc_dd_mor-w,.gsc_art_sel #gsc_a_nn,#gsc_btn_mer,#gsc_btn_del,#gsc_btn_expw{display:none;}#gsc_dd_mor-w,.gsc_art_sel #gsc_btn_mer,.gsc_art_sel #gsc_btn_del,.gsc_art_sel #gsc_btn_expw{position:relative;display:inline-block;}#gsc_a_t{width:100%;table-layout:fixed;}#gsc_a_tr0 th.gsc_a_x,#gsc_a_tr0 th.gsc_a_t,#gsc_a_tr0 th.gsc_a_c,#gsc_a_tr0 th.gsc_a_y{height:0;padding-top:0;padding-bottom:0;border:none;border-top:1px solid #ccc;}#gsc_a_trh{left:0;top:0;z-index:700;background:#f1f1f1;}th.gsc_a_x,th.gsc_a_t,th.gsc_a_c,th.gsc_a_y{box-sizing:border-box;background:#f1f1f1;}.gsc_a_x,.gsc_a_t,.gsc_a_c,.gsc_a_y,.gsc_a_e{font-weight:normal;padding:8px 16px;vertical-align:middle;text-align:right;border-bottom:1px solid #ccc;}.gs_el_sm .gsc_a_t,.gs_el_sm .gsc_a_c,.gs_el_sm .gsc_a_y{padding:8px 8px;}.gsc_a_x{padding:0;}th.gsc_a_x{width:41px;height:41px;text-align:left;}th.gsc_a_x .gs_ico{background-position:-134px 0;opacity:.55;display:block;margin-left:12px;}.gsc_a_t{text-align:left;}#gsc_a_ta,#gsc_a_nn{display:inline-block;vertical-align:middle;margin-right:16px;}.gs_el_ph #gsc_a_nn{display:none;}th.gsc_a_c{width:90px;}.gs_el_sm th.gsc_a_c{width:74px;}.gs_el_ph th.gsc_a_c{padding:0 8px;width:74px;word-wrap:break-word;}#gsc_a_ca{display:block;width:58px;}.gs_el_ph #gsc_a_ca{width:58px;}.gs_el_ph .gsc_art_sel #gsc_a_ca{display:none}td.gsc_a_c{padding:8px;}th.gsc_a_y{width:71px;}.gs_el_sm th.gsc_a_y{width:55px;}.gs_el_ph th.gsc_a_y,.gs_el_ph td.gsc_a_y{width:0;padding:0;}.gs_el_ph .gsc_a_h{display:none}@media print{#gs_top th.gsc_a_c{width:63pt;}#gs_top th.gsc_a_y{width:45pt;}}.gsc_a_e{padding:16px;text-align:center;}.gsc_a_a{padding:8px 0}.gsc_a_at{padding:8px 0;font-size:16px}.gsc_a_ac{padding:8px}a.gsc_a_acm{text-decoration:line-through}a.gsc_a_acm:hover,a.gs_a_acm:active{text-decoration:underline}.gsc_a_m{position:absolute}.gs_el_ph .gsc_a_m{display:block;position:static;}.gsc_a_am{font-size:24px;position:absolute;top:-12px;left:-8px;padding:8px 12px 4px 8px;}.gs_el_ph .gsc_a_am{display:inline-block;position:static;padding:6px 16px;margin-bottom:-6px;}#gsc_a_sp{margin-top:16px;height:25px;background:url('/intl/en/scholar/images/spinner.gif') no-repeat 50% 0%;padding-bottom:16px;border-bottom:1px solid #ccc;}#gsc_a_err{padding:28px 0;}.gsc_a_fix{position:fixed;}#gsc_lwp{margin:35px 0;text-align:center;}#gsc_bpf{display:inline-block;}#gsc_bpf_more{min-width:200px;margin:0 16px;}.gs_el_ph #gsc_bpf_more{margin:0;}#gsc_md_mopt,#gsc_md_cbyd,#gsc_md_cbym{width:600px;}.gs_el_ta #gsc_md_mopt,.gs_el_ta #gsc_md_cbyd,.gs_el_ta #gsc_md_cbym{width:500px;}.gs_el_ph #gsc_md_mopt,.gs_el_ph #gsc_md_cbyd,.gs_el_ph #gsc_md_cbym{width:80%;}#gsc_md_mopt .gs_md_prg,#gsc_md_cbyd .gs_md_prg,#gsc_md_cbym .gs_md_prg{margin:48px 0;}.gsc_mob_art{vertical-align:top;padding:8px 0;}.gsc_mob_cby{vertical-align:top;text-align:right;padding:8px 12px;}.gsc_mob_ttl,.gsc_mob_pub{display:block;}.gsc_mob_pub{color:#666;}.gsc_mob_cbym{text-decoration:line-through}.gsc_mob_cbm{font-size:24px;position:absolute;padding:4px 0 0 4px;line-height:16px;}@media print{#gs_top #gs_gb,#gs_top #gsc_nag,#gs_top #gsc_rsb_lg_m,#gs_top #gsc_rsb_lg,#gs_top #gsc_prf_e,#gs_top #gsc_prf_ib,#gs_top #gsc_prf_ip,#gs_top #gsc_prf_ivh,#gs_top #gsc_prf_puf,#gs_top #gsc_rsb_ssc,#gs_top #gsc_rsb_co,#gs_top #gsc_rsb_vc,#gs_top #gsc_g,#gs_top #gsc_btn_add,#gs_top #gsc_btn_mer, #gs_top #gsc_btn_del,#gs_top #gsc_btn_expw,#gs_top #gsc_dd_mor-w,#gs_top .gsc_a_x,#gs_top #gsc_lwp,#gs_top #gsc_ftr_h{display:none;}#gs_top,#gs_top #gsc_bdy,#gs_top #gsc_prf_w,#gs_top #gsc_prf,#gs_top #gsc_prf_pu,#gs_top #gsc_prf_i,#gs_top .gsc_rsb_s,#gs_top .gsc_lcl,#gs_top #gsc_rsb,#gs_top #gsc_a_t,#gs_top .gsc_prf_il,#gs_top #gsc_rsb_st{background:none;border:none;padding:0;margin:0;height:auto;width:auto;min-width:0;max-width:none;float:none;display:block;position:static;color:black;font-weight:normal;font-size:12pt;}#gs_top .gsc_a_ac,#gs_top .gsc_a_a,#gs_top #gsc_a_ca,#gs_top .gsc_a_at,#gs_top .gsc_rsb_sc1,#gs_top .gsc_rsb_sth,#gs_top .gsc_rsb_std,#gs_top #gsc_bdy .gsc_a_x,#gs_top #gsc_bdy .gsc_a_t,#gs_top #gsc_bdy .gsc_a_c,#gs_top #gsc_bdy .gsc_a_y,#gs_top #gsc_a_trh,#gs_top .gsc_a_m,#gs_top .gsc_a_am{color:black;font-weight:normal;font-size:12pt;padding:0;margin:0;background:none;border:none;}#gs_top .gsc_a_ac{font-size:10pt}#gs_top #gsc_prf_pu{float:left;width:80pt;text-align:center;margin:0 7pt 7pt 0;}#gs_top #gsc_prf_i{margin:0 7pt 7pt 0;}#gs_top #gsc_prf_in{font-size:20pt;line-height:20pt;padding:0 0 4pt 0;}#gs_top #gsc_prf_w{float:left;width:64%;}#gs_top #gsc_rsb{float:right;width:35%;}#gs_top #gsc_rsb_st{display:table;width:100%;}#gs_top #gsc_rsb_t{display:block;text-align:right;line-height:20pt;padding-bottom:5pt;font-weight:bold;}#gs_top .gsc_rsb_sc1,#gs_top .gsc_rsb_sth,#gs_top .gsc_rsb_std{font-size:10pt;}#gs_top th.gsc_rsb_sc1,#gs_top .gsc_rsb_sth{border-bottom:1pt solid #ccc;}#gs_top .gsc_rsb_sth{padding-left:14pt;}#gs_top #gsc_bdy .gsc_a_x,#gs_top #gsc_bdy .gsc_a_t,#gs_top #gsc_bdy .gsc_a_c,#gs_top #gsc_bdy .gsc_a_y,#gs_top #gsc_a_trh{padding:6pt 0;}#gs_top #gsc_a_trh{border-bottom:1pt solid #ccc;}#gs_top #gsc_a_ca{display:block;width:auto;}#gs_top #gsc_a_ta,#gs_top #gsc_a_nn{display:inline-block;vertical-align:middle;margin-right:12pt;}#gs_top .gsc_a_h{display:inline;font-size:10pt;}#gs_top .gsc_a_at,#gs_top .gsc_prf_ila{color:blue;}#gs_top .gsc_a_m,#gs_top .gsc_a_am{display:inline;position:absolute;}#gs_top .gsc_a_am{padding: 11pt 0 0 8pt;}#gs_top .gsc_a_fix{position:static}#gs_top .gsc_a_t .gs_gray{color:black;font-size:10pt;}#gs_top #gsc_dscl{margin:12pt 0 0 0;color:black;}}</style><script>var gs_ie_ver=100;</script><!--[if lte IE 8]><script>gs_ie_ver=8;</script><![endif]--><script>function gs_id(i){return document.getElementById(i)}function gs_ch(e,t){return e?e.getElementsByTagName(t):[]}function gs_ech(e){return e.children||e.childNodes}function gs_atr(e,a){return e.getAttribute(a)}function gs_hatr(e,a){var n=e.getAttributeNode(a);return n&&n.specified}function gs_xatr(e,a,v){e.setAttribute(a,v)}function gs_uatr(e,a){e.removeAttribute(a)}function gs_catr(e,a,v){gs_hatr(e,a)&&gs_xatr(e,a,v)}function gs_ctai(e,v){gs_hatr(e,\"tabindex\")&&(e.tabIndex=v)}function gs_uas(s){return (navigator.userAgent||\"\").indexOf(s)>=0}function gs_is_ph(){return document.documentElement.className.indexOf(\"gs_el_ph\")+1;}var gs_is_tc=/[?&]tc=([01])/.exec(window.location.search||\"\"),gs_is_ios=gs_uas(\"iPhone\")||gs_uas(\"iPod\")||gs_uas(\"iPad\");if(gs_is_tc){gs_is_tc=parseInt(gs_is_tc[1]);}else if(gs_uas(\"Android\")){gs_is_tc=1;}else if(window.matchMedia&&matchMedia(\"(pointer),(-moz-touch-enabled),(-moz-touch-enabled:0)\").matches){gs_is_tc=matchMedia(\"(pointer:coarse),(-moz-touch-enabled)\").matches;}else{gs_is_tc=0||('ontouchstart' in window)||(navigator.msMaxTouchPoints||0)>0;}var gs_re_sp=/\\s+/,gs_re_sel=/(?:^|\\s)gs_sel(?!\\S)/g,gs_re_par=/(?:^|\\s)gs_par(?!\\S)/g,gs_re_dis=/(?:^|\\s)gs_dis(?!\\S)/g,gs_re_vis=/(?:^|\\s)gs_vis(?!\\S)/g,gs_re_anm=/(?:^|\\s)gs_anm(?!\\S)/g,gs_re_bsp=/(?:^|\\s)gs_bsp(?!\\S)/g,gs_re_err=/(?:^|\\s)gs_err(?!\\S)/g,gs_re_nscl=/(?:^|\\s)gs_nscl(?!\\S)/g,gs_re_cb=/(?:^|\\s)gs_in_cb(?!\\S)/,gs_re_ra=/(?:^|\\s)gs_in_ra(?!\\S)/,gs_re_qsp=/[\\s\\u0000-\\u002f\\u003a-\\u0040\\u005b-\\u0060\\u007b-\\u00bf\\u2000-\\u206f\\u2e00-\\u2e42\\u3000-\\u303f\\uff00-\\uff0f\\uff1a-\\uff20\\uff3b-\\uff40\\uff5b-\\uff65]+/g;function gs_xcls(e,c){gs_scls(e,e.className+\" \"+c)}function gs_ucls(e,r){gs_scls(e,e.className.replace(r,\"\"))}function gs_scls(e,c){return e.className!=c&&(e.className=c,true)}function gs_usel(e){gs_ucls(e,gs_re_sel)}function gs_xsel(e){gs_usel(e);gs_xcls(e,\"gs_sel\")}function gs_tsel(e){return e.className.match(gs_re_sel)}function gs_isel(e){(gs_tsel(e)?gs_usel:gs_xsel)(e)}function gs_upar(e){gs_ucls(e,gs_re_par)}function gs_xpar(e){gs_upar(e);gs_xcls(e,\"gs_par\")}function gs_tpar(e){return e.className.match(gs_re_par)}function gs_udis(e){gs_ucls(e,gs_re_dis)}function gs_xdis(e){gs_udis(e);gs_xcls(e,\"gs_dis\")}function gs_tdis(e){return e.className.match(gs_re_dis)}function gs_btn_xdis(b,d){b&&((b.disabled=!!d)?gs_xdis:gs_udis)(b);}function gs_uvis(e){gs_ucls(e,gs_re_vis)}function gs_xvis(e){gs_uvis(e);gs_xcls(e,\"gs_vis\")}function gs_uanm(e){gs_ucls(e,gs_re_anm)}function gs_xanm(e){gs_uanm(e);gs_xcls(e,\"gs_anm\")}function gs_ubsp(e){gs_ucls(e,gs_re_bsp)}function gs_xbsp(e){gs_ubsp(e);gs_xcls(e,\"gs_bsp\")}function gs_uerr(e){gs_ucls(e,gs_re_err)}function gs_xerr(e){gs_uerr(e);gs_xcls(e,\"gs_err\")}function gs_unscl(e){gs_ucls(e,gs_re_nscl)}function gs_xnscl(e){gs_unscl(e);gs_xcls(e,\"gs_nscl\")}var gs_gcs=window.getComputedStyle?function(e){return getComputedStyle(e,null)}:function(e){return e.currentStyle};var gs_ctd=function(){var s=document.documentElement.style,p,l=['OT','MozT','webkitT','t'],i=l.length;function f(s){return Math.max.apply(null,(s||\"\").split(\",\").map(parseFloat))||0;}do{p=l[--i]+'ransition'}while(i&&!(p in s));return i?function(e){var s=gs_gcs(e);return f(s[p+\"Delay\"])+f(s[p+\"Duration\"]);}:function(){return 0};}();var gs_tmh=function(){var X,P={};return {a:function(e){var t=pageYOffset+e.getBoundingClientRect().bottom;X=X||gs_id(\"gs_top\");if(e.id){P[e.id]=1;t>X.offsetHeight&&(X.style.minHeight=t+\"px\");}},r:function(e){if(e.id&&X){delete P[e.id];if(!Object.keys(P).length){X.style.minHeight=\"\";}}}}}();var gs_vis=function(){return function(e,v,c){var s=e&&e.style,h,f;if(s){gs_catr(e,\"aria-hidden\",v?\"false\":\"true\");if(v){s.display=v===2?\"inline\":\"block\";gs_ctd(e);gs_xvis(e);f=gs_ctd(e);gs_uas(\"AppleWebKit\")&&f&&gs_evt_one(e,\"transitionend webkitTransitionEnd\",function(){gs_tmh.a(e);});c&&(f?setTimeout(c,1000*f):c());}else{gs_uvis(e);h=function(){s.display=\"none\";gs_tmh.r(e);c&&c();};f=gs_ctd(e);f?setTimeout(h,1000*f):h();}}};}();function gs_visi(i,v,c){gs_vis(gs_id(i),v,c)}function gs_sel_clk(p,t){var l=gs_ch(gs_id(p),\"li\"),i=l.length,c,x,s;while(i--){if((c=gs_ch(x=l[i],\"a\")).length){s=c[0]===t;(s?gs_xsel:gs_usel)(x);gs_catr(c[0],\"aria-selected\",s?\"true\":\"false\");}}return false;}function gs_efl(f){if(typeof f==\"string\"){var c=f.charAt(0),x=f.slice(1);if(c===\"#\")f=function(t){return t.id===x};else if(c===\".\")f=function(t){return (\" \"+t.className+\" \").indexOf(\" \"+x+\" \")>=0};else{c=f.toLowerCase();f=function(t){return t.nodeName.toLowerCase()===c};}}return f;}function gs_dfcn(d){return (d?\"last\":\"first\")+\"Child\"}function gs_dnsn(d){return (d?\"previous\":\"next\")+\"Sibling\"}var gs_trv=function(){function h(r,x,f,s,n,c){var t,p;while(x){if(x.nodeType===1){if(f(x)){if(c)return x;}else{for(p=x[s];p;p=p[n])if(t=h(p,p,f,s,n,1))return t;}}c=1;while(1){if(x===r)return;p=x.parentNode;if(x=x[n])break;x=p;}}}return function(r,x,f,d){return h(r,x,gs_efl(f),gs_dfcn(d),gs_dnsn(d))};}();function gs_bind(){var a=Array.prototype.slice.call(arguments),f=a.shift();return function(){return f.apply(null,a.concat(Array.prototype.slice.call(arguments)))};}function gs_evt1(e,n,f){e.addEventListener(n,f,false)}function gs_uevt1(e,n,f){e.removeEventListener(n,f,false)}if(!window.addEventListener){gs_evt1=function(e,n,f){e.attachEvent(\"on\"+n,f)};gs_uevt1=function(e,n,f){e.detachEvent(\"on\"+n,f)};}function gs_evtX(e,n,f,w){var i,a;typeof n===\"string\"&&(n=n.split(\" \"));for(i=n.length;i--;)(a=n[i])&&w(e,a,f);}function gs_evt(e,n,f){gs_evtX(e,n,f,gs_evt1)}function gs_uevt(e,n,f){gs_evtX(e,n,f,gs_uevt1)}function gs_evt_one(e,n,f){function g(E){gs_uevt(e,n,g);f(E);}gs_evt(e,n,g);}function gs_evt_all(l,n,f){for(var i=l.length;i--;){gs_evt(l[i],n,gs_bind(f,l[i]))}}function gs_evt_dlg(p,c,n,f){p!==c&&(c=gs_efl(c));gs_evt(p,n,p===c?function(e){f(p,e)}:function(e){var t=gs_evt_tgt(e);while(t){if(c(t))return f(t,e);if(t===p)return;t=t.parentNode;}});}function gs_evt_sms(v){var L=[],l=[\"mousedown\",\"click\"],i=l.length;function s(e){for(var l=L,n=l.length,i=0,x=e.clientX,y=e.clientY;i<n;i+=2){if(Math.abs(x-l[i])<10&&Math.abs(y-l[i+1])<10){gs_evt_ntr(e);break;}}}while(i--)document.addEventListener(l[i],s,true);gs_evt_sms=function(e){var l=e.changedTouches||[],h=l[0]||{};L.push(h.clientX,h.clientY);setTimeout(function(){L.splice(0,2)},2000);};gs_evt_sms(v);v=0;}function gs_evt_clk(e,f,w,k,d){return gs_evt_dlg_clk(e,e,function(t,e){f(e)},w,k,d);}function gs_evt_dlg_clk(p,c,f,w,k,d){k=\",\"+(k||[13,32]).join(\",\")+\",\";return gs_evt_dlg_xclk(p,c,function(t,e){if(e.type==\"keydown\"){if(k.indexOf(\",\"+e.keyCode+\",\")<0)return;gs_evt_ntr(e);}f(t,e);},w,d);}function gs_evt_xclk(e,f,w){return gs_evt_dlg_xclk(e,e,function(t,e){f(e)},w);}function gs_evt_dlg_xclk(p,c,f,w,d){var T,S=0,D=0;function u(t,e){var n=e.type;if(!S&&d&&d(e))return;if(t!==T){T=t;S=0;}if(!gs_evt_spk(e)){if(n===\"mousedown\"){S=1;D=d&&d(e);}else if(n===\"click\"){if(S){D||gs_evt_ntr(e);return S=0;}}else if(n===\"touchstart\"){S=0;gs_evt_sms(e);}else if(n===\"keydown\"){f(t,e);return;}else if(n===\"keyup\"){e.keyCode===32&&gs_evt_pdf(e);return;}else{return}gs_evt_ntr(e);f(t,e);}}gs_evt_dlg(p,c,[\"keydown\",\"keyup\",\"click\"].concat(w?[\"mousedown\",\"touchstart\"]:[]),u);return u;}function gs_evt_inp(e,f){gs_evt(e,[\"input\",\"keyup\",\"cut\",\"paste\",\"change\",\"gs-change\"],function(){setTimeout(f,0)});}function gs_evt_fcs(e,f){e.addEventListener(\"focus\",f,true)}function gs_evt_blr(e,f){e.addEventListener(\"blur\",f,true)}if(\"onfocusin\" in document){gs_evt_fcs=function(e,f){gs_evt(e,\"focusin\",f)};gs_evt_blr=function(e,f){gs_evt(e,\"focusout\",f)};}function gs_evt_stp(e){e.cancelBubble=true;e.stopPropagation&&e.stopPropagation();return false;}function gs_evt_pdf(e){e.returnValue=false;e.preventDefault&&e.preventDefault();}function gs_evt_ntr(e){gs_evt_stp(e);gs_evt_pdf(e);}function gs_evt_tgt(e){var t=e.target||e.srcElement;t&&t.nodeType===3&&(t=t.parentNode);return t;}function gs_evt_spk(e){return (e.ctrlKey?1:0)|(e.altKey?2:0)|(e.metaKey?4:0)|(e.shiftKey?8:0);}function gs_evt_crt(d,t){if(document.createEvent){var e=document.createEvent('Event');e.initEvent(t,!0,!0);d.dispatchEvent(e);}}function gs_tfcs(t){if(!gs_is_tc||(gs_uas(\"Windows\")&&!gs_uas(\"Windows Phone\"))){t.focus();t.value=t.value;}}var gs_raf=window.requestAnimationFrame||window.webkitRequestAnimationFrame||window.mozRequestAnimationFrame||function(c){setTimeout(c,33)};var gs_evt_rdy=function(){var d=document,l=[],h=function(){var n=l.length,i=0;while(i<n)l[i++]();l=[];};gs_evt(d,\"DOMContentLoaded\",h);gs_evt(d,\"readystatechange\",function(){var s=d.readyState;(s==\"complete\"||(s==\"interactive\"&&gs_id(\"gs_rdy\")))&&h();});gs_evt(window,\"load\",h);return function(f){l.push(f)};}();function gs_evt_raf(e,n){var l=[],t=0,h=function(){var x=l,n=x.length,i=0;while(i<n)x[i++]();t=0;};return function(f){l.length||gs_evt(e,n,function(){!t++&&gs_raf(h)});l.push(f);};}var gs_evt_wsc=gs_evt_raf(window,\"scroll\"),gs_evt_wre=gs_evt_raf(window,\"resize\");var gs_md_st=[],gs_md_lv={},gs_md_fc={},gs_md_if,gs_md_is=0;function gs_md_ifc(d,f){gs_md_fc[d]=f}function gs_md_sif(){gs_md_if=1;setTimeout(function(){gs_md_if=0},0);}function gs_md_plv(n){var l=gs_md_lv,x=0;while(n&&!x){x=l[n.id]||0;n=n.parentNode;}return x;}gs_evt(document,\"click\",function(e){var m=gs_md_st.length;if(m&&!gs_evt_spk(e)&&m>gs_md_plv(gs_evt_tgt(e))){(gs_md_st.pop())();gs_evt_pdf(e);}});gs_evt(document,\"keydown\",function(e){e.keyCode==27&&!gs_evt_spk(e)&&gs_md_st.length&&(gs_md_st.pop())();});gs_evt(document,\"selectstart\",function(e){gs_md_is&&gs_evt_pdf(e)});gs_evt_fcs(document,function(e){var l=gs_md_lv,m=gs_md_st.length,x,k,v,d;if(m&&!gs_md_if){x=gs_md_plv(gs_evt_tgt(e));while(x<m){v=0;for(k in l)l.hasOwnProperty(k)&&l[k]>v&&(v=l[d=k]);if(v=gs_md_fc[d]){gs_evt_stp(e);gs_id(v).focus();break;}else{(gs_md_st.pop())(1);--m;!gs_md_is++&&setTimeout(function(){gs_md_is=0},1000);}}}});function gs_md_cls(d,e){var x=gs_md_lv[d]||1e6;while(gs_md_st.length>=x)(gs_md_st.pop())();return e&&gs_evt_stp(e);}function gs_md_shw(d,e,o,c){if(!gs_md_lv[d]){var x=gs_md_plv(gs_id(d));while(gs_md_st.length>x)(gs_md_st.pop())();o&&o();gs_md_st.push(function(u){gs_md_lv[d]=0;c&&c(u);});gs_md_lv[d]=gs_md_st.length;return gs_evt_stp(e);}}function gs_md_opn(d,e,c,z){var a=document.activeElement;return gs_md_shw(d,e,gs_bind(gs_visi,d,1),function(u){gs_visi(d,0,z);try{u||a.focus()}catch(_){}c&&c(u);});}function gs_evt_md_mnu(d,b,f,a,w){var O,X;d=gs_id(d);b=gs_id(b);f=f?gs_efl(f):function(t){return (gs_hatr(t,\"data-a\")||t.nodeName===\"A\"&&t.href)&&t.offsetWidth;};a=a||function(t){var c=gs_atr(t,\"data-a\");c?eval(c):t.nodeName===\"A\"&&t.href&&(location=t.href);};function u(e){if(e.type==\"keydown\"){var k=e.keyCode;if(k==38||k==40){if(O){try{gs_trv(d,d,f,k==38).focus()}catch(_){}gs_evt_ntr(e);return;}}else if(k!=13&&k!=32){return;}gs_evt_pdf(e);}if(O){gs_md_cls(d.id,e);}else{gs_md_sif();O=1;gs_xsel(b);gs_md_opn(d.id,e,function(){O=0;gs_usel(b);try{X.blur()}catch(_){}});w&&w();}}function c(x,r){var p=x.parentNode,c=gs_ech(p),i=c.length,l=\"offsetLeft\";if(p!==d){while(c[--i]!==x);p=p[gs_dnsn(r)]||p.parentNode[gs_dfcn(r)];c=gs_ech(p);if(i=Math.min(i+1,c.length)){p=c[i-1];if(p.nodeType==1&&f(p)&&p[l]!=x[l])return p;}}}function g(t,e){function m(x){if(x){gs_evt_ntr(e);x.focus();}}if(O){if(e.type==\"keydown\"){var k=e.keyCode;if(k==13||k==32){}else{if(k==38||k==40){m(gs_trv(d,t,f,k==38)||gs_trv(d,d,f,k==38));}else if(k==37||k==39){m(c(t,k==37));}return;}}gs_hatr(t,\"data-md-no-close\")||gs_md_cls(d.id,e);gs_evt_pdf(e);gs_md_sif();a(t);}}gs_evt_xclk(b,u,2);gs_evt_fcs(d,function(e){var x=gs_evt_tgt(e);if(x&&f(x)){gs_ctai(x,0);X=x;}});gs_evt_blr(d,function(e){var x=gs_evt_tgt(e);if(x&&f(x)){gs_ctai(x,-1);X=0;}});gs_evt_dlg_xclk(d,f,g);return u;}function gs_evt_md_sel(d,b,h,c,s,u){h=gs_id(h);c=gs_id(c);s=gs_id(s);return gs_evt_md_mnu(d,b,function(t){return gs_hatr(t,\"data-v\")},function(t){h.innerHTML=t.innerHTML;c.value=gs_atr(t,\"data-v\");if(t!==s){gs_usel(s);gs_uatr(s,\"aria-selected\");gs_xsel(s=t);gs_xatr(s,\"aria-selected\",\"true\");}gs_evt_crt(c,\"gs-change\");u&&u();},function(){s.focus()});}function gs_xhr(){if(window.XMLHttpRequest)return new XMLHttpRequest();var c=[\"Microsoft.XMLHTTP\",\"MSXML2.XMLHTTP\",\"MSXML2.XMLHTTP.3.0\",\"MSXML2.XMLHTTP.6.0\"],i=c.length;while(i--)try{return new ActiveXObject(c[i])}catch(e){}}function gs_ajax(u,d,c){var r=gs_xhr();r.onreadystatechange=function(){r.readyState==4&&c(r.status,r.responseText);};r.open(d?\"POST\":\"GET\",u,true);d&&r.setRequestHeader(\"Content-Type\",\"application/x-www-form-urlencoded\");d?r.send(d):r.send();}var gs_json_parse=\"JSON\" in window?function(s){return JSON.parse(s)}:function(s){return eval(\"(\"+s+\")\")};function gs_frm_ser(e,f){var i=e.length,r=[],x,n,t;while(i--){x=e[i];n=encodeURIComponent(x.name||\"\");t=x.type;n&&(!f||f(x))&&!x.disabled&&((t!=\"checkbox\"&&t!=\"radio\")||x.checked)&&r.push(n+\"=\"+encodeURIComponent(x.value||\"\"));}return r.join(\"&\");}function gs_btn_ssp(b,v){  var x=gs_id(b);  (v?gs_xbsp:gs_ubsp)(x);  x.disabled=!!v;}function gs_evt_frm_ajax(f,b,a){var Z=f.elements,H={},x,y,i=Z.length;while(i--){x=Z[i];y=x.nextSibling;if(y&&x.name&&x.type==\"text\")H[x.name]=y.innerHTML;}function s(e){var p=\"json=&\"+gs_frm_ser(Z);b&&b();gs_ajax(f.action,p,function(c,t){if(c!=200){a&&a(c);return}var p=gs_json_parse(t),l=p[\"L\"],g,m;if(l){a&&a(c,p);location=l;return}g=p[\"E\"];for(i in H){x=Z[i];m=g[i];y=x.nextSibling;gs_scls(x,\"gs_in_txt\"+(m?\" gs_in_txte\":\"\"));gs_scls(y,\"gs_in_txts \"+(m?\"gs_red\":\"gs_gray\"));y.innerHTML=m||H[i];gs_vis(y,y.innerHTML?1:0);}a&&a(c,p);});e&&gs_evt_pdf(e);};gs_evt(f,\"submit\",s);return s;}var gs_rlst,gs_wlst;!function(U){var L={},S;try{S=window.localStorage}catch(_){}gs_rlst=function(k,s){if(s||!(k in L)){var v=S&&S[k];if(v)try{v=JSON.parse(v)}catch(_){v=U}else v=U;L[k]=v;}return L[k];};gs_wlst=function(k,v){L[k]=v;try{S&&(S[k]=JSON.stringify(v))}catch(_){}};}();function gs_ac_nrm(q,o,t){o=o||[];o.length=0;q=(q||\"\").toLowerCase();var L=q.length,M;q=q.replace(gs_re_qsp,function(m,p){o.push(M=m.length);return !t||p+M<L?\" \":\"\";});q[0]==\" \"?(q=q.substr(1)):o.unshift(0);return q;}function gs_ac_get(Q){var h=gs_rlst(\"H:\"+Q),t={\"\":1},i=0,j=0,n,v,q;(h instanceof Array)||(h=[]);h=h.slice();for(n=h.length;i<n;i++){v=h[i]=((v=h[i]) instanceof Array)&&v.length==3?v.slice():[0,0,\"\"];v[0]=+v[0]||0;v[1]=+v[1]||0;v[2]=\"\"+v[2];q=v[3]=gs_ac_nrm(\"\"+v[2],v[4]=[],1);t[q]||(t[q]=1,h[j++]=v);}h.splice(Math.min(j,50),n);return h;}function gs_ac_set(Q,h){var r=[],i=0,n=h.length;while(i<n)r.push(h[i++].slice(0,3));gs_wlst(\"H:\"+Q,r);}function gs_ac_fre(t){return Math.exp(.0231*((Math.max(t-1422777600,0)/86400)|0));}function gs_ac_add(Q,q,d){var h=gs_ac_get(Q),n=h.length,t=1e-3*(new Date()),m=0,x,w,o=[];if(w=gs_ac_nrm(q,o,1)){d=d||t;while(m<n&&h[m][3]!=w)++m;m<n||h.push([0,0,q,w,o]);if(d-h[m][0]>1){h[m][0]=d;h[m][2]=q;h[m][4]=o;h[m][1]=Math.min(h[m][1]+gs_ac_fre(d),10*gs_ac_fre(t));while(m&&h[m][1]>h[m-1][1]){x=h[m];h[m]=h[m-1];h[--m]=x;}h.splice(50,h.length);gs_ac_set(Q,h);}}}var gs_evt_el=function(W,D,L){function p(){var r=D.documentElement,w=W.innerWidth,h=W.innerHeight,S=W.screen,a=S.width*S.height,m=\"\",n,i;if(w<600||a<480000)m=\"gs_el_sm gs_el_ph\";else if(w<982)m=\"gs_el_sm gs_el_ta\";else if(w<1060||h<590)m=\"gs_el_sm\";else if(w<1252||h<640)m=\"gs_el_me\";(gs_is_tc||m==\"gs_el_sm gs_el_ph\")&&(m+=\" gs_el_tc\");gs_is_ios&&(m+=\" gs_el_ios\");if(gs_scls(r,m))for(n=L.length,i=0;i<n;)L[i++]();}p();gs_evt_wre(p);gs_evt(W,[\"pageshow\",\"load\"],p);return function(f){f();L.push(f)};}(window,document,[]);!function(B,U){gs_evt(document,(B&&\"1\"?[]:[\"mousedown\",\"touchstart\"]).concat([\"contextmenu\",\"click\"]),function(e){var t=gs_evt_tgt(e),a=\"data-clk\",w=window,r=document.documentElement,p=\"http://scholar.google.com\"||\"http://\"+location.host,n,h,c,u;while(t){n=t.nodeName;if(n===\"A\"&&(h=gs_ie_ver<=8?t.getAttribute(\"href\",2):gs_atr(t,\"href\"))&&(c=gs_atr(t,a))){u=\"/scholar_url?url=\"+encodeURIComponent(h)+\"&\"+c+\"&ws=\"+(w.innerWidth||r.offsetWidth||0)+\"x\"+(w.innerHeight||r.offsetHeight||0);if(c.indexOf(\"&scisig=\")>0){gs_xatr(t,\"href\",p+u);gs_uatr(t,a);}else if(B){B.call(navigator,u);}else if(u!=U.src){(U=new Image()).src=u;setTimeout(function(){U={};},1000);}break;}t=(n===\"SPAN\"||n===\"B\"||n===\"I\"||n===\"EM\")&&t.parentNode;}});}(navigator.sendBeacon,{});function gs_is_cb(e){var n=e.className||\"\";return n.match(gs_re_cb)||n.match(gs_re_ra);}function gs_ssel(e){}(function(d){function c(){var v=l,i=v.length,k=p,e,x=gs_id(\"gs_top\");if(x&&!r){gs_evt(x,\"click\",function(){});r=1;if(!s){clearInterval(t);t=null}}p=i;while(i-->k)gs_is_cb((e=v[i]).parentNode)&&gs_ssel(e);}var s=gs_ie_ver<=8,l=[],p=0,t=setInterval(c,200),r;gs_evt_rdy(function(){c();l=[];clearInterval(t)});if(!s&&gs_is_tc){s=/AppleWebKit\\/([0-9]+)/.exec(navigator.userAgent||\"\");s=s&&parseInt(s[1])<535;}if(!s)return;l=gs_ch(d,\"input\");gs_ssel=function(e){var p=e.parentNode,l,i,r;(e.checked?gs_xsel:gs_usel)(p);if(p.className.match(gs_re_ra)){l=e.form.elements[e.name]||[];for(i=l.length;i--;)((r=l[i]).checked?gs_xsel:gs_usel)(r.parentNode);}};gs_evt(d,\"click\",function(e){var x=gs_evt_tgt(e),p=x.parentNode;gs_is_cb(p)&&x.nodeName===\"INPUT\"&&gs_ssel(x);});})(document);function gs_cb_set(d,s,i){(s==1?gs_xsel:gs_usel)(d);(s==2?gs_xpar:gs_upar)(d);gs_xatr(d,\"aria-checked\",[\"false\",\"true\",\"mixed\"][s]);i||gs_xatr(d,\"data-s\",\"\"+s);}function gs_cb_get(e){return !!gs_tsel(e)+2*!!gs_tpar(e)}function gs_cb_ch(e){return +gs_atr(e,\"data-s\")!=gs_cb_get(e);}function gs_cb_nxt(t){var i=+gs_atr(t,\"data-s\");i!=2?gs_isel(t):gs_cb_set(t,[2,0,1][gs_cb_get(t)],true);gs_evt_crt(t,'gs-change');}gs_evt_dlg(document,function(t){return (\" \"+t.className+\" \").indexOf(\" gs_cb_gen \")>=0;},\"click\",gs_cb_nxt);</script><script>function gsc_scroll_right(x){x.scrollLeft=x.scrollWidth;}var gsc_art_sel_l=[];function gsc_evt_art_sel(f){gsc_art_sel_l.push(f)}var gsc_art_sel_p=[];function gsc_art_sel_chg(x){var l=gsc_art_sel_l,n=l.length,i=0;while(i<n)l[i++](x);}function gsc_art_cbs(){return gs_ch(gs_id(\"gsc_a_t\"),\"input\")}function gsc_art_sel_cbs(l){l=l||gsc_art_cbs();var i=l.length,c,s=[];while(i--)(c=l[i]).checked&&s.push(c);return s;}function gsc_art_sids(l){l=l||gsc_art_cbs();var i=l.length,c,s=[];while(i--)(c=l[i]).checked&&s.push(c.value);return s;}function gsc_art_sel(v,l){l=l||gsc_art_cbs();var i=l.length,x;while(i--){(x=l[i]).checked=!!v;gs_ssel(x)}gsc_art_sel_chg(true);}gs_evt_rdy(function(){gsc_art_sel_chg();gs_evt_dlg(gs_id(\"gsc_a_t\"),\"input\",\"change\",function(){gsc_art_sel_chg();});var x=gs_id(\"gsc_x_all\");x&&gs_evt(x,\"gs-change\",function(){var s=gs_cb_get(x);gsc_art_sel(s!=0,s==2&&gsc_art_sel_p);});});function gsc_btn_sdis(b,d){b.disabled=!!d;(d?gs_xdis:gs_udis)(b);}function gsc_tr_add(b,h){try{b.innerHTML+=h}catch(_){var d=document.createElement(\"div\"),r,c,i,n;d.innerHTML=\"<table>\"+h+\"</table>\";r=gs_ch(d,\"tr\");for(i=0,n=r.length,c=[];i<n;i++)c.push(r[i]);for(i=0;i<n;i++)b.appendChild(c[i]);}}function gsc_rsb_mco(x,w){var f=gs_id(\"gsc_rsb_mco\"),e=f.elements;e.colleague.value=x;e.add_colleague_btn.disabled=!!w;e.del_sugg_coll_btn.disabled=!w;f.submit();return false;}function gsc_prf_ed(w){gs_scls(gs_id(\"gsc_bdy\"),w?\"gsc_prf_ed\":\"\");}gsc_evt_art_sel(function(x){var v,n,k=gsc_art_cbs(),m=gs_id(\"gsc_btn_mer\"),o=gs_id(\"gsc_x_all\");if(m){p=gsc_art_sel_cbs(k);n=p.length;gsc_btn_sdis(m,n<2||n>5);if(!x&&o){gs_cb_set(o,v=n&&2-(n==k.length));v==2&&(gsc_art_sel_p=p);}gs_scls(gs_id(\"gsc_a_hd\"),n?\"gsc_art_sel\":\"\");}});function gsc_art_export(f){var l=gsc_art_cbs(),s=gsc_art_sids(l);gsc_md_show_exa(s,s.length==l.length,f,{});}gs_evt_rdy(function(){var m=gs_id(\"gsc_btn_mer\");m&&gs_evt(m,\"click\",function(e){gsc_md_show_mopt(gsc_art_sids(),e);});var p=gs_id(\"gsc_prf_puf\"),u;if(p){u=p.elements.upload_file;gs_evt(gs_id(\"gsc_prf_pufb\"),\"click\",function(e){if(gs_uas(\"MSIE \")||gs_uas(\"Trident\")){gs_scls(p,\"gsc_prf_pufo\");gs_md_opn(\"gsc_prf_pufi\",e,function(){gs_scls(p,\"\")});u.focus();}else{u.click();}});gs_evt(u,\"change\",function(){p.submit()});}});gs_evt_rdy(function(){var h=window.location.href,s=parseInt(h.replace(/.*[?&]cstart=([0-9]*).*/,\"$1\"))||0,n=parseInt(h.replace(/.*[?&]pagesize=([0-9]*).*/,\"$1\"))||20,P=gs_id(\"gsc_bpf_prev\"),N=gs_id(\"gsc_bpf_next\"),M=gs_id(\"gsc_bpf_more\");function t(){return Math.max(Math.min(n,100),20)}n=t();function p(k,m){return (h.replace(/([?&])(cstart|pagesize)=[^&]*/g,\"$1\")+\"&cstart=\"+k+\"&pagesize=\"+m).replace(/([?&])&+/,\"$1\");}gs_evt(P,\"click\",function(){location=p(s-t(),t())});gs_evt(N,\"click\",function(){location=p(s+n,t())});gs_evt(M,\"click\",function(){var m=n<100?100-n:100,x=gs_id(\"gsc_a_sp\"),g=gs_id(\"gsc_a_err\");gs_vis(g,0);gs_vis(x,1);gs_ajax(p(s+n,m),\"json=1\",function(c,t){var r=c==200&&gs_json_parse(t),b=gs_id(\"gsc_a_b\");gs_vis(x,0);if(r){gsc_tr_add(b,r[\"B\"]);n+=m;(gs_id(\"gsc_a_nn\")||{}).innerHTML=(s+1)+\"&ndash;\"+(s+gs_ch(b,\"tr\").length);gsc_btn_sdis(P,!r[\"P\"]||s<=0);gsc_btn_sdis(N,!r[\"N\"]);gsc_btn_sdis(M,!r[\"N\"]||n>=1000);gsc_art_sel_chg();}else{gs_vis(g,1);}});});});gs_evt_rdy(function(){var f=gs_id(\"gsc_rsb_f_m\"),i=gs_id(\"gsc_rsb_fin_m\"),b=gs_id(\"gsc_rsb_fbt_m\");gs_evt_blr(f,function(){setTimeout(function(){var a=document.activeElement;a!==i&&a!==b&&gs_scls(f,\"gsc_rsb_foff\");},0);});gs_evt(f,\"submit\",function(e){  if(f.className){gs_scls(f,\"\");setTimeout(function(){i.focus();},1000*gs_ctd(i));      gs_evt_pdf(e);  }else if(!i.value){      gs_scls(f,\"gsc_rsb_foff\");gs_evt_pdf(e);  }});});</script><title>Jeff Leek - Google Scholar Citations</title><link rel=\"canonical\" href=\"http://scholar.google.com/citations?user=HI-I6C0AAAAJ&amp;hl=en\"></head><body><div id=\"gs_top\"><style>#gs_gb{position:relative;height:30px;background:#2d2d2d;font-size:13px;line-height:16px;-webkit-backface-visibility:hidden;}#gs_gb_lt,#gs_gb_rt,#gs_gb_lp{position:absolute;top:0;white-space:nowrap;}#gs_gb_lt{left:22px}.gs_el_sm #gs_gb_lt{left:6px}.gs_el_ph #gs_gb_lt{display:none}#gs_gb_lp{display:none}#gs_gb_lp:hover,#gs_gb_lp:focus,#gs_gb_lp:active{-webkit-filter:brightness(100%);}.gs_el_ph #gs_gb_lp{display:block;z-index:1;cursor:pointer;top:8px;left:8px;width:48px;height:16px;background:no-repeat url('/intl/en/scholar/images/1x/googlelogo_bbb_color_48x16dp.png');background-size:48px 16px;}@media(-webkit-min-device-pixel-ratio:1.5),(min-resolution:144dpi){.gs_el_ph #gs_gb_lp{background-image:url('/intl/en/scholar/images/2x/googlelogo_bbb_color_48x16dp.png');}}#gs_gb_rt{right:22px}.gs_el_sm #gs_gb_rt{right:6px}.gs_el_ta #gs_gb_rt,.gs_el_ph #gs_gb_rt{right:0}#gs_gb_lt a:link,#gs_gb_lt a:visited,#gs_gb_rt a:link,#gs_gb_rt a:visited{display:inline-block;vertical-align:top;height:29px;line-height:27px;padding:2px 10px 0 10px;font-weight:bold;color:#bbb;cursor:pointer;text-decoration:none;}#gs_gb_rt a:link,#gs_gb_rt a:visited{padding:2px 8px 0 8px}#gs_gb_lt a:hover,#gs_gb_lt a:focus,#gs_gb_lt a:active,#gs_gb_rt a:hover,#gs_gb_rt a:focus,#gs_gb_rt a:active{color:white;outline:none;}#gs_gb_ac{top:30px;left:auto;right:6px;width:288px;white-space:normal;}#gs_gb_aw,#gs_gb_ap,.gs_gb_am,#gs_gb_ab{display:block;padding:10px 20px;word-wrap:break-word;}#gs_gb_aw{background:#fef9db;font-size:11px;}#gs_gb_ap,.gs_gb_am{border-bottom:1px solid #ccc;}#gs_gb_aa:link,#gs_gb_aa:visited{float:right;margin-left:8px;color:#1a0dab;}#gs_gb_aa:active{color:#d14836}.gs_gb_am:link,.gs_gb_am:visited{color:#222;text-decoration:none;background:#fbfbfb;}.gs_gb_am:hover,.gs_gb_am:focus{background:#f1f1f1}.gs_gb_am:active{background:#eee}#gs_gb_ab{background:#fbfbfb;overflow:auto;}#gs_gb_aab{float:left}#gs_gb_aso{float:right}</style><div id=\"gs_gb\" role=\"navigation\"><div id=\"gs_gb_lt\"><a href=\"//www.google.com/webhp?hl=en&amp;\">Web</a><a href=\"//www.google.com/imghp?hl=en&amp;\">Images</a><a href=\"//www.google.com/intl/en/options/\">More&hellip;</a></div><a id=\"gs_gb_lp\" aria-label=\"Web\" href=\"//www.google.com/webhp?hl=en&\"></a><div id=\"gs_gb_rt\"><a href=\"https://accounts.google.com/Login?hl=en&amp;continue=http://scholar.google.com/citations%3Fuser%3DHI-I6C0AAAAJ%26hl%3Den\">Sign in</a></div></div><!--[if lte IE 9]><div class=\"gs_alrt\" style=\"padding:16px\"><div>Sorry, some features may not work in this version of Internet Explorer.</div><div>Please use <a href=\"//www.google.com/chrome/\">Google Chrome</a> or <a href=\"//www.mozilla.com/firefox/\">Mozilla Firefox</a> for the best experience.</div></div><![endif]--><style>html,body{height:100%}#gs_top{min-height:100%}#gs_md_s,#gs_md_w{z-index:1200;position:absolute;top:0;left:0;width:100%;height:100%;}#gs_md_s{background:#666;filter:alpha(opacity=50);-ms-filter:\"alpha(opacity=50)\";opacity:.5;}.gs_md_d{position:relative;padding:28px 32px;margin:0 auto;width:400px;-moz-box-shadow:2px 2px 8px rgba(0,0,0,.65);-webkit-box-shadow:2px 2px 8px rgba(0,0,0,.65);box-shadow:2px 2px 8px rgba(0,0,0,.65);}.gs_el_ph .gs_md_d{padding:16px 20px;width:80%;max-width:400px;}.gs_md_d .gs_ico_X{position:absolute;top:8px;right:8px;background-color:#fff;}.gs_md_d h2{font-size:16px;font-weight:normal;line-height:24px;margin-bottom:16px;}.gs_el_ph .gs_md_d h2{margin-bottom:8px}.gs_md_lbl{margin:16px 0}.gs_md_btns{margin-top:16px}.gs_md_btns button{margin-right:16px}.gs_md_prg{margin:24px 0;}</style><script>function gs_md_opw(d,e,b){var r=document.documentElement,s=gs_id(\"gs_md_s\").style,w=gs_id(\"gs_md_w\").style,q=gs_id(d),g=q.style;g.visibility=\"hidden\";s.display=w.display=g.display=\"block\";g.top=Math.max(document.body.scrollTop||0,r.scrollTop||0)+Math.max((r.clientHeight-q.offsetHeight)/2,10)+\"px\";g.visibility=\"visible\";gs_md_opn(d,e,function(){s.display=\"none\"},function(){w.display=\"none\"});if(b){b=gs_id(b);b.type===\"text\"?gs_tfcs(b):b.focus();}return false;}function gs_md_ldw(d,e,b,c,u,p,f){c=gs_id(c);c.innerHTML=\"<div class='gs_md_prg'>Loading...</div>\";gs_md_opw(d,e,b);gs_ajax(u,p,function(x,t){c.innerHTML=x===200?t:\"<div class='gs_md_prg gs_alrt'>The system can\\x27t perform the operation now. Try again later.</div>\";f&&f(x,t);});}</script><div id=\"gs_md_s\" style=\"display:none\"></div><div id=\"gs_md_w\" style=\"display:none\"><div id=\"gsc_md_cbym\" style=\"display:none\" class=\"gs_md_d gs_md_wn gs_ttzi\" role=\"dialog\" aria-hidden=\"true\" aria-labelledby=\"gsc_md_cbym-t\"><a id=\"gsc_md_cbym-x\" href=\"#\" role=\"button\" aria-label=\"Cancel\" class=\"gs_ico gs_ico_X gs_ico_Xt\" onclick=\"return gs_md_cls('gsc_md_cbym',event)\"></a><h2 id=\"gsc_md_cbym-t\">Merged citations</h2><p>This \"Cited by\" count includes citations to the following articles in Scholar. The ones marked <span id=\"gsc_md_cbym_s\">*</span> may be different from the article in the profile.</p><div id=\"gsc_md_cbym_l\"></div><div class=\"gs_md_btns\"><button type=\"button\" id=\"gsc_md_cbym_cancel\" class=\"\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Done</span></span></button></div><script>function gsc_md_show_cbym(x,e){var a=gs_id(\"gsc_md_cbym_e\");a&&(a.href=a.href.replace(/(&citation_for_view=)[^&]*/,\"$1\"+x));gs_md_ldw(\"gsc_md_cbym\",e,\"gsc_md_cbym_cancel\",\"gsc_md_cbym_l\",\"/citations?hl\\x3den\\x26oe\\x3dASCII\\x26user\\x3dHI-I6C0AAAAJ\\x26view_op\\x3dlist_works\",\"merge_btn=1&s=\"+x);}gs_evt_clk(gs_id(\"gsc_md_cbym_cancel\"),gs_bind(gs_md_cls,\"gsc_md_cbym\"));</script></div><script>gs_md_ifc(\"gsc_md_cbym\",\"gsc_md_cbym-x\");</script><div id=\"gsc_md_cbyd\" style=\"display:none\" class=\"gs_md_d gs_md_wn gs_ttzi\" role=\"dialog\" aria-hidden=\"true\" aria-labelledby=\"gsc_md_cbyd-t\"><a id=\"gsc_md_cbyd-x\" href=\"#\" role=\"button\" aria-label=\"Cancel\" class=\"gs_ico gs_ico_X gs_ico_Xt\" onclick=\"return gs_md_cls('gsc_md_cbyd',event)\"></a><h2 id=\"gsc_md_cbyd-t\">Duplicate citations</h2><p>The following articles are merged in Scholar. Their <a id=\"gsc_md_cbyd_c\" href=\"#\">combined citations</a> are counted only for the first article.</p><div id=\"gsc_md_cbyd_l\"></div><div class=\"gs_md_btns\"><button type=\"button\" id=\"gsc_md_cbyd_cancel\" class=\"\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Done</span></span></button></div><script>function gsc_md_show_cbyd(a,x,u,e){var f=gs_id(\"gsc_md_cbyd_f\"),m=gs_id(\"gsc_md_cbyd_merge\"),s=u+\",\"+x;f&&(f.elements.s.value=s);gs_id(\"gsc_md_cbyd_c\").href=a;if(m){m.disabled=false;gs_udis(m);}gs_md_ldw(\"gsc_md_cbyd\",e,m?m.id:\"gsc_md_cbyd_cancel\",\"gsc_md_cbyd_l\",\"/citations?hl\\x3den\\x26oe\\x3dASCII\\x26user\\x3dHI-I6C0AAAAJ\\x26view_op\\x3dlist_works\",\"merge_btn=1&s=\"+s,function(){if(f&&m){var c=f.elements.choose;m.disabled=!c;(c?gs_udis:gs_xdis)(m);c&&gs_ssel(c[0]);}});}gs_evt_clk(gs_id(\"gsc_md_cbyd_cancel\"),gs_bind(gs_md_cls,\"gsc_md_cbyd\"));</script></div><script>gs_md_ifc(\"gsc_md_cbyd\",\"gsc_md_cbyd-x\");</script><div id=\"gsc_md_hist\" style=\"display:none\" class=\"gs_md_d gs_md_wn gs_ttzi\" role=\"dialog\" aria-hidden=\"true\" aria-labelledby=\"gsc_md_hist-t\"><a id=\"gsc_md_hist-x\" href=\"#\" role=\"button\" aria-label=\"Cancel\" class=\"gs_ico gs_ico_X gs_ico_Xt\" onclick=\"return gs_md_cls('gsc_md_hist',event)\"></a><h2 id=\"gsc_md_hist-t\">Citations per year</h2><div id=\"gsc_md_hist_c\"></div></div><script>gs_md_ifc(\"gsc_md_hist\",\"gsc_md_hist-x\");</script></div><style>#gs_alrt_w{position:fixed;width:100%;z-index:1050;display:none;opacity:0;transition:opacity .3s ease-out;}#gs_alrt_w.gs_vis{opacity:1}#gs_alrt_p{position:absolute;top:-15px;right:0;width:100%;text-align:center;}#gs_alrt{display:inline-block;font-size:13px;line-height:16px;padding:0 16px 6px 16px;}#gs_alrt_m,#gs_alrt_u{display:inline-block;padding-top:7px;}#gs_alrt_l:link,#gs_alrt_l:visited{margin-left:16px;padding:7px 0 6px 0;color:#222;text-decoration:underline;}#gs_alrt_l:hover{color:#1a0dab}#gs_alrt_l:active{color:#d14836}</style><div id=\"gs_alrt_w\"><div id=\"gs_alrt_p\"><form><input type=\"hidden\" id=\"gs_alrt_ss\"></form><form action=\"\" method=\"post\" id=\"gs_alrt\" class=\"gs_alrt\"><span id=\"gs_alrt_m\"></span><span id=\"gs_alrt_h\"></span><span id=\"gs_alrt_u\" style=\"display:none\"><a id=\"gs_alrt_l\" href=\"javascript:void(0)\" onclick=\"gs_id('gs_alrt').submit();return false\"></a></span></form></div></div><script>var gs_wa_t;function gs_wa_hf(e){gs_uevt(document,\"click\",gs_wa_hf);clearTimeout(gs_wa_t);gs_wa_t=undefined;var d=gs_id(\"gs_alrt_w\");if(e){setTimeout(function(){d.style.display=\"none\";gs_uvis(d);},0);}else{gs_vis(d,0);}}function gs_wa_sf(){gs_visi(\"gs_alrt_w\",1);gs_evt(document,\"click\",gs_wa_hf);clearTimeout(gs_wa_t);gs_wa_t=setTimeout(gs_wa_hf,60000);}function gs_wa_m(m,l,u,f) {var x,h,e;gs_id(\"gs_alrt_m\").innerHTML=m;if(l&&u&&f){x=gs_id(\"gs_alrt_l\");\"innerText\" in x?(x.innerText=l):(x.textContent=l);gs_id(\"gs_alrt\").action=u;h=gs_id(\"gs_alrt_h\");h.innerHTML=\"\";for(i in f){e=document.createElement(\"input\");e.name=i;e.type=\"hidden\";e.value=f[i];h.appendChild(e);}gs_visi(\"gs_alrt_u\",2);} else {gs_visi(\"gs_alrt_u\",0);}gs_wa_sf();}gs_evt_rdy(function(){gs_evt(window,\"pagehide\",gs_bind(gs_visi,\"gs_alrt_w\",0));});</script><div id=\"gsc_bdy\"><div id=\"gsc_rsb_m\" role=\"search\"><div class=\"gsc_rsb_s_m\" id=\"gsc_rsb_lg_m\"><a href=\"/schhp?hl=en&amp;oe=ASCII\" id=\"gsc_rsb_icol_m\" aria-label=\"Scholar Home\"><span class=\"gs_oph\">Scholar</span></a><form action=\"/citations\" id=\"gsc_rsb_f_m\" class=\"gsc_rsb_foff\"><input type=hidden name=hl value=\"en\"><input type=hidden name=oe value=\"ASCII\"><input type=\"hidden\" name=\"view_op\" value=\"search_authors\"><input type=\"text\" class=\"gs_in_txt\" name=\"mauthors\" value=\"\" data-iq=\"\" id=\"gsc_rsb_fin_m\" size=\"57\" maxlength=\"256\" autocapitalize=\"off\" aria-label=\"Search\"><button type=\"submit\" id=\"gsc_rsb_fbt_m\" aria-label=\"Search Authors\" class=\"gs_btnSB gs_in_ib gs_btn_half\"><span class=\"gs_wr\"><span class=\"gs_lbl\"></span><span class=\"gs_ico\"></span></span></button></form></div></div><div class=\"gsc_lcl\" role=\"main\" id=\"gsc_prf_w\"><div id=\"gsc_prf\" class=\"gs_scl\"><div id=\"gsc_prf_pu\"><style>#gsc_prf_pup{width:113px;height:150px;}.gs_el_ta #gsc_prf_pup{width:75px;height:100px;}.gs_el_ph #gsc_prf_pup{width:56px;height:75px;}@media print{#gs_top #gsc_prf_pup{width:60pt;height:80pt;}}</style><a href=\"/citations?user=HI-I6C0AAAAJ&amp;hl=en&amp;oe=ASCII\" id=\"gsc_prf_pua\"><img src=\"/citations?view_op=view_photo&amp;user=HI-I6C0AAAAJ&amp;citpid=1\" sizes=\"print 60px,(max-width:599px) 56px,(max-width:981px) 75px,113px\" srcset=\"/citations?view_op=view_photo&amp;user=HI-I6C0AAAAJ&amp;citpid=1 113w,/citations?view_op=medium_photo&amp;user=HI-I6C0AAAAJ&amp;citpid=1 225w\" id=\"gsc_prf_pup\" alt=\"Jeff Leek\"></a></div><div id=\"gsc_prf_i\"><div id=\"gsc_prf_ib\"><div id=\"gsc_prf_ibi\"><div style=\"position:relative\" class=\"gs_ibl\"><button type=\"button\" id=\"gsc_fol_btn\" aria-controls=\"gsc_fol_dd\" aria-haspopup=\"true\" class=\"gs_btnMW gs_in_ib gs_in_se gs_btn_mnu gs_btn_mn2 gs_btn_act gs_btn_hph\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Follow</span><span class=\"gs_ico\"></span><span class=\"gs_icm\"></span></span></button><div class=\"gs_md_se\" id=\"gsc_fol_dd\"><form method=\"post\" id=\"gsc_fol_f\" action=\"/citations?hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;view_op=list_works\"><input type=\"hidden\" name=\"xsrf\" value=\"AMstHGQAAAAAWMEqeGtrWh87-Pcodju8HOZi918GC1qw\"><input type=\"hidden\" name=\"user\" value=\"HI-I6C0AAAAJ\"><div id=\"gsc_fol_ml\">Email</div><div class=\"gs_in_txtf\"><input type=\"text\" class=\"gs_in_txt\" name=\"email_for_op\" value=\"\" data-iq=\"\" id=\"gsc_fol_m\" maxlength=\"100\" autocapitalize=\"off\" autocorrect=\"off\"></div><div id=\"gsc_fol_cb\"><div class=\"gsc_fol_cr\"><span class=\"gs_in_cb\" onclick=\"void(0)\"><input type=\"checkbox\" name=\"follow_articles_btn\" id=\"gsc_fol_a\" value=\"1\" checked><label for=\"gsc_fol_a\">Follow new articles</label><span class=\"gs_chk\"></span><span class=\"gs_cbx\"></span></span></div><div class=\"gsc_fol_cr\"><span class=\"gs_in_cb\" onclick=\"void(0)\"><input type=\"checkbox\" name=\"follow_citations_btn\" id=\"gsc_fol_c\" value=\"1\"><label for=\"gsc_fol_c\">Follow new citations</label><span class=\"gs_chk\"></span><span class=\"gs_cbx\"></span></span></div></div><div><button type=\"submit\" id=\"gsc_fol_b\" class=\" gs_btn_cre\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Create alert</span></span></button><button type=\"button\" id=\"gsc_fol_x\" class=\"\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Cancel</span></span></button></div></form></div></div><script>gs_md_ifc(\"gsc_fol_dd\",\"gsc_fol_m\");!function(){var t=gs_id(\"gsc_fol_btn\"),d=\"gsc_fol_dd\",b=gs_id(\"gsc_fol_b\");var a=gs_id(\"gsc_fol_a\"),c=gs_id(\"gsc_fol_c\"),m=gs_id(\"gsc_fol_m\"),r=/\\S+@\\S+\\.\\S+/,s=function(x){return x.checked&&!x.disabled},f=function(){((b.disabled=!r.test(m.value)||!s(a)&&!s(c))?gs_xdis:gs_udis)(b);};f();gs_evt_rdy(f);gs_evt_all([a,c],\"click\",f);gs_evt_inp(m,f);gs_evt_clk(gs_id(\"gsc_fol_x\"),gs_bind(gs_md_cls,d));gs_evt_clk(t,function(e){if(gs_id(d).offsetWidth){gs_md_cls(d,e);}else{gs_xsel(t);gs_md_opn(d,e,gs_bind(gs_usel,t));b.disabled?gs_tfcs(m):b.focus();}},1,[13,32,38,40]);}();</script></div></div><div id=\"gsc_prf_in\">Jeff Leek</div><div class=\"gsc_prf_il\">Associate Professor of Biostatistics, Johns Hopkins Bloomberg School of Public Health</div><div class=\"gsc_prf_il\"><a href=\"/citations?view_op=search_authors&amp;hl=en&amp;oe=ASCII&amp;mauthors=label:statistics\" class=\"gsc_prf_ila\">Statistics</a>, <a href=\"/citations?view_op=search_authors&amp;hl=en&amp;oe=ASCII&amp;mauthors=label:computing\" class=\"gsc_prf_ila\">Computing</a>, <a href=\"/citations?view_op=search_authors&amp;hl=en&amp;oe=ASCII&amp;mauthors=label:genomics\" class=\"gsc_prf_ila\">Genomics</a>, <a href=\"/citations?view_op=search_authors&amp;hl=en&amp;oe=ASCII&amp;mauthors=label:personalized_medicine\" class=\"gsc_prf_ila\">Personalized Medicine</a>, <a href=\"/citations?view_op=search_authors&amp;hl=en&amp;oe=ASCII&amp;mauthors=label:scientific_communication\" class=\"gsc_prf_ila\">Scientific Communication</a></div><div class=\"gsc_prf_il\" id=\"gsc_prf_ivh\">Verified email at jhsph.edu - <a href=\"http://jtleek.com/\" rel=\"nofollow\">Homepage</a></div></div></div></div><div id=\"gsc_rsb\" role=\"navigation\"><div class=\"gsc_rsb_s\" id=\"gsc_rsb_lg\"><a href=\"/schhp?hl=en&amp;oe=ASCII\" id=\"gsc_rsb_icol\" aria-label=\"Scholar Home\"><span class=\"gs_oph\">Scholar</span></a><form action=\"/citations\" id=\"gsc_rsb_f\" class=\"gsc_rsb_foff\"><input type=hidden name=hl value=\"en\"><input type=hidden name=oe value=\"ASCII\"><input type=\"hidden\" name=\"view_op\" value=\"search_authors\"><input type=\"text\" class=\"gs_in_txt\" name=\"mauthors\" value=\"\" data-iq=\"\" id=\"gsc_rsb_fin\" size=\"57\" maxlength=\"256\" autocapitalize=\"off\" aria-label=\"Search\"><button type=\"submit\" id=\"gsc_rsb_fbt\" aria-label=\"Search Authors\" class=\"gs_btnSB gs_in_ib gs_btn_half\"><span class=\"gs_wr\"><span class=\"gs_lbl\"></span><span class=\"gs_ico\"></span></span></button></form><div id=\"gsc_rsb_gpl\"><button type=\"button\" onclick=\"window.location='/citations?hl\\x3den\\x26oe\\x3dASCII'\" class=\" gs_btn_act\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Get my own profile</span></span></button></div></div><div class=\"gsc_rsb_s\"><div id=\"gsc_rsb_t\">Google Scholar</div><table id=\"gsc_rsb_st\"><tr><th class=\"gsc_rsb_sc1\"><a href=\"javascript:void(0)\" onclick=\"gs_md_hist_opn(event);\">Citation indices</a></th><th class=\"gsc_rsb_sth\">All</th><th class=\"gsc_rsb_sth\">Since 2012</th></tr><tr><td class=\"gsc_rsb_sc1\"><a href=\"javascript:void(0)\" class=\"gsc_rsb_f\" title='This is the number of citations to all publications. The second column has the &quot;recent&quot; version of this metric which is the number of new citations in the last 5 years to all publications.'>Citations</a></td><td class=\"gsc_rsb_std\">4631</td><td class=\"gsc_rsb_std\">3709</td></tr><tr><td class=\"gsc_rsb_sc1\"><a href=\"javascript:void(0)\" class=\"gsc_rsb_f\" title='h-index is the largest number h such that h publications have at least h citations. The second column has the &quot;recent&quot; version of this metric which is the largest number h such that h publications have at least h new citations in the last 5 years.'>h-index</a></td><td class=\"gsc_rsb_std\">24</td><td class=\"gsc_rsb_std\">24</td></tr><tr><td class=\"gsc_rsb_sc1\"><a href=\"javascript:void(0)\" class=\"gsc_rsb_f\" title='i10-index is the number of publications with at least 10 citations. The second column has the &quot;recent&quot; version of this metric which is the number of publications that have received at least 10 new citations in the last 5 years.'>i10-index</a></td><td class=\"gsc_rsb_std\">37</td><td class=\"gsc_rsb_std\">37</td></tr></table><div id=\"gsc_g\"><div id=\"gsc_g_x\"><span class=\"gsc_g_t\" style=\"left:3px\">2009</span><span class=\"gsc_g_t\" style=\"left:35px\">2010</span><span class=\"gsc_g_t\" style=\"left:67px\">2011</span><span class=\"gsc_g_t\" style=\"left:99px\">2012</span><span class=\"gsc_g_t\" style=\"left:131px\">2013</span><span class=\"gsc_g_t\" style=\"left:163px\">2014</span><span class=\"gsc_g_t\" style=\"left:195px\">2015</span><span class=\"gsc_g_t\" style=\"left:227px\">2016</span><span class=\"gsc_g_t\" style=\"left:259px\">2017</span></div><div id=\"gsc_g_bars\"><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:8px;height:6px;z-index:9\"><span class=\"gsc_g_al\">119</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:40px;height:10px;z-index:8\"><span class=\"gsc_g_al\">188</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:72px;height:16px;z-index:7\"><span class=\"gsc_g_al\">296</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:104px;height:25px;z-index:6\"><span class=\"gsc_g_al\">449</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:136px;height:32px;z-index:5\"><span class=\"gsc_g_al\">582</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:168px;height:39px;z-index:4\"><span class=\"gsc_g_al\">701</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:200px;height:43px;z-index:3\"><span class=\"gsc_g_al\">783</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:232px;height:57px;z-index:2\"><span class=\"gsc_g_al\">1015</span></a><a href=\"javascript:void(0)\" class=\"gsc_g_a\" style=\"left:264px;height:9px;z-index:1\"><span class=\"gsc_g_al\">174</span></a></div></div><script>function gs_md_hist_opn(e){gs_md_ldw(\"gsc_md_hist\",e,0,\"gsc_md_hist_c\",\"/citations?hl\\x3den\\x26oe\\x3dASCII\\x26user\\x3dHI-I6C0AAAAJ\\x26view_op\\x3dcitations_histogram\",\"\",function(){var x=gs_id(\"gsc_md_hist_b\");x&&gsc_scroll_right(x);});}gs_evt_rdy(function(){gs_evt_clk(gs_id(\"gsc_g\"),gs_md_hist_opn);});</script></div></div><div class=\"gsc_lcl\" role=\"complementary\" id=\"gsc_art\"><form method=\"post\" action=\"/citations?hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;view_op=list_works\" id=\"citationsForm\"><input type=\"hidden\" name=\"xsrf\" value=\"AMstHGQAAAAAWMEqeGtrWh87-Pcodju8HOZi918GC1qw\"><table id=\"gsc_a_t\"><thead id=\"gsc_a_hd\"><tr id=\"gsc_a_tr0\" aria-hidden=\"true\"><th class=\"gsc_a_t\" id=\"gsc_a_tr0_t\"></th><th class=\"gsc_a_c\"></th><th class=\"gsc_a_y\"></th></tr><tr id=\"gsc_a_trh\"><th class=\"gsc_a_t\" id=\"gsc_a_trh_t\" scope=\"col\"><span id=\"gsc_a_ta\"><a href=\"/citations?hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;view_op=list_works&amp;sortby=title\" class=\"gsc_a_a\">Title</a></span><span id=\"gsc_a_nn\">1&ndash;20</span></th><th class=\"gsc_a_c\" scope=\"col\"><span id=\"gsc_a_ca\">Cited by</span></th><th class=\"gsc_a_y\" scope=\"col\"><span class=\"gsc_a_h\"><a href=\"/citations?hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;view_op=list_works&amp;sortby=pubdate\" class=\"gsc_a_a\">Year</a></span></th></tr></thead><tbody id=\"gsc_a_b\"><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:UeHWp8X0CEIC\" class=\"gsc_a_at\">Tackling the widespread and critical impact of batch effects in high-throughput data</a><div class=\"gs_gray\">JT Leek, RB Scharpf, HC Bravo, D Simcha, B Langmead, WE Johnson, ...</div><div class=\"gs_gray\">Nature Reviews Genetics 11 (10), 733-739<span class=\"gs_oph\">, 2010</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=16403016020126782741\" class=\"gsc_a_ac\">658</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2010</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:d1gkVwhDpl0C\" class=\"gsc_a_at\">Capturing heterogeneity in gene expression studies by surrogate variable analysis</a><div class=\"gs_gray\">JT Leek, JD Storey</div><div class=\"gs_gray\">PLoS Genet 3 (9), e161<span class=\"gs_oph\">, 2007</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=14943880347723800617\" class=\"gsc_a_ac\">633</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2007</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:u5HHmVD_uO8C\" class=\"gsc_a_at\">Significance analysis of time course microarray experiments</a><div class=\"gs_gray\">JD Storey, W Xiao, JT Leek, RG Tompkins, RW Davis</div><div class=\"gs_gray\">Proceedings of the National Academy of Sciences of the United States of ...<span class=\"gs_oph\">, 2005</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=8692182196465189153\" class=\"gsc_a_ac\">520</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2005</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:Se3iqnhoufwC\" class=\"gsc_a_at\">Temporal dynamics and genetic control of transcription in the human prefrontal cortex</a><div class=\"gs_gray\">C Colantuoni, BK Lipska, T Ye, TM Hyde, R Tao, JT Leek, EA Colantuoni, ...</div><div class=\"gs_gray\">Nature 478 (7370), 519-523<span class=\"gs_oph\">, 2011</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=15401379057477374674\" class=\"gsc_a_ac\">341</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2011</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:8k81kl-MbHgC\" class=\"gsc_a_at\">The sva package for removing batch effects and other unwanted variation in high-throughput experiments</a><div class=\"gs_gray\">JT Leek, WE Johnson, HS Parker, AE Jaffe, JD Storey</div><div class=\"gs_gray\">Bioinformatics 28 (6), 882-883<span class=\"gs_oph\">, 2012</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=8144311546232376767\" class=\"gsc_a_ac\">298</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2012</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:IjCSPb-OGe4C\" class=\"gsc_a_at\">Cloud-scale RNA-sequencing differential expression analysis with Myrna</a><div class=\"gs_gray\">B Langmead, KD Hansen, JT Leek</div><div class=\"gs_gray\">Genome biology 11 (8), R83<span class=\"gs_oph\">, 2010</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=9392678635432440961\" class=\"gsc_a_ac\">264</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2010</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:u-x6o8ySG0sC\" class=\"gsc_a_at\">EDGE: extraction and analysis of differential gene expression</a><div class=\"gs_gray\">JT Leek, E Monsen, AR Dabney, JD Storey</div><div class=\"gs_gray\">Bioinformatics 22 (4), 507-508<span class=\"gs_oph\">, 2006</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=8862746171832697560\" class=\"gsc_a_ac\">218</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2006</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:qjMakFHDy7sC\" class=\"gsc_a_at\">Systems-level dynamic analyses of fate change in murine embryonic stem cells</a><div class=\"gs_gray\">R Lu, F Markowetz, RD Unwin, JT Leek, EM Airoldi, BD MacArthur, ...</div><div class=\"gs_gray\">Nature 462 (7271), 358-362<span class=\"gs_oph\">, 2009</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=4114573275173642545\" class=\"gsc_a_ac\">194</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2009</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:2osOgNQ5qMEC\" class=\"gsc_a_at\">A general framework for multiple testing dependence</a><div class=\"gs_gray\">JT Leek, JD Storey</div><div class=\"gs_gray\">Proceedings of the National Academy of Sciences 105 (48), 18718-18723<span class=\"gs_oph\">, 2008</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=6862569568109470250\" class=\"gsc_a_ac\">193</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2008</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:5nxA0vEk-isC\" class=\"gsc_a_at\">Bump hunting to identify differentially methylated regions in epigenetic epidemiology studies</a><div class=\"gs_gray\">AE Jaffe, P Murakami, H Lee, JT Leek, MD Fallin, AP Feinberg, RA Irizarry</div><div class=\"gs_gray\">International journal of epidemiology 41 (1), 200-209<span class=\"gs_oph\">, 2012</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=13917393765835926103\" class=\"gsc_a_ac\">192</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2012</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:9yKSN-GCB0IC\" class=\"gsc_a_at\">The optimal discovery procedure for large-scale significance testing, with applications to comparative microarray experiments</a><div class=\"gs_gray\">JD Storey, JY Dai, JT Leek</div><div class=\"gs_gray\">Biostatistics 8 (2), 414-432<span class=\"gs_oph\">, 2007</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=6987233910267027632\" class=\"gsc_a_ac\">149</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2007</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:0EnyYjriUFMC\" class=\"gsc_a_at\">ReCount: a multi-experiment resource of analysis-ready RNA-seq gene count datasets</a><div class=\"gs_gray\">AC Frazee, B Langmead, JT Leek</div><div class=\"gs_gray\">BMC bioinformatics 12 (1), 449<span class=\"gs_oph\">, 2011</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=5499264026160610251\" class=\"gsc_a_ac\">106</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2011</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:LkGwnXOMwfcC\" class=\"gsc_a_at\">Sequencing technology does not eliminate biological variability</a><div class=\"gs_gray\">KD Hansen, Z Wu, RA Irizarry, JT Leek</div><div class=\"gs_gray\">Nature biotechnology 29 (7), 572-573<span class=\"gs_oph\">, 2011</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=13984583295080419572\" class=\"gsc_a_ac\">86</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2011</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:ldfaerwXgEUC\" class=\"gsc_a_at\">Statistics: P values are just the tip of the iceberg</a><div class=\"gs_gray\">JT Leek, RD Peng</div><div class=\"gs_gray\">Nature 520 (7549), 612<span class=\"gs_oph\">, 2015</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=7990176079596762579\" class=\"gsc_a_ac\">64</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2015</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:hqOjcs7Dif8C\" class=\"gsc_a_at\">Significance analysis and statistical dissection of variably methylated regions</a><div class=\"gs_gray\">AE Jaffe, AP Feinberg, RA Irizarry, JT Leek</div><div class=\"gs_gray\">Biostatistics 13 (1), 166-178<span class=\"gs_oph\">, 2012</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=8713725236803269731\" class=\"gsc_a_ac\">61</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2012</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:-f6ydRqryjwC\" class=\"gsc_a_at\">An estimate of the science-wise false discovery rate and application to the top medical literature</a><div class=\"gs_gray\">LR Jager, JT Leek</div><div class=\"gs_gray\">Biostatistics, kxt007<span class=\"gs_oph\">, 2013</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=7889935075533581268,15367445605081037117\" class=\"gsc_a_ac\">60</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2013</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:BqipwSGYUEgC\" class=\"gsc_a_at\">Developmental regulation of human cortex transcription and its clinical relevance at single base resolution</a><div class=\"gs_gray\">AE Jaffe, J Shin, L Collado-Torres, JT Leek, R Tao, C Li, Y Gao, Y Jia, ...</div><div class=\"gs_gray\">Nature neuroscience 18 (1), 154-161<span class=\"gs_oph\">, 2015</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=10491065069728818067\" class=\"gsc_a_ac\">49</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2015</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:TQgYirikUcIC\" class=\"gsc_a_at\">Differential expression analysis of RNA-seq data at single-base resolution</a><div class=\"gs_gray\">AC Frazee, S Sabunciyan, KD Hansen, RA Irizarry, JT Leek</div><div class=\"gs_gray\">Biostatistics, kxt053<span class=\"gs_oph\">, 2014</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=1603345619966351948\" class=\"gsc_a_ac\">42</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2014</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:bEWYMUwI8FkC\" class=\"gsc_a_at\">Inflammatory molecular signature associated with infectious agents in psychosis</a><div class=\"gs_gray\">LN Hayes, EG Severance, JT Leek, KL Gressitt, C Rohleder, JM Coughlin, ...</div><div class=\"gs_gray\">Schizophrenia bulletin 40 (5), 963-972<span class=\"gs_oph\">, 2014</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=16513563558053101459\" class=\"gsc_a_ac\">36</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2014</span></td></tr><tr class=\"gsc_a_tr\"><td class=\"gsc_a_t\"><a href=\"/citations?view_op=view_citation&amp;hl=en&amp;oe=ASCII&amp;user=HI-I6C0AAAAJ&amp;citation_for_view=HI-I6C0AAAAJ:_kc_bZDykSQC\" class=\"gsc_a_at\">Gene expression anti-profiles as a basis for accurate universal cancer signatures</a><div class=\"gs_gray\">HC Bravo, V Pihur, M McCall, RA Irizarry, JT Leek</div><div class=\"gs_gray\">BMC bioinformatics 13 (1), 272<span class=\"gs_oph\">, 2012</span></div></td><td class=\"gsc_a_c\"><a href=\"http://scholar.google.com/scholar?oi=bibs&amp;hl=en&amp;oe=ASCII&amp;cites=5170926751178488472\" class=\"gsc_a_ac\">32</a></td><td class=\"gsc_a_y\"><span class=\"gsc_a_h\">2012</span></td></tr></tbody></table><div id=\"gsc_a_sp\" style=\"display:none\"></div><div id=\"gsc_a_err\" class=\"gs_alrt\" style=\"display:none\">The system can&#39;t perform the operation now. Try again later.</div><div id=\"gsc_lwp\"><div id=\"gsc_bpf\"><button type=\"button\" id=\"gsc_bpf_prev\" aria-label=\"Previous\" disabled class=\"gs_btnPL gs_in_ib gs_btn_half gs_btn_slt gs_dis\"><span class=\"gs_wr\"><span class=\"gs_lbl\"></span><span class=\"gs_ico gs_ico_dis\"></span></span></button><button type=\"button\" id=\"gsc_bpf_more\" class=\" gs_btn_smd\"><span class=\"gs_wr\"><span class=\"gs_lbl\">Show more</span></span></button><button type=\"button\" id=\"gsc_bpf_next\" aria-label=\"Next\" class=\"gs_btnPR gs_in_ib gs_btn_half gs_btn_srt\"><span class=\"gs_wr\"><span class=\"gs_lbl\"></span><span class=\"gs_ico\"></span></span></button></div></div></form></div><div class=\"gsc_lcl\"><div id=\"gs_ftr\" role=\"contentinfo\"><div id=\"gsc_dscl\">Dates and citation counts are estimated and are determined automatically by a computer program.</div><div id=\"gsc_ftr_h\"><a href=\"/intl/en/scholar/citations.html\">Help</a> <a href=\"//www.google.com/intl/en/policies/privacy/\">Privacy</a> <a href=\"//www.google.com/intl/en/policies/terms/\">Terms</a> <a href=\"//support.google.com/scholar/contact/general\">Provide feedback</a> <a href=\"/citations?hl=en&amp;oe=ASCII\">Get my own profile</a></div></div></div></div><script>var gs_zvb;!function(u){gs_zvb=new Image();gs_zvb.onload=gs_zvb.onerror=function(){gs_zvb=0};gs_zvb.src=u;}(\"https://id.google.com/verify/RAAAACRNjG627dtUm61vG1Erpm7pOZlWnOT6vwmz9huCJQxmXqIy2AB459AdnsMRptsrVx_gnEGuMwcbryR_bNTMK429aE8MZMkWoitEdkPmb51D.gif\");</script><noscript><img src=\"https://id.google.com/verify/RAAAACRNjG627dtUm61vG1Erpm7pOZlWnOT6vwmz9huCJQxmXqIy2AB459AdnsMRptsrVx_gnEGuMwcbryR_bNTMK429aE8MZMkWoitEdkPmb51D.gif\" width=\"1\" height=\"1\" alt=\"\" style=\"margin:-1px\"></noscript></div><div id=\"gs_rdy\"></div></body></html>"

As we can see, the output is unformatted xml code lines.

We can deal with that using the `xml package`.

``` r
library(XML)

## The url
url <- "http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en"

## Parse the html code
html <- htmlTreeParse(url, useInternalNodes=TRUE)

## Get the page title
xpathSApply(html,"//title", xmlValue)
```

    ## [1] "Jeff Leek - Google Scholar Citations"

Another way is to use `httr` package. For more information see [this](https://cran.r-project.org/web/packages/httr/httr.pdf).

``` r
library(httr)

## The url
url <- "http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en"

html2 <- GET(url)

## Get the url content
pageContent <- content(html2, as="text")

## Parsing
parsedHTML <- htmlParse(pageContent, asText = TRUE)

## get the title
xpathSApply(parsedHTML, "//title", xmlValue)
```

    ## [1] "Jeff Leek - Google Scholar Citations"

Another use for the `httr` package is to access websites with password.

``` r
## this page needs a password
pg = GET("http://httpbin.org/basic-auth/user/passwd")
pg
```

    ## Response [http://httpbin.org/basic-auth/user/passwd]
    ##   Date: 2017-03-08 10:12
    ##   Status: 401
    ##   Content-Type: <unknown>
    ## <EMPTY BODY>

``` r
## giving the username and password
pg = GET("http://httpbin.org/basic-auth/user/passwd", authenticate("user", "passwd"))
pg
```

    ## Response [http://httpbin.org/basic-auth/user/passwd]
    ##   Date: 2017-03-08 10:12
    ##   Status: 200
    ##   Content-Type: application/json
    ##   Size: 47 B
    ## {
    ##   "authenticated": true, 
    ##   "user": "user"
    ## }

``` r
##
names(pg)
```

    ##  [1] "url"         "status_code" "headers"     "all_headers" "cookies"    
    ##  [6] "content"     "date"        "times"       "request"     "handle"

Example:- get the number of characters in the 10th, 20th, 30th and 100th lines of HTML from this page:

<http://biostat.jhsph.edu/~jleek/contact.html>

``` r
urlConnection <-url("http://biostat.jhsph.edu/~jleek/contact.html") 

htmlLines <- readLines(urlConnection)

nchar(htmlLines[10])
```

    ## [1] 45

``` r
nchar(htmlLines[20])
```

    ## [1] 31

``` r
nchar(htmlLines[30])
```

    ## [1] 7

``` r
nchar(htmlLines[100])
```

    ## [1] 25

Reading from APIs
=================

Using the `httr` package we can get data from different websites.

Access github API
-----------------

First, Register an application with the Github API [here](https://github.com/settings/applications/new). See [here](https://datatweet.wordpress.com/2014/05/14/reading-data-from-github-api-using-r/) for help.

``` r
# library(httr)
# library(httpuv)
# library(jsonlite)
# 
# oauth_endpoints("github")
# 
# 
# clientID <- "cb5b1be3e375e3a310b5"
# ClientSecret <- "03420ea6c24f1e25af2b174369d17d5996a4025c"
# 
# github.app <- oauth_app("github", clientID, ClientSecret)
# 
# ## Get OAuth credentials. Note that authorization code is the key you entered above
# github_token <- oauth2.0_token(oauth_endpoints("github"), github.app)
# 
# ## Use the API
# gtoken <- config(token = github_token)
# req <- GET("https://api.github.com/users/jtleek/repos", gtoken)
# 
# ## Get the url content
# jsonContent <- content(req)
# json2 = jsonlite::fromJSON(toJSON(jsonContent))
```

Reading fixed width files
=========================

Data in a fixed-width text file is arranged in rows and columns, with one entry per row. Each column has a fixed width, specified in characters, which determines the maximum amount of data it can contain. No delimiters are used to separate the fields in the file. Instead, smaller quantities of data are padded with spaces to fill the allotted space, such that the start of a given column can always be specified as an offset from the beginning of a line.

To read a fixed width file into a data frame, use `read.fwf`. This function needs the width of each column as one of the inputs.

To get the width of each column, i opened the [file](http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for) using notepad (you can use any text editor) and counted them manually. And since the actual data in this file starts from the 5th row, we will skip 4 rows.

``` r
## The url of the fixed width file
url <- url("https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for")

width <- c(1, 9, 5, 4, 1, 3, 5, 4, 1, 3, 5, 4, 1, 3, 5, 4, 1, 3)

## We named the spaces (with width = 1) "filler" so that we can remove them 
colNames <- c("filler", "week", "filler", "sstNino12", "filler", "sstaNino12", "filler", "sstNino3", "filler", "sstaNino3", "filler", "sstNino34", "filler", "sstaNino34", "filler", "sstNino4", "filler", "sstaNino4")

## Read the file
fwf <- read.fwf(url, width, header = FALSE, skip = 4, col.names = colNames)
head(fwf)
```

    ##   filler      week filler.1 sstNino12 filler.2 sstaNino12 filler.3
    ## 1     NA 03JAN1990       NA      23.4        -        0.4       NA
    ## 2     NA 10JAN1990       NA      23.4        -        0.8       NA
    ## 3     NA 17JAN1990       NA      24.2        -        0.3       NA
    ## 4     NA 24JAN1990       NA      24.4        -        0.5       NA
    ## 5     NA 31JAN1990       NA      25.1        -        0.2       NA
    ## 6     NA 07FEB1990       NA      25.8                 0.2       NA
    ##   sstNino3 filler.4 sstaNino3 filler.5 sstNino34 filler.6 sstaNino34
    ## 1     25.1        -       0.3       NA      26.6                 0.0
    ## 2     25.2        -       0.3       NA      26.6                 0.1
    ## 3     25.3        -       0.3       NA      26.5        -        0.1
    ## 4     25.5        -       0.4       NA      26.5        -        0.1
    ## 5     25.8        -       0.2       NA      26.7                 0.1
    ## 6     26.1        -       0.1       NA      26.8                 0.1
    ##   filler.7 sstNino4 filler.8 sstaNino4
    ## 1       NA     28.6                0.3
    ## 2       NA     28.6                0.3
    ## 3       NA     28.6                0.3
    ## 4       NA     28.4                0.2
    ## 5       NA     28.4                0.2
    ## 6       NA     28.4                0.3

``` r
## remove the non-data columns "columns with name filler". In other words, get the indices of the columns that are not fillers.
notFiller <- grep("^[^filler]", names(fwf))

## subsetting the dataframe
fwf <- fwf[,notFiller]
head(fwf)
```

    ##        week sstNino12 sstaNino12 sstNino3 sstaNino3 sstNino34 sstaNino34
    ## 1 03JAN1990      23.4        0.4     25.1       0.3      26.6        0.0
    ## 2 10JAN1990      23.4        0.8     25.2       0.3      26.6        0.1
    ## 3 17JAN1990      24.2        0.3     25.3       0.3      26.5        0.1
    ## 4 24JAN1990      24.4        0.5     25.5       0.4      26.5        0.1
    ## 5 31JAN1990      25.1        0.2     25.8       0.2      26.7        0.1
    ## 6 07FEB1990      25.8        0.2     26.1       0.1      26.8        0.1
    ##   sstNino4 sstaNino4
    ## 1     28.6       0.3
    ## 2     28.6       0.3
    ## 3     28.6       0.3
    ## 4     28.4       0.2
    ## 5     28.4       0.2
    ## 6     28.4       0.3

Reading `jpeg` images
=====================

``` r
library(jpeg)
myurl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fjeff.jpg"
z <- tempfile()
download.file(myurl,z,mode="wb")
pic <- readJPEG(z, native = T)
file.remove(z) # cleanup
```

    ## [1] TRUE

``` r
quantile(pic, probs = c(0.3,0.8))
```

    ##       30%       80% 
    ## -15259150 -10575416
