# Ecommerce-basket-purchase-inventory-stock-Analysis
We have a huge amount of data set of customer can buy product form ecommerce websites so have to analysis this data and get some useful information means what the customer fulfilments their needs , or as catch the customer interaction, what they buy next product (oil ,breed , butter , jam, other )this is the problem statement we can solve on this project.
# PURPOSE
__________________________________________________________________________
I have a data set consist multiple customer purchase items on by way of transaction data, so in these recent years, the transaction data have been prevalent as research object in means of discovering new information so , we use this information, as well as consume the purchased inventory and services including in the customer factors which can give a rise to their decisions whether and even-if, to purchase this product. Every customer has different needs and tendency as well as have different behaviors of fulfilling on those things. So, in the event of different behaviors to fulfill their needs, they still share some equality, one of them is want to maximize their satisfactions in consuming a necessary product or service. Of that consumption activity, that can be deduce as to the behavior pattern or habit that the customers do in fulfilling their needs and desires so That behavior can be identified consumer needs (supermarket), so we have to design Association Algorithm they can resolve my problem statement by analysis basket (cart) inventory stocks .

association rules with R
================

Market Basket Analysis with R
-----------------------------

``` r
suppressMessages(library(arules))
data("Groceries")
summary(Groceries)
```

    ## transactions as itemMatrix in sparse format with
    ##  9835 rows (elements/itemsets/transactions) and
    ##  169 columns (items) and a density of 0.02609146 
    ## 
    ## most frequent items:
    ##       whole milk other vegetables       rolls/buns             soda 
    ##             2513             1903             1809             1715 
    ##           yogurt          (Other) 
    ##             1372            34055 
    ## 
    ## element (itemset/transaction) length distribution:
    ## sizes
    ##    1    2    3    4    5    6    7    8    9   10   11   12   13   14   15 
    ## 2159 1643 1299 1005  855  645  545  438  350  246  182  117   78   77   55 
    ##   16   17   18   19   20   21   22   23   24   26   27   28   29   32 
    ##   46   29   14   14    9   11    4    6    1    1    1    1    3    1 
    ## 
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   1.000   2.000   3.000   4.409   6.000  32.000 
    ## 
    ## includes extended item information - examples:
    ##        labels  level2           level1
    ## 1 frankfurter sausage meat and sausage
    ## 2     sausage sausage meat and sausage
    ## 3  liver loaf sausage meat and sausage

``` r
ar <- apriori(Groceries,
              parameter = list(
                support = 0.001,
                confidence = 0.6,
                maxlen = 10
              ),
              control = list(verbose = FALSE))
inspect(sample(ar, 6))
```

    ##     lhs                     rhs                    support confidence     lift
    ## [1] {frankfurter,                                                             
    ##      yogurt,                                                                  
    ##      sliced cheese}      => {whole milk}       0.001016777  0.8333333 3.261374
    ## [2] {tropical fruit,                                                          
    ##      curd,                                                                    
    ##      yogurt,                                                                  
    ##      whipped/sour cream} => {whole milk}       0.001321810  0.7647059 2.992790
    ## [3] {sausage,                                                                 
    ##      curd,                                                                    
    ##      pastry}             => {other vegetables} 0.001016777  0.7142857 3.691540
    ## [4] {chicken,                                                                 
    ##      citrus fruit,                                                            
    ##      whipped/sour cream} => {yogurt}           0.001016777  0.6666667 4.778912
    ## [5] {whole milk,                                                              
    ##      butter,                                                                  
    ##      waffles}            => {other vegetables} 0.001423488  0.7777778 4.019677
    ## [6] {pip fruit,                                                               
    ##      root vegetables,                                                         
    ##      yogurt}             => {whole milk}       0.003558719  0.6730769 2.634187

``` r
subs <- subset(ar,
               subset = lhs %in% c("meat", "pasta"))
subs <- sort(subs, by = "confidence")
subs
```

    ## set of 16 rules

``` r
redundant <- which(colSums(is.subset(subs, subs)) > 1)
length(redundant)
```

    ## [1] 2

``` r
subs <- subs[-redundant]
              
recs <- unique(rhs(sort(subs, by = "lift")))
inspect(recs)
```

    ##     items             
    ## [1] {other vegetables}
    ## [2] {whole milk}

``` r
suppressMessages(library(arulesViz))
plot(subs, method = "graph")
```

![](ar_files/figure-markdown_github/subs-1.png)

``` r
plot(subs)
```

![](ar_files/figure-markdown_github/subs-2.png)

``` r
plot(subs, method = "grouped")
```

![](ar_files/figure-markdown_github/subs-3.png)

``` r
library(shiny)
runApp("App.R")
```
 
![](ar_files/figure-markdown_github/shinyApp.png)

