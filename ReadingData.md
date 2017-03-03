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

    ## [1] "Fri Mar 03 13:07:07 2017"

Reading Excel Files
===================

First download the file using `download.file()`

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
download.file(url, "data.xlsx",mode="wb")
date()
```

    ## [1] "Fri Mar 03 13:07:08 2017"

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

This package inherits from `data.frame` this means that all functions that accept `data.frame` will work on `data.table`. Also, `data.table` is much faster in data subsetting, grouping and updating, because it is written in C.

Starting by downloading the package and load it. Then, create a `data table`

``` r
library(data.table)

#create a data table with 9 rows and 3 columns
DT <- data.table(x=rnorm(9), y=rep(c("a","b","c"), each=3), z=rnorm(9))

DT
```

    ##              x y           z
    ## 1: -1.03871927 a  1.84657841
    ## 2:  1.32844903 a  1.02595415
    ## 3:  0.57736423 a  0.63293168
    ## 4: -0.95500623 b  0.97076231
    ## 5: -1.05255293 b -1.54383292
    ## 6: -1.00418252 b  0.64481354
    ## 7:  0.60799311 c -0.51342557
    ## 8:  0.05421943 c -0.77541982
    ## 9: -0.58864945 c  0.01581029

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
    ## 1: 1.328449 a 1.025954

``` r
## Get the rows with y=c
DT[DT$y=="c",]
```

    ##              x y           z
    ## 1:  0.60799311 c -0.51342557
    ## 2:  0.05421943 c -0.77541982
    ## 3: -0.58864945 c  0.01581029

``` r
## Get certain rows for example 1st, 5th, and 9th
DT[c(1,5,9),]
```

    ##             x y           z
    ## 1: -1.0387193 a  1.84657841
    ## 2: -1.0525529 b -1.54383292
    ## 3: -0.5886495 c  0.01581029

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

    ## [1]  1.84657841  1.02595415  0.63293168  0.97076231 -1.54383292  0.64481354
    ## [7] -0.51342557 -0.77541982  0.01581029

``` r
## Get certain columns for example 1st and 3rd
DT[,c(1,3)]
```

    ##              x           z
    ## 1: -1.03871927  1.84657841
    ## 2:  1.32844903  1.02595415
    ## 3:  0.57736423  0.63293168
    ## 4: -0.95500623  0.97076231
    ## 5: -1.05255293 -1.54383292
    ## 6: -1.00418252  0.64481354
    ## 7:  0.60799311 -0.51342557
    ## 8:  0.05421943 -0.77541982
    ## 9: -0.58864945  0.01581029

Operating on a subset of a data table
-------------------------------------

Until now the subsetting either rows or columns are intuitive. `DT` is a 2-dimensional array(table), and you can get a specific element using DT\[i,j\] format just like `matlab`.

But, what if we want to take the average of the columns, or do any other operation on a subset of the `DT`. In this case **DT\[i,j,by\]** comes very handy. **DT\[i,j,by\]** means Take DT subset rows by **i**, then compute **j** grouped by **by**. Examples.

``` r
## Calculate the mean of x and sum of z
DT[,list(mean(x), sum(z))]
```

    ##            V1       V2
    ## 1: -0.2301205 2.304172

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

    ##              x y           z            w
    ## 1: -1.03871927 a  1.84657841 3.4098518415
    ## 2:  1.32844903 a  1.02595415 1.0525819171
    ## 3:  0.57736423 a  0.63293168 0.4006025117
    ## 4: -0.95500623 b  0.97076231 0.9423794698
    ## 5: -1.05255293 b -1.54383292 2.3834200829
    ## 6: -1.00418252 b  0.64481354 0.4157845065
    ## 7:  0.60799311 c -0.51342557 0.2636058205
    ## 8:  0.05421943 c -0.77541982 0.6012758908
    ## 9: -0.58864945 c  0.01581029 0.0002499653

``` r
## Add new column m = log(x+z+5). Note that we used {} to put in multi-line expression. Each expression ends with ';'
DT[,m:={tmp <- (x+z); log2(tmp+5)}]
```

    ##              x y           z            w        m
    ## 1: -1.03871927 a  1.84657841 3.4098518415 2.538006
    ## 2:  1.32844903 a  1.02595415 1.0525819171 2.878608
    ## 3:  0.57736423 a  0.63293168 0.4006025117 2.634662
    ## 4: -0.95500623 b  0.97076231 0.9423794698 2.326467
    ## 5: -1.05255293 b -1.54383292 2.3834200829 1.265205
    ## 6: -1.00418252 b  0.64481354 0.4157845065 2.214321
    ## 7:  0.60799311 c -0.51342557 0.2636058205 2.348960
    ## 8:  0.05421943 c -0.77541982 0.6012758908 2.097206
    ## 9: -0.58864945 c  0.01581029 0.0002499653 2.146382

``` r
## BOLEAN OPERATIONS: Add new column a shows if x>=0 or <0
DT[,a:= x>=0]
```

    ##              x y           z            w        m     a
    ## 1: -1.03871927 a  1.84657841 3.4098518415 2.538006 FALSE
    ## 2:  1.32844903 a  1.02595415 1.0525819171 2.878608  TRUE
    ## 3:  0.57736423 a  0.63293168 0.4006025117 2.634662  TRUE
    ## 4: -0.95500623 b  0.97076231 0.9423794698 2.326467 FALSE
    ## 5: -1.05255293 b -1.54383292 2.3834200829 1.265205 FALSE
    ## 6: -1.00418252 b  0.64481354 0.4157845065 2.214321 FALSE
    ## 7:  0.60799311 c -0.51342557 0.2636058205 2.348960  TRUE
    ## 8:  0.05421943 c -0.77541982 0.6012758908 2.097206  TRUE
    ## 9: -0.58864945 c  0.01581029 0.0002499653 2.146382 FALSE

``` r
## GROUPING: get the mean of(x+w) when a is TRUE and a is False, then add the result in new column b. Note that b has only 2 values.
DT[,b:=mean(x+w), by=a]
```

    ##              x y           z            w        m     a         b
    ## 1: -1.03871927 a  1.84657841 3.4098518415 2.538006 FALSE 0.5025151
    ## 2:  1.32844903 a  1.02595415 1.0525819171 2.878608  TRUE 1.2215230
    ## 3:  0.57736423 a  0.63293168 0.4006025117 2.634662  TRUE 1.2215230
    ## 4: -0.95500623 b  0.97076231 0.9423794698 2.326467 FALSE 0.5025151
    ## 5: -1.05255293 b -1.54383292 2.3834200829 1.265205 FALSE 0.5025151
    ## 6: -1.00418252 b  0.64481354 0.4157845065 2.214321 FALSE 0.5025151
    ## 7:  0.60799311 c -0.51342557 0.2636058205 2.348960  TRUE 1.2215230
    ## 8:  0.05421943 c -0.77541982 0.6012758908 2.097206  TRUE 1.2215230
    ## 9: -0.58864945 c  0.01581029 0.0002499653 2.146382 FALSE 0.5025151

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

    ##              x y           z            w        m     a         b
    ## 1: -1.03871927 2  1.84657841 3.4098518415 2.538006 FALSE 0.5025151
    ## 2:  1.32844903 2  1.02595415 1.0525819171 2.878608  TRUE 1.2215230
    ## 3:  0.57736423 2  0.63293168 0.4006025117 2.634662  TRUE 1.2215230
    ## 4: -0.95500623 2  0.97076231 0.9423794698 2.326467 FALSE 0.5025151
    ## 5: -1.05255293 2 -1.54383292 2.3834200829 1.265205 FALSE 0.5025151
    ## 6: -1.00418252 2  0.64481354 0.4157845065 2.214321 FALSE 0.5025151
    ## 7:  0.60799311 2 -0.51342557 0.2636058205 2.348960  TRUE 1.2215230
    ## 8:  0.05421943 2 -0.77541982 0.6012758908 2.097206  TRUE 1.2215230
    ## 9: -0.58864945 2  0.01581029 0.0002499653 2.146382 FALSE 0.5025151

``` r
DT2
```

    ##              x y           z            w        m     a         b
    ## 1: -1.03871927 2  1.84657841 3.4098518415 2.538006 FALSE 0.5025151
    ## 2:  1.32844903 2  1.02595415 1.0525819171 2.878608  TRUE 1.2215230
    ## 3:  0.57736423 2  0.63293168 0.4006025117 2.634662  TRUE 1.2215230
    ## 4: -0.95500623 2  0.97076231 0.9423794698 2.326467 FALSE 0.5025151
    ## 5: -1.05255293 2 -1.54383292 2.3834200829 1.265205 FALSE 0.5025151
    ## 6: -1.00418252 2  0.64481354 0.4157845065 2.214321 FALSE 0.5025151
    ## 7:  0.60799311 2 -0.51342557 0.2636058205 2.348960  TRUE 1.2215230
    ## 8:  0.05421943 2 -0.77541982 0.6012758908 2.097206  TRUE 1.2215230
    ## 9: -0.58864945 2  0.01581029 0.0002499653 2.146382 FALSE 0.5025151

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

After completing the configurations, start oading the library. `RMySQL` might need the `DBI` package, so yeah, download it.

``` r
library(DBI)
library(RMySQL)
```

Then we will use some mySQL data from UCSC. You can find more details about it and how to connect on the server [Here](https://genome.ucsc.edu/goldenPath/help/mysql.html) .

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
## Show the result: list of all the databases that are available on that server
result
```

    ##               Database
    ## 1   information_schema
    ## 2              ailMel1
    ## 3              allMis1
    ## 4              anoCar1
    ## 5              anoCar2
    ## 6              anoGam1
    ## 7              apiMel1
    ## 8              apiMel2
    ## 9              aplCal1
    ## 10             aptMan1
    ## 11             balAcu1
    ## 12             bosTau2
    ## 13             bosTau3
    ## 14             bosTau4
    ## 15             bosTau5
    ## 16             bosTau6
    ## 17             bosTau7
    ## 18             bosTau8
    ## 19           bosTauMd3
    ## 20             braFlo1
    ## 21             caeJap1
    ## 22              caePb1
    ## 23              caePb2
    ## 24             caeRem2
    ## 25             caeRem3
    ## 26             calJac1
    ## 27             calJac3
    ## 28             calMil1
    ## 29             canFam1
    ## 30             canFam2
    ## 31             canFam3
    ## 32             cavPor3
    ## 33                 cb1
    ## 34                 cb3
    ## 35                ce10
    ## 36                ce11
    ## 37                 ce2
    ## 38                 ce4
    ## 39                 ce6
    ## 40             cerSim1
    ## 41             chlSab2
    ## 42             choHof1
    ## 43             chrPic1
    ## 44                 ci1
    ## 45                 ci2
    ## 46             criGri1
    ## 47             danRer1
    ## 48            danRer10
    ## 49             danRer2
    ## 50             danRer3
    ## 51             danRer4
    ## 52             danRer5
    ## 53             danRer6
    ## 54             danRer7
    ## 55             dasNov3
    ## 56             dipOrd1
    ## 57                 dm1
    ## 58                 dm2
    ## 59                 dm3
    ## 60                 dm6
    ## 61                 dp2
    ## 62                 dp3
    ## 63             droAna1
    ## 64             droAna2
    ## 65             droEre1
    ## 66             droGri1
    ## 67             droMoj1
    ## 68             droMoj2
    ## 69             droPer1
    ## 70             droSec1
    ## 71             droSim1
    ## 72             droVir1
    ## 73             droVir2
    ## 74             droYak1
    ## 75             droYak2
    ## 76             eboVir3
    ## 77             echTel1
    ## 78             echTel2
    ## 79             equCab1
    ## 80             equCab2
    ## 81             eriEur1
    ## 82             eriEur2
    ## 83             felCat3
    ## 84             felCat4
    ## 85             felCat5
    ## 86             felCat8
    ## 87                 fr1
    ## 88                 fr2
    ## 89                 fr3
    ## 90             gadMor1
    ## 91             galGal2
    ## 92             galGal3
    ## 93             galGal4
    ## 94             galGal5
    ## 95             galVar1
    ## 96             gasAcu1
    ## 97              gbMeta
    ## 98             geoFor1
    ## 99                  go
    ## 100           go080130
    ## 101           go140213
    ## 102           go150121
    ## 103            gorGor3
    ## 104            gorGor4
    ## 105            gorGor5
    ## 106            hetGla1
    ## 107            hetGla2
    ## 108               hg16
    ## 109               hg17
    ## 110               hg18
    ## 111               hg19
    ## 112        hg19Patch10
    ## 113         hg19Patch2
    ## 114         hg19Patch5
    ## 115         hg19Patch9
    ## 116               hg38
    ## 117         hg38Patch2
    ## 118         hg38Patch3
    ## 119         hg38Patch6
    ## 120         hg38Patch7
    ## 121         hg38Patch9
    ## 122            hgFixed
    ## 123             hgTemp
    ## 124          hgcentral
    ## 125            latCha1
    ## 126            loxAfr3
    ## 127            macEug1
    ## 128            macEug2
    ## 129            macFas5
    ## 130            melGal1
    ## 131            melUnd1
    ## 132            micMur1
    ## 133            micMur2
    ## 134               mm10
    ## 135         mm10Patch1
    ## 136         mm10Patch4
    ## 137                mm5
    ## 138                mm6
    ## 139                mm7
    ## 140                mm8
    ## 141                mm9
    ## 142            monDom1
    ## 143            monDom4
    ## 144            monDom5
    ## 145            musFur1
    ## 146            myoLuc2
    ## 147            nomLeu1
    ## 148            nomLeu2
    ## 149            nomLeu3
    ## 150            ochPri2
    ## 151            ochPri3
    ## 152            oreNil1
    ## 153            oreNil2
    ## 154            ornAna1
    ## 155            ornAna2
    ## 156            oryCun2
    ## 157            oryLat2
    ## 158            otoGar3
    ## 159            oviAri1
    ## 160            oviAri3
    ## 161            panPan1
    ## 162            panTro1
    ## 163            panTro2
    ## 164            panTro3
    ## 165            panTro4
    ## 166            panTro5
    ## 167            papAnu2
    ## 168            papHam1
    ## 169 performance_schema
    ## 170            petMar1
    ## 171            petMar2
    ## 172            ponAbe2
    ## 173            priPac1
    ## 174            proCap1
    ## 175     proteins120806
    ## 176     proteins121210
    ## 177     proteins140122
    ## 178     proteins150225
    ## 179     proteins160229
    ## 180           proteome
    ## 181            pteVam1
    ## 182            rheMac1
    ## 183            rheMac2
    ## 184            rheMac3
    ## 185            rheMac8
    ## 186                rn3
    ## 187                rn4
    ## 188                rn5
    ## 189                rn6
    ## 190            sacCer1
    ## 191            sacCer2
    ## 192            sacCer3
    ## 193            saiBol1
    ## 194            sarHar1
    ## 195            sorAra1
    ## 196            sorAra2
    ## 197           sp120323
    ## 198           sp121210
    ## 199           sp140122
    ## 200           sp150225
    ## 201           sp160229
    ## 202            speTri2
    ## 203            strPur1
    ## 204            strPur2
    ## 205            susScr2
    ## 206            susScr3
    ## 207            taeGut1
    ## 208            taeGut2
    ## 209            tarSyr1
    ## 210            tarSyr2
    ## 211               test
    ## 212            tetNig1
    ## 213            tetNig2
    ## 214            triMan1
    ## 215            tupBel1
    ## 216            turTru2
    ## 217            uniProt
    ## 218            vicPac1
    ## 219            vicPac2
    ## 220           visiGene
    ## 221            xenTro1
    ## 222            xenTro2
    ## 223            xenTro3
    ## 224            xenTro7

Now, we are going to connect on a specific dataset `hg19`

``` r
## Connect on the "hg19" database on the server
hg <- dbConnect(MySQL(), user="genome",db="hg19", host="genome-mysql.cse.ucsc.edu")

## Get the list of tabels names in this database
allTables <- dbListTables(hg)
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
