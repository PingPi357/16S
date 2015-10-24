# top_100_heatmap_and_indicative_species_analyze
Jingzhe Jiang  
2015年10月19日  
# top 100 otus
combine top 100 otus of each oyster samples in cohort 2

## read data

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
origin_data <- read.csv("D:/bioinfo/UBC_data/otu_summaries/otu_table_os_top960_NormAndRanked.csv", header = TRUE, row.names = 1) # single number giving the column of the table which contains the row names. But dpyr never preserve row names, you should use add_rownames() to preserve them!
str(origin_data)
```

```
## 'data.frame':	960 obs. of  12 variables:
##  $ os1 : int  65781 6220 3572 1863 239 632 2239 2038 3537 9216 ...
##  $ os2 : int  166746 12708 762 174 456 899 1571 962 1218 669 ...
##  $ os3 : int  133838 3240 1622 799 1095 2425 1246 4931 1098 216 ...
##  $ os4 : int  114072 7716 138 1465 832 321 2280 243 7172 110 ...
##  $ os5 : int  116996 25435 1546 80 271 2153 1378 1093 286 2709 ...
##  $ os6 : int  53608 20694 2523 410 1598 3401 1585 5390 2444 705 ...
##  $ os7 : int  103203 2675 2239 83 4953 3386 2691 1232 4012 129 ...
##  $ os8 : int  32723 9560 11451 19998 15650 11040 7173 2677 2223 2059 ...
##  $ os9 : int  56342 7871 7209 17405 1020 4394 2064 3516 1243 65 ...
##  $ os10: int  112139 13210 770 155 10638 1705 5373 34 101 245 ...
##  $ os11: int  36434 3173 14906 1187 4509 9532 6013 2188 1448 9157 ...
##  $ os12: int  64483 7371 7489 2369 4602 2998 1427 3894 2628 352 ...
```

```r
row.names(origin_data) %>% head
```

```
## [1] "denovo662732"  "denovo1425070" "denovo814559"  "denovo1760022"
## [5] "denovo1559849" "denovo1327185"
```

```r
colnames(origin_data)
```

```
##  [1] "os1"  "os2"  "os3"  "os4"  "os5"  "os6"  "os7"  "os8"  "os9"  "os10"
## [11] "os11" "os12"
```

```r
mydata <- origin_data %>% add_rownames %>% tbl_df
mydata %>% glimpse
```

```
## Observations: 960
## Variables: 13
## $ rowname (chr) "denovo662732", "denovo1425070", "denovo814559", "deno...
## $ os1     (int) 65781, 6220, 3572, 1863, 239, 632, 2239, 2038, 3537, 9...
## $ os2     (int) 166746, 12708, 762, 174, 456, 899, 1571, 962, 1218, 66...
## $ os3     (int) 133838, 3240, 1622, 799, 1095, 2425, 1246, 4931, 1098,...
## $ os4     (int) 114072, 7716, 138, 1465, 832, 321, 2280, 243, 7172, 11...
## $ os5     (int) 116996, 25435, 1546, 80, 271, 2153, 1378, 1093, 286, 2...
## $ os6     (int) 53608, 20694, 2523, 410, 1598, 3401, 1585, 5390, 2444,...
## $ os7     (int) 103203, 2675, 2239, 83, 4953, 3386, 2691, 1232, 4012, ...
## $ os8     (int) 32723, 9560, 11451, 19998, 15650, 11040, 7173, 2677, 2...
## $ os9     (int) 56342, 7871, 7209, 17405, 1020, 4394, 2064, 3516, 1243...
## $ os10    (int) 112139, 13210, 770, 155, 10638, 1705, 5373, 34, 101, 2...
## $ os11    (int) 36434, 3173, 14906, 1187, 4509, 9532, 6013, 2188, 1448...
## $ os12    (int) 64483, 7371, 7489, 2369, 4602, 2998, 1427, 3894, 2628,...
```

```r
row.names(mydata) %>% head
```

```
## [1] "1" "2" "3" "4" "5" "6"
```
## select top 100 otus ids
from each samples (os7-os12), than combined and get rid of redundences, than filtrate from my data

```r
top_7 <- mydata %>% 
    arrange(desc(os7)) %>% 
      select(rowname) %>% 
        head(100)
top_8 <- mydata %>% 
    arrange(desc(os8)) %>% 
      select(rowname) %>% 
        head(100)
top_9 <- mydata %>% 
    arrange(desc(os9)) %>% 
      select(rowname) %>% 
        head(100)
top_10 <- mydata %>% 
    arrange(desc(os10)) %>% 
      select(rowname) %>% 
        head(100)
top_11 <- mydata %>% 
    arrange(desc(os11)) %>% 
      select(rowname) %>% 
        head(100)
top_12 <- mydata %>% 
    arrange(desc(os12)) %>% 
      select(rowname) %>% 
        head(100)
# all = TRUE means full Outer join tables
top_cohort2 <- merge(top_12,top_11, by = "rowname", all = TRUE)
top_cohort2 <- merge(top_cohort2,top_10, by = "rowname", all = TRUE)
top_cohort2 <- merge(top_cohort2,top_9, by = "rowname", all = TRUE)
top_cohort2 <- merge(top_cohort2,top_8, by = "rowname", all = TRUE)
top_cohort2 <- merge(top_cohort2,top_7, by = "rowname", all = TRUE)
top_cohort2_data <- merge(mydata, top_cohort2, by = "rowname", all.y = TRUE)
top_cohort2_data %>% arrange(desc(os8)) %>% head
```

```
##         rowname   os1    os2    os3    os4    os5   os6    os7   os8   os9
## 1  denovo662732 65781 166746 133838 114072 116996 53608 103203 32723 56342
## 2 denovo1760022  1863    174    799   1465     80   410     83 19998 17405
## 3 denovo1559849   239    456   1095    832    271  1598   4953 15650  1020
## 4  denovo814559  3572    762   1622    138   1546  2523   2239 11451  7209
## 5 denovo1327185   632    899   2425    321   2153  3401   3386 11040  4394
## 6 denovo1425070  6220  12708   3240   7716  25435 20694   2675  9560  7871
##     os10  os11  os12
## 1 112139 36434 64483
## 2    155  1187  2369
## 3  10638  4509  4602
## 4    770 14906  7489
## 5   1705  9532  2998
## 6  13210  3173  7371
```
## normalized columns
based on the total reads of each sample

```r
# clip total reads from excel, than paste into R
refs <- read.delim("refs.txt", header = TRUE)
```

```
## Warning in read.table(file = file, header = header, sep = sep, quote =
## quote, : incomplete final line found by readTableHeader on 'refs.txt'
```

```r
refs %>% glimpse
```

```
## Observations: 1
## Variables: 13
## $ seqID (fctr) totReads
## $ os1   (int) 494604
## $ os2   (int) 311167
## $ os3   (int) 493566
## $ os4   (int) 470232
## $ os5   (int) 547637
## $ os6   (int) 490970
## $ os7   (int) 501020
## $ os8   (int) 507057
## $ os9   (int) 478260
## $ os10  (int) 545820
## $ os11  (int) 496962
## $ os12  (int) 554586
```

```r
## top_normal <- top_cohort2_data[,2:13]*311167/as.numeric(refs[1,2:13])
## 两个dataframe相除必须维度一致，或者将分母转变为vector才可以相除。但above is not correct, but I don't know why!
top_normal <- top_cohort2_data[,2:13]*311167/refs[rep(1,251),2:13]
#两个dataframe相除必须维度一致，因此用rep(1,251)复制了refs中的ttlReads数据，使其变成dataframe。参见如下：
refs[rep(1,5),1:13]
```

```
##        seqID    os1    os2    os3    os4    os5    os6    os7    os8
## 1   totReads 494604 311167 493566 470232 547637 490970 501020 507057
## 1.1 totReads 494604 311167 493566 470232 547637 490970 501020 507057
## 1.2 totReads 494604 311167 493566 470232 547637 490970 501020 507057
## 1.3 totReads 494604 311167 493566 470232 547637 490970 501020 507057
## 1.4 totReads 494604 311167 493566 470232 547637 490970 501020 507057
##        os9   os10   os11   os12
## 1   478260 545820 496962 554586
## 1.1 478260 545820 496962 554586
## 1.2 478260 545820 496962 554586
## 1.3 478260 545820 496962 554586
## 1.4 478260 545820 496962 554586
```

```r
rownames(top_normal) <- top_cohort2_data$rowname
top_normal %>% str
```

```
## 'data.frame':	251 obs. of  12 variables:
##  $ os1 : num  17 294.4 194.4 44.7 0 ...
##  $ os2 : num  0 524 2051 185 3 ...
##  $ os3 : num  43.5 88.3 158.9 100.9 93.9 ...
##  $ os4 : num  0.662 251.458 251.458 898.63 0.662 ...
##  $ os5 : num  11.9 218.2 1877.3 176.1 13.1 ...
##  $ os6 : num  1.9 209.78 252.88 6.97 38.03 ...
##  $ os7 : num  10.6 187.6 261.5 31.1 37.3 ...
##  $ os8 : num  28.84 200.06 276.15 22.71 5.52 ...
##  $ os9 : num  135.33 201.69 65.71 709.83 4.55 ...
##  $ os10: num  2.85 55.87 18.81 174.45 1.71 ...
##  $ os11: num  227 424 128 929 221 ...
##  $ os12: num  65.65 1691.09 135.22 3.93 67.89 ...
```

```r
top_cohort2_data %>% str
```

```
## 'data.frame':	251 obs. of  13 variables:
##  $ rowname: chr  "denovo1015897" "denovo1017076" "denovo1022006" "denovo1031142" ...
##  $ os1    : int  27 468 309 71 0 2038 388 42 108 47 ...
##  $ os2    : int  0 524 2051 185 3 962 262 2 28 5 ...
##  $ os3    : int  69 140 252 160 149 4931 40 8 76 30 ...
##  $ os4    : int  1 380 380 1358 1 243 418 5 35 156 ...
##  $ os5    : int  21 384 3304 310 23 1093 588 22 374 86 ...
##  $ os6    : int  3 331 399 11 60 5390 212 5 470 10 ...
##  $ os7    : int  17 302 421 50 60 1232 143 135 131 418 ...
##  $ os8    : int  47 326 450 37 9 2677 555 1388 206 3 ...
##  $ os9    : int  208 310 101 1091 7 3516 49 87 410 62 ...
##  $ os10   : int  5 98 33 306 3 34 25 1009 7 8 ...
##  $ os11   : int  362 677 205 1483 353 2188 80 287 116 68 ...
##  $ os12   : int  117 3014 241 7 121 3894 40 109 416 142 ...
```

```r
identical(row.names(top_normal),top_cohort2_data$rowname)
```

```
## [1] TRUE
```

```r
identical(top_normal$os2,top_cohort2_data$os2) # one is num, one is int
```

```
## [1] FALSE
```

```r
identical(as.integer(top_normal$os2),top_cohort2_data$os2)
```

```
## [1] TRUE
```
## heatmap with ggplot2

```r
library(ggplot2)
library(reshape2)
# the dataframe is converted from wide format to a long format
top_melt <- top_normal %>% 
  add_rownames %>% 
    select(rowname,os7,os8,os9,os10,os11,os12) %>% 
      melt
```

```
## Using rowname as id variables
```

```r
(p <- ggplot(top_melt, aes(variable, rowname)) + geom_tile(aes(fill = log10(value)), colour = "white") + scale_fill_gradient2(low = "blue", mid = "white", high = "red", midpoint = 2) + theme(axis.text.y = element_text(size = 1), axis.text.x = element_text(size = 4)) + coord_fixed(ratio=0.15))
```

```
## Warning: Non Lab interpolation is deprecated
```

![](top_100_heatmap_and_indicative_species_analyze_files/figure-html/unnamed-chunk-4-1.png) 
## cluster otus on heatmap

```r
euc_dist <- top_normal %>% dist(method = "euclidean") 
otuclust <- hclust(euc_dist, method = "average")
den_order <- otuclust %>% 
  as.dendrogram %>%
    order.dendrogram
den_order %>% str
```

```
##  int [1:251] 200 65 116 88 218 56 212 204 231 106 ...
```

```r
# euc_dist %>% head
# typeof(euc_dist)
# mode(euc_dist)s
# class(euc_dist)
otuclust %>% str
```

```
## List of 7
##  $ merge      : int [1:250, 1:2] -22 -62 -215 -186 -36 3 -114 -206 -250 -107 ...
##  $ height     : num [1:250] 8.08 19.35 22.84 26.07 36.39 ...
##  $ order      : int [1:251] 200 65 116 88 218 56 212 204 231 106 ...
##  $ labels     : chr [1:251] "denovo1015897" "denovo1017076" "denovo1022006" "denovo1031142" ...
##  $ method     : chr "average"
##  $ call       : language hclust(d = euc_dist, method = "average")
##  $ dist.method: chr "euclidean"
##  - attr(*, "class")= chr "hclust"
```

```r
# otuclust %>% class
# otuclust %>% mode
# otuclust %>% as.dendrogram %>% str 
# otuclust %>% plot

table <- top_normal %>% add_rownames
table$rowname <- with(otuclust, reorder(labels, desc(order)))

top_melt <- table %>% 
    select(rowname,os7,os8,os9,os10,os11,os12) %>% 
      melt
```

```
## Using rowname as id variables
```

```r
(p <- ggplot(top_melt, aes(variable, rowname)) + geom_tile(aes(fill = log10(value)), colour = "white") + scale_fill_gradient2(low = "blue", mid = "white", high = "red", midpoint = 2) + theme(axis.text.y = element_text(size = 1), axis.text.x = element_text(size = 4)) + coord_fixed(ratio=0.15))
```

```
## Warning: Non Lab interpolation is deprecated
```

![](top_100_heatmap_and_indicative_species_analyze_files/figure-html/unnamed-chunk-5-1.png) 


