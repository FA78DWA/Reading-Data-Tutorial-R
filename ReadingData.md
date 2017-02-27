-   [Downloading Data from the internet](#downloading-data-from-the-internet)

Downloading Data from the internet
==================================

Use `download.file()` that takes the url and the destination on your computer to save the file at.

Example: download the 2006 microdata survey about housing for the state of Idaho.

``` r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
download.file(url, "data.csv") ##save in the current dir, and name the file "data.csv"
```

List the files in the current directory

``` r
list.files("./")
```

    ## [1] "data.csv"            "ReadingData.nb.html" "ReadingData.Rmd"

Use `date()` to get the downloading date.

``` r
downloadDate <- date()
downloadDate
```

    ## [1] "Mon Feb 27 08:53:53 2017"

Note that: \* If the file url starts with **http** you can use `download.file()`. \* It is also okay to use `download.file()` in case of **https** and **windows**. \* But on **Mac**, you might need to set `method="curl"` to download from **https** url. \* The download duration depends on the file size.
