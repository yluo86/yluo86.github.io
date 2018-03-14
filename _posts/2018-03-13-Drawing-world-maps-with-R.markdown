---
layout: post
title:  "Drawing world maps using R"
date:   2018-03-13
categories: science
tags: bioinformatics R
author: Yang
permalink: rmaps
---
Previously I posted about using [googleVis](https://github.com/mages/googleVis)  to create [MotionChart]({{site.baseurl}}/MotionChart.html) or Maps. Recently in my work, I need to reproduce and modify a map of America from [Reich et al 2012](https://www.nature.com/articles/nature11258) Nature paper on *reconstructing Native American population history*. To keep it brief, I want to reproduce a map similar to this without manually manipulating the figure in a graphics software.![original]({{site.baseurl}}/assets/images/nat_map.png){:height="400px" width="400px"}

To start, here are the prerequisties you need to install:

```R
# dependency libraries
install.packages(c("ggplot2", "devtools", "dplyr", "stringr","ggrepel"))

# install map packages.
install.packages(c("maps", "mapdata"))

# load up required libraries
require(ggplot2)
require(ggrepel)
require(maps)
require(mapdata)
```

Now you can basically draw any map that's already in the mapdata library

```R
world<-map_data("world")
world[sample(nrow(world),10),]
#            long       lat group order           region       subregion
#79207   31.25879  53.01670  1309 79207           Russia            <NA>
#45362  -88.57159  15.90107   665 45362        Guatemala            <NA>
#45012  -69.69424  76.98946   664 45012        Greenland            <NA>
#73720  150.36406 -10.18965  1195 73720 Papua New Guinea            <NA>
#49830  105.71856   2.85918   813 49830        Indonesia          Jemaja
#10850   26.48926  44.08398   197 10850         Bulgaria            <NA>
#92946  -76.24424  36.95264  1501 92946              USA            <NA>
#24227 -113.87852  75.37544   368 24227           Canada Melville Island
#65955   22.62402 -17.78164  1041 65955          Namibia            <NA>
#89250   60.93320  41.22900  1418 89250     Turkmenistan            <NA>  
```

The data is organized as a data frame with longitude and latitude information of each country/region. Next step is to identify regions of interests. For me, this includes all [South American countries]({{site.baseurl}}/assets/data/amr_countries.txt), but you can pick whatever you like.

```R
#selecting countries of interest, this steps need some manual curation, some of the countries might have a different quoted name
countrylist<-scan("data/amr_countries.txt",as.character())

lat<-map_data("world",region=countrylist)
#remove some islands for visually pleasing effects
idx<-which(lat$long<=-75 & lat$lat<=-20)

set.seed(42)
#here is the background map for region of interest
g1<-ggplot() + geom_polygon(data = lat[-idx,], aes(x=long, y = lat, group = group),fill='NA',colour="black") +
  coord_fixed(1.5)

print(g1)
```
![nat_map]({{site.baseurl}}/assets/images/nat_plot_raw.png)
So we now have the background map, and we can now add some labels on top! This requires knowing geographical coordinates of your data points. To me, these are the Native American populations sampled from the 2012 paper. Luckily, authors did a great job in including all geographical information in their supplementary table ([ST1]({{site/baseurl}}/assets/data/nat_pop.txt)). All I needed to do is to extract the information and project it onto the background map.

```R
#native american population of interest
# 52 Native american population based on Reich et al. 2012 (Supplementary Table 1)
nat<-read.table("data/nat_pop.txt",h=T,sep="\t",stringsAsFactors = F)
nat$lat<-as.numeric(nat$Latitude)

#for populations with multiple sampling locations, I took the average of the two
nat[grepl("/",nat$Latitude),]$lat<-rowMeans(matrix(as.numeric(unlist(strsplit(nat[grepl("/",nat$Latitude),]$Latitude,"/"))),ncol=2,byrow=T))
nat$long<-as.numeric(nat$Longitude)
nat[grepl("/",nat$Longitude),]$long<-rowMeans(matrix(as.numeric(unlist(strsplit(nat[grepl("/",nat$Longitude),]$Longitude,"/"))),ncol=2,byrow=T))

#define sub populations of interests
andean<-nat[nat$Population=="Aymara"|nat$Population=="Quechua",]
northen_amerind<-nat[nat$Population %in%c("Maya1"   ,  "Mixe"    ,    "Kaqchikel"  ),]
central_amerind<-nat[nat$Population %in% c("Pima"   ,   "Zapotec1"  ,"Mixtec"    ,"Yaqui"  ,   "Chorotega" , "Tepehuano"),]
south_amerind<-nat[nat$Population %in% c("Piapoco" ,  "Karitiana", "Surui"    , "Wayuu"  ,   "Jamamadi" , "Parakana" , "Guarani" ,
                                         "Kaingang",  "Ticuna" ,   "Palikur",   "Toba"    ,  "Arara" ,    "Wichi"     ,"Chane"  ,  
                                         "Guahibo" ),]
pops<-rbind(andean,northen_amerind,central_amerind,south_amerind)

df<-nat[nat$Population%in%pops$Population,]

#modify with multiple names
df[df$Population=="Maya1",]$Population<-"Maya"
df[df$Population=="Zapotec1" ,]$Population<-"Zapotec"
df$region<-"NA"
df$region<-ifelse(df$Language.family=="Equatorial-Tucanoan" | df$Language.family=="Ge-Pano-Carib","Southern Amerindian",df$region)
df$region<-ifelse(df$Language.family=="Central-Amerind","Central Amerindian",df$region)
df$region<-ifelse(df$Language.family=="Northern Amerind","Northern Amerindian",df$region)
df$region<-ifelse(df$Language.family=="Andean","Andean",df$region)
```

With some data curation, we are now ready to plot the final product! And a side note, [ggrepel](https://cran.r-project.org/web/packages/ggrepel/vignettes/ggrepel.html) is such a great package if you have a lot of labelling to do. You can see below what a difference it makes before and after.

```R
p2<-g1+  geom_point(data = df, aes(x = long, y = lat,colour=region), size = 2)+
    geom_text_repel(data=df,aes(x = long, y = lat, label = Population,colour=region),box.padding = 0.4,point.padding = 0.25,size=4,show.legend=FALSE)+
     theme_void()+theme(legend.position = c(.2, .3),legend.title=element_blank(),legend.text = element_text( size = 12))
print(p2)
```

![nat_map]({{site/baseurl}}/assets/images/nat_map_final.png)
