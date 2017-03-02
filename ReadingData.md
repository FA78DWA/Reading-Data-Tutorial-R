-   [Downloading Data from the internet](#downloading-data-from-the-internet)
-   [Reading Excel Files](#reading-excel-files)
    -   [Using `xlsx` Package](#using-xlsx-package)
        -   [Reading specific rows and columns form the Excel file](#reading-specific-rows-and-columns-form-the-excel-file)
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

    ## [1] "Thu Mar 02 17:24:17 2017"

Reading Excel Files
===================

First download the file using `download.file()`

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
download.file(url, "data.xlsx",mode="wb")
date()
```

    ## [1] "Thu Mar 02 17:24:18 2017"

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

    ##             x y           z
    ## 1: -1.2345010 a -0.70733729
    ## 2:  1.4574064 a -0.56909865
    ## 3: -0.3041914 a  1.18882800
    ## 4: -0.2227956 b -0.29525013
    ## 5: -0.6002536 b  0.58807632
    ## 6:  0.6939075 b -0.88263183
    ## 7:  0.4322602 c -0.94177727
    ## 8: -0.9263215 c -0.03740395
    ## 9: -0.9307859 c  0.22576969

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

    ##           x y          z
    ## 1: 1.457406 a -0.5690986

``` r
## Get the rows with y=c
DT[DT$y=="c",]
```

    ##             x y           z
    ## 1:  0.4322602 c -0.94177727
    ## 2: -0.9263215 c -0.03740395
    ## 3: -0.9307859 c  0.22576969

``` r
## Get certain rows for example 1st, 5th, and 9th
DT[c(1,5,9),]
```

    ##             x y          z
    ## 1: -1.2345010 a -0.7073373
    ## 2: -0.6002536 b  0.5880763
    ## 3: -0.9307859 c  0.2257697

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

    ## [1] -0.70733729 -0.56909865  1.18882800 -0.29525013  0.58807632 -0.88263183
    ## [7] -0.94177727 -0.03740395  0.22576969

``` r
## Get certain columns for example 1st and 3rd
DT[,c(1,3)]
```

    ##             x           z
    ## 1: -1.2345010 -0.70733729
    ## 2:  1.4574064 -0.56909865
    ## 3: -0.3041914  1.18882800
    ## 4: -0.2227956 -0.29525013
    ## 5: -0.6002536  0.58807632
    ## 6:  0.6939075 -0.88263183
    ## 7:  0.4322602 -0.94177727
    ## 8: -0.9263215 -0.03740395
    ## 9: -0.9307859  0.22576969

Operating on a subset of a data table
-------------------------------------

Until now the subsetting either rows or columns are intuitive. `DT` is a 2-dimensional array(table), and you can get a specific element using DT\[i,j\] format just like `matlab`.

But, what if we want to take the average of the columns, or do any other operation on a subset of the `DT`. In this case **DT\[i,j,by\]** comes very handy. **DT\[i,j,by\]** means Take DT subset rows by **i**, then compute **j** grouped by **by**. Examples.

``` r
## Calculate the mean of x and sum of z
DT[,list(mean(x), sum(z))]
```

    ##            V1        V2
    ## 1: -0.1816972 -1.430825

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

    ##             x y           z           w
    ## 1: -1.2345010 a -0.70733729 0.500326046
    ## 2:  1.4574064 a -0.56909865 0.323873273
    ## 3: -0.3041914 a  1.18882800 1.413312024
    ## 4: -0.2227956 b -0.29525013 0.087172641
    ## 5: -0.6002536 b  0.58807632 0.345833758
    ## 6:  0.6939075 b -0.88263183 0.779038955
    ## 7:  0.4322602 c -0.94177727 0.886944420
    ## 8: -0.9263215 c -0.03740395 0.001399055
    ## 9: -0.9307859 c  0.22576969 0.050971955

``` r
## Add new column m = log(x+z+5). Note that we used {} to put in multi-line expression. Each expression ends with ';'
DT[,m:={tmp <- (x+z); log2(tmp+5)}]
```

    ##             x y           z           w        m
    ## 1: -1.2345010 a -0.70733729 0.500326046 1.612665
    ## 2:  1.4574064 a -0.56909865 0.323873273 2.557853
    ## 3: -0.3041914 a  1.18882800 1.413312024 2.556953
    ## 4: -0.2227956 b -0.29525013 0.087172641 2.164128
    ## 5: -0.6002536 b  0.58807632 0.345833758 2.318410
    ## 6:  0.6939075 b -0.88263183 0.779038955 2.266419
    ## 7:  0.4322602 c -0.94177727 0.886944420 2.166871
    ## 8: -0.9263215 c -0.03740395 0.001399055 2.013024
    ## 9: -0.9307859 c  0.22576969 0.050971955 2.102653

``` r
## BOLEAN OPERATIONS: Add new column a shows if x>=0 or <0
DT[,a:= x>=0]
```

    ##             x y           z           w        m     a
    ## 1: -1.2345010 a -0.70733729 0.500326046 1.612665 FALSE
    ## 2:  1.4574064 a -0.56909865 0.323873273 2.557853  TRUE
    ## 3: -0.3041914 a  1.18882800 1.413312024 2.556953 FALSE
    ## 4: -0.2227956 b -0.29525013 0.087172641 2.164128 FALSE
    ## 5: -0.6002536 b  0.58807632 0.345833758 2.318410 FALSE
    ## 6:  0.6939075 b -0.88263183 0.779038955 2.266419  TRUE
    ## 7:  0.4322602 c -0.94177727 0.886944420 2.166871  TRUE
    ## 8: -0.9263215 c -0.03740395 0.001399055 2.013024 FALSE
    ## 9: -0.9307859 c  0.22576969 0.050971955 2.102653 FALSE

``` r
## GROUPING: get the mean of(x+w) when a is TRUE and a is False, then add the result in new column b. Note that b has only 2 values.
DT[,b:=mean(x+w), by=a]
```

    ##             x y           z           w        m     a          b
    ## 1: -1.2345010 a -0.70733729 0.500326046 1.612665 FALSE -0.3033056
    ## 2:  1.4574064 a -0.56909865 0.323873273 2.557853  TRUE  1.5244769
    ## 3: -0.3041914 a  1.18882800 1.413312024 2.556953 FALSE -0.3033056
    ## 4: -0.2227956 b -0.29525013 0.087172641 2.164128 FALSE -0.3033056
    ## 5: -0.6002536 b  0.58807632 0.345833758 2.318410 FALSE -0.3033056
    ## 6:  0.6939075 b -0.88263183 0.779038955 2.266419  TRUE  1.5244769
    ## 7:  0.4322602 c -0.94177727 0.886944420 2.166871  TRUE  1.5244769
    ## 8: -0.9263215 c -0.03740395 0.001399055 2.013024 FALSE -0.3033056
    ## 9: -0.9307859 c  0.22576969 0.050971955 2.102653 FALSE -0.3033056

**CAUTION**

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

    ##             x y           z           w        m     a          b
    ## 1: -1.2345010 2 -0.70733729 0.500326046 1.612665 FALSE -0.3033056
    ## 2:  1.4574064 2 -0.56909865 0.323873273 2.557853  TRUE  1.5244769
    ## 3: -0.3041914 2  1.18882800 1.413312024 2.556953 FALSE -0.3033056
    ## 4: -0.2227956 2 -0.29525013 0.087172641 2.164128 FALSE -0.3033056
    ## 5: -0.6002536 2  0.58807632 0.345833758 2.318410 FALSE -0.3033056
    ## 6:  0.6939075 2 -0.88263183 0.779038955 2.266419  TRUE  1.5244769
    ## 7:  0.4322602 2 -0.94177727 0.886944420 2.166871  TRUE  1.5244769
    ## 8: -0.9263215 2 -0.03740395 0.001399055 2.013024 FALSE -0.3033056
    ## 9: -0.9307859 2  0.22576969 0.050971955 2.102653 FALSE -0.3033056

``` r
DT
```

    ##             x y           z           w        m     a          b
    ## 1: -1.2345010 2 -0.70733729 0.500326046 1.612665 FALSE -0.3033056
    ## 2:  1.4574064 2 -0.56909865 0.323873273 2.557853  TRUE  1.5244769
    ## 3: -0.3041914 2  1.18882800 1.413312024 2.556953 FALSE -0.3033056
    ## 4: -0.2227956 2 -0.29525013 0.087172641 2.164128 FALSE -0.3033056
    ## 5: -0.6002536 2  0.58807632 0.345833758 2.318410 FALSE -0.3033056
    ## 6:  0.6939075 2 -0.88263183 0.779038955 2.266419  TRUE  1.5244769
    ## 7:  0.4322602 2 -0.94177727 0.886944420 2.166871  TRUE  1.5244769
    ## 8: -0.9263215 2 -0.03740395 0.001399055 2.013024 FALSE -0.3033056
    ## 9: -0.9307859 2  0.22576969 0.050971955 2.102653 FALSE -0.3033056

``` r
DT2
```

    ##             x y           z           w        m     a          b
    ## 1: -1.2345010 2 -0.70733729 0.500326046 1.612665 FALSE -0.3033056
    ## 2:  1.4574064 2 -0.56909865 0.323873273 2.557853  TRUE  1.5244769
    ## 3: -0.3041914 2  1.18882800 1.413312024 2.556953 FALSE -0.3033056
    ## 4: -0.2227956 2 -0.29525013 0.087172641 2.164128 FALSE -0.3033056
    ## 5: -0.6002536 2  0.58807632 0.345833758 2.318410 FALSE -0.3033056
    ## 6:  0.6939075 2 -0.88263183 0.779038955 2.266419  TRUE  1.5244769
    ## 7:  0.4322602 2 -0.94177727 0.886944420 2.166871  TRUE  1.5244769
    ## 8: -0.9263215 2 -0.03740395 0.001399055 2.013024 FALSE -0.3033056
    ## 9: -0.9307859 2  0.22576969 0.050971955 2.102653 FALSE -0.3033056
