---
title       : ShinyMap
subtitle    : Data product developement course project
author      : Eric Fan
job         : Engineer
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

--- 
<!-- Limit image width and height -->
<style type='text/css'>
img {
    max-height: 672px;
    max-width: 1156px;
}
</style>

<!-- Center image on slide -->
<script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js"></script>
<script type='text/javascript'>
$(function() {
    $("p:has(img)").addClass('centered');
});
</script>
## About ShinyMap
- What is ShinyMap?  
A simple API that allow user to do a quick search on google map.
- Where is ShinyMap?  
You can ifnd ShinyMap at [ShinyMap](http://hipporz.shinyapps.io/ShinyMap/)  
- Input  
A description of the location, it could be zip code, city name or detailed address.  
A number indicating the level of zoom.
- Output  
The google map in response to the search.
- Source  
This App is built with the shiny package in R.  
Packages ggmap and ggplot2 are also used.

--- .class #id 
## Search location  
Enter 'Chicago' and hit search

![map1](/Users/eric/Downloads/ShinyMap slides/assets/img/map1.png)



---
## zooming
Adjust zoom value from 10(default) to 15.

![map1](assets/img/map2.png)

---





## Source Code--ui.R

```r
library(shiny)
library(ggmap)
shinyUI(fluidPage(
  titlePanel("Shiny Map"),
  sidebarLayout (
    sidebarPanel(
           textInput("address",label=h3("Location"),
                     value="" ),          
           submitButton("Search"),
           sliderInput("zoom",label=h3("zoom"),
                       min=5,max=20,value=10)
      ),
    
    mainPanel(
      plotOutput("map",width="800px",height="800px")
      )  
  )))
```


--- 

## Source Code--server.R

```r
library(shiny)
library(ggplot2)
library(ggmap)

shinyServer(function(input, output) {
  output$map<-renderPlot({
    
      getMap<-get_map(input$address,zoom=input$zoom)
      ggmap(getMap,extent="panel")  
})
})
```

