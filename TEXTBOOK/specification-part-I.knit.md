---
title: "Specification Part I"
---






# OUTLIERS




## Anscombe's Quartet Data


```r
data( anscombe )

m1 <- lm( y1 ~ x1, data=anscombe )
m2 <- lm( y2 ~ x2, data=anscombe )
m3 <- lm( y3 ~ x3, data=anscombe )
m4 <- lm( y4 ~ x4, data=anscombe )


par(mfrow = c(2, 2), mar = 0.1+c(4,4,1,1), oma =  c(0, 0, 2, 0))

plot( y1 ~ x1, data=anscombe,
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 13))
abline( m1, col = "blue")

plot( y2 ~ x2, data=anscombe,
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 13))
abline( m2, col = "blue")

plot( y3 ~ x3, data=anscombe,
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 13))
abline( m3, col = "blue")

plot( y4 ~ x4, data=anscombe,
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 13))
abline( m4, col = "blue")

mtext("Anscombe's 4 Regression data sets", outer = TRUE, cex = 1.5)
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-1-1.png" width="960" />


![](anscombes.png)


```r
# {r, results='asis'}

stargazer( m1, m2, m3, m4, type="html", digits=2,
           omit.stat = c("f","ser") )
```



## Model Fit Diagnostics

There are a bunch of diagnostics that we can run to examine whether we should worry about specification bias.


```r
par( mfrow=c(2,2), oma=c(2,0,0,0) )

# plot( m1 )
# mtext( "Model 1", side=1, outer = TRUE, cex = 1.5)

plot( m3 )
mtext("Model 3", size=1, outer = TRUE, cex = 1.5)
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-3-1.png" width="960" />


## Residual Analysis

The first step in outlier analysis is finding points that are the furthest away from the regression line. We can simply look at the residuals.



```r
plot( m3$residuals, type="h", main="Residual Analysis", bty="n", ylab="Y - Yhat" )
points( m3$residuals, pch=19 )
text( 1:11, m3$residuals, 1:11, pos=4, cex=0.8 )
abline( h=0, col="gray")
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-4-1.png" width="960" />



The problem with this approach is that not all outliers impact the regression in the same way. Outliers near the mean of X and Y will tug the regression line up or down slightly, so they impact the intercept only. Outliers near the lower and upper range of X will tilt the regression line, impacting both the slope and the intercept.


```r
par( mfrow=c(2,2))

x <- 1:11
y <- 1:11

mm1 <- lm( y ~ x )

plot( x, y, bty="n", pch=19, col="darkgray", cex=2, ylim=c(-5,25), xlim=c(0,13) )
abline( mm1, col="red" )

y[6] <- y[6]+10
mm2 <- lm( y ~ x )

plot( x, y, bty="n", pch=19, col="darkgray", cex=2, ylim=c(-5,25), xlim=c(0,13) )
abline( mm2, col="red" )

y <- 1:11
y[1] <- y[1] + 10
mm3 <- lm( y ~ x )

plot( x, y, bty="n", pch=19, col="darkgray", cex=2, ylim=c(-5,25), xlim=c(0,13) )
abline( mm3, col="red" )

y <- 1:11
y[11] <- y[11] + 10
mm4 <- lm( y ~ x )

plot( x, y, bty="n", pch=19, col="darkgray", cex=2, ylim=c(-5,25), xlim=c(0,13) )
abline( mm4, col="red" )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-5-1.png" width="960" />



```r
stargazer( mm1, mm2, mm3, mm4, type="html", digits=2,
           omit.stat = c("f","ser") )
```


<table style="text-align:center"><tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="4"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="4" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="4">y</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td><td>(4)</td></tr>
<tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">x</td><td>1.00<sup>***</sup></td><td>1.00<sup>***</sup></td><td>0.55<sup>*</sup></td><td>1.45<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.00)</td><td>(0.30)</td><td>(0.26)</td><td>(0.26)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>0.00<sup>***</sup></td><td>0.91</td><td>3.64<sup>*</sup></td><td>-1.82</td></tr>
<tr><td style="text-align:left"></td><td>(0.00)</td><td>(2.06)</td><td>(1.78)</td><td>(1.78)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td></td></tr>
<tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>11</td><td>11</td><td>11</td><td>11</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>1.00</td><td>0.55</td><td>0.32</td><td>0.77</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>1.00</td><td>0.50</td><td>0.25</td><td>0.75</td></tr>
<tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="4" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>


## Cook's Distance

Let's focus on one common measure one used to identify outliers that are having a large impact on our regression model. Cook's distance focuses not on distance from the regression line, but instead is a measure of how much leverage each point will have on the slope of the model.


```r
cooks.distance( m3 ) %>% pander
```


--------------------------------------------------------------------------------
    1         2         3        4          5        6          7          8    
--------- ---------- ------- ---------- --------- -------- ----------- ---------
 0.01176   0.002141   1.393   0.005473   0.02598   0.3006   0.0005176   0.03382 
--------------------------------------------------------------------------------

Table: Table continues below

 
--------------------------------
    9         10          11    
--------- ----------- ----------
 0.05954   0.0003546   0.006948 
--------------------------------

```r
par( mfrow=c(1,2) )

plot( y3 ~ x3, data=anscombe,
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 13))
abline( m3, col = "blue")
text( anscombe$x3, anscombe$y3, 1:11, pos=4 )

plot( cooks.distance( m3 ), pch=19, bty="n", ylab="Cook's Distance" )
text( 1:11, cooks.distance( m3 ), 1:11, pos=4 )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-7-1.png" width="960" />






Dealing with outliers can be tricky because once you start changing your data you are going down a slippery slope toward manufacturing results. In the case that the outlier is a data entry error you can fix it or eliminate it without hesitation. But if the data point is accurate and it has a large impact on your outcome then you need to take care to explain and justify your actions. If you delete outliers then report results before and after you have altered the data for the sake of transparency.


There are several rules of thumb on how to use Cook's distance to identify outliers:

* A general rule of thumb is that observations with a Cook's D of more than 3 times the mean distance is a possible outlier.
* An alternative interpretation is to investigate any point over 4/n, where n is the number of observations.
* Other authors suggest that any "large" Di should be investigated. How large is "too large"? The consensus seems to be that a Di value of more that 1 indicates an influential value, but you may want to look at values above 0.5.
* An alternative (but slightly more technical) way to interpret Di is to find the potential outlier's percentile value using the F-distribution. A percentile of over 50 indicates a highly influential point.


```r
par( mfrow=c(1,2) )

plot( y3 ~ x3, data=anscombe, main="Original Data",
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 15), ylim = c(3, 13))
abline( m3, col = "blue")


anscombe$x3[3] <- NA
anscombe$y3[3] <- NA

m3.2 <- lm( y3 ~ x3, data=anscombe )

plot( y3 ~ x3, data=anscombe, main="After Outlier Removal",
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 15), ylim = c(3, 13))
abline( m3.2, col = "blue")
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-8-1.png" width="960" />



```r
stargazer( m3, m3.2, type="html", digits=2,
           omit.stat = c("ser") )
```


<table style="text-align:center"><tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="2"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="2" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="2">y3</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td></tr>
<tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">x3</td><td>0.50<sup>***</sup></td><td>0.35<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td><td>(0.0003)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>3.00<sup>**</sup></td><td>4.01<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.12)</td><td>(0.003)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td></tr>
<tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>11</td><td>10</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.67</td><td>1.00</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.63</td><td>1.00</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>17.97<sup>***</sup> (df = 1; 9)</td><td>1,160,688.00<sup>***</sup> (df = 1; 8)</td></tr>
<tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="2" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>



# NONLINEAR RELATIONSHIPS

How do we address the situation when our relationships are nonlinear? This is important in public policy and program evaluation because of the economic phenomenon of diminishing marginal returns. If we have no resources or program services then providing a little big might have a large impact on the outcome. If we already have lots of resources and access then adding a little more might not have as much of an impact.

Let's now consider the second case in the quartet. 


```r
plot( y2 ~ x2, data=anscombe,
     col = "red", pch = 21, bg = "orange", cex = 3, 
     xlim = c(3, 15), ylim = c(3, 13), bty="n")
abline( m2, col = "blue")
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-10-1.png" width="960" />


We can see that the linear model does not adequately represent the true relationship. To get a better model we must introduce quadratic terms to allow for non-linear fit.

**Linear:** $Y = b_{0} + b_{1}X1 + e$

**Nonlinear:**  $Y = b_{0} + b_{1}X1 + b_{2}X1^2 + e$


```r
x <- anscombe$x2
y <- anscombe$y2

x_squared <- x^2

df <- data.frame( Y=y, X=x, X_squared=x_squared )
row.names(df) <- NULL
df %>% pander
```


-----------------------
  Y     X    X_squared 
------ ---- -----------
 9.14   10      100    

 8.14   8       64     

 8.74   13      169    

 8.77   9       81     

 9.26   11      121    

 8.1    14      196    

 6.13   6       36     

 3.1    4       16     

 9.13   12      144    

 7.26   7       49     

 4.74   5       25     
-----------------------


### Linear and Quadratic Models


```r
quad1 <- lm( y ~ x )
quad2 <- lm( y ~ x + x_squared )

stargazer( quad1, quad2, type="html", digits=2,
           omit.stat = c("ser","f") )
```


<table style="text-align:center"><tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="2"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="2" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="2">y</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td></tr>
<tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">x</td><td>0.50<sup>***</sup></td><td>2.78<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.12)</td><td>(0.001)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td></tr>
<tr><td style="text-align:left">x_squared</td><td></td><td>-0.13<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.0001)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>3.00<sup>**</sup></td><td>-6.00<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.13)</td><td>(0.004)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td></tr>
<tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>11</td><td>11</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.67</td><td>1.00</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.63</td><td>1.00</td></tr>
<tr><td colspan="3" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="2" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>


```r
par( mfrow=c(1,2) )

plot( y2 ~ x2, data=anscombe, main="Linear Fit",
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 10), bty="n")
abline( m2, col = "blue")

plot( y2 ~ x2, data=anscombe, main="Quadratic Fit",
     col = "red", pch = 21, bg = "orange", cex = 1.2, 
     xlim = c(3, 19), ylim = c(3, 10), bty="n")


lines( x[order(x)], quad2$fitted.values[order(x)], col = "blue")
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-13-1.png" width="960" />


## Interpretation of Program Effects

The main challenge that nonlinear models present is the interpretation of effects.

Up until now we have used the slope of X to determine the impact of our program. If the slope is negative five, for exmaple, we infer that for each additional student we add to the classroom we see a drop in the average standardized test score in the classroom of five points. That doesn't matter if we have 10 students and we are adding an 11th, or if we have 40 students and we are adding a 41st (which is likely an unrealistic scenario).


```r
coefficients( quad2 ) %>% round(4)
```

In the nonlinear case, however, the marginal impact of an additional student will be different depending upon the initial class size. Consider Anscombe's second case above. 

$Y = b_{0} + b_{1}X1 + b_{2}X1^2$

Where

$b_{0} = -6.00$ 

$b_1 =  2.78$ 

$b_2 = -0.13$ 

Now let's consider what happens when we add an additional unit of the treatment in three cases:




#### Case 1: X=6


```r
x_pos <- 6

(2.7808)*(x_pos+1) + (-0.1267)*((x_pos+1)^2) -5.9957  - (  (2.7808)*(x_pos) + (-0.1267)*(x_pos^2) -5.9957 )
```



Program Effect of one additional unit of X:  $(\hat{Y} | X=7)  - (\hat{Y} | X=6 )$

$[ (2.78)(7) + (-0.13)(7^2) -6 ]  -  [ (2.78)(6) + (-0.13)(6^2) -6 ]  = 1.13$

<br>



#### Case 1: X=10


```r
x_pos <- 10

(2.7808)*(x_pos+1) + (-0.1267)*((x_pos+1)^2) -5.9957  - (  (2.7808)*(x_pos) + (-0.1267)*(x_pos^2) -5.9957 )
```

$(\hat{Y} | X=11)  - (\hat{Y} | X=10 )$

$[ (2.78)(11) + (-0.13)(11^2) -6 ]  -  [ (2.78)(10) + (-0.13)(10^2) -6 ]  = 0.12$

<br>



#### Case 1: X=13


```r
x_pos <- 13

(2.7808)*(x_pos+1) + (-0.1267)*((x_pos+1)^2) -5.9957  - (  (2.7808)*(x_pos) + (-0.1267)*(x_pos^2) -5.9957 )
```

Program Effect of one additional unit of X:  $(\hat{Y} | X=14)  - (\hat{Y} | X=13 )$

$[ (2.78)(14) + (-0.13)(14^2) -6 ]  -  [ (2.78)(13) + (-0.13)(13^2) -6 ]  = -0.64$

<br>

#### Or More Generally


```r
y.hat <- quad2$fitted.values[order(x)]

dt <- data.frame(  X=4:13, MarginEffect=round(diff( y.hat ),2) ) 
row.names(dt) <- NULL
dt %>% pander
```


-------------------
 X    MarginEffect 
---- --------------
 4        1.64     

 5        1.39     

 6        1.13     

 7        0.88     

 8        0.63     

 9        0.37     

 10       0.12     

 11      -0.13     

 12      -0.39     

 13      -0.64     
-------------------

```r
# diff( y[order(x)] )
```

<br>

#### Effects of Adding Unit or Subtracting are Nonsymmetrical

Also note that if you are in a classroom with ten students, then the effect of adding a student will not be the same as the effect of removing a student. 

$ABS( (\hat{Y} | X=5)  - (\hat{Y} | X=4 ) )$  does not equal  $ABS( (\hat{Y} | X=4)  - (\hat{Y} | X=5 ) )$

<br>


## Reporting Program Effects

The typical way to address this issue in a program evaluation report is to select several representative cases and report the program effects for each. For example, you might select the typical (median) student, a small classroom (1st quartile of X), and a large classroom (3rd quartile of X). 

It is discouraged to interpret model effects at the min and max values of X because they may be outliers and the model predictions will be less robust.



<br>
<br>


# REGRESSIONS WITH GROUPS

Let's consider a basic model of wages as a function of experience. 

<img src="specification-part-I_files/figure-html/unnamed-chunk-19-1.png" width="960" />





Our analysis gets much more interesting when we can include groups. In this instance we will include a dummy variable (one that takes on values of 1 or 0) for gender.


```r
palette( c("steelblue","darkorange3") )
plot( years, wages, pch=19, col=(female+1), cex=1.5, bty="n", 
      main="Wages by Years of Experience", xlab="Years of Experience" )
abline( lm(wages~years), col="black", lwd=2 )

points( 25, 1500, col="steelblue", pch=19, cex=2 )
text( 25, 1500, "Male", col="steelblue", pos=4 )
points( 25, 7000, col="darkorange3", pch=19, cex=2 )
text( 25, 7000, "Female", col="darkorange3", pos=4 )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-20-1.png" width="960" />


When we have dummy variables, our data looks something like this where Female=1 represents women, and Female=0 represents men in the data.


```r
data.frame( Wages=wages, Years=years, Female=female )[1:10,] %>% pander
```


------------------------
 Wages   Years   Female 
------- ------- --------
 27022    22       1    

 15778     1       1    

 26577    20       0    

 12876     0       1    

 12208     4       0    

 6714      2       0    

 42489    27       0    

 20049     3       1    

 21598     3       1    

 39492    19       1    
------------------------

When we have categorical variables we can now look at many interesting nuances in relationships between the groups. Specifically, in the models reported below we can ask more interesting questions:



#### Model 1: Do men and women earn different wages, on average?

**Model:**  $Wages = b_0 + b_1*Female$

**Test:** If b0 is significant, Men's wages are different than zero. If b1 is significant, Women's wages are different than Men's.

*Note:* This is an unconditional average, so it might be explained by other factors like differences in experience between men and women.



#### Model 2: What is the wage gain related to an extra year of experience?

**Model:**  $Wages = b_0 + b_1*Years$

**Test:** If b1 is significant then it is different than zero, experience does impact wages.


#### Model 3: Do men and women have different initial wages at the start of their careers?

**Model:**  $Wages = b_0 + b_1*Years + b_2*Female$

**Test:** If b2 is significant then the Female intercept (b0+b2) is different than the Male intercept (b0).




#### Model 4: Are the gains in wages related to experience the same for men and women?

**Model:**  $Wages = b_0 + b_1*Years + b_2*Female + b_3*Years*Female$

**Test:** If b3 is significant then the slope for Women (b1+b3) is different than the slope for Men (b1).

*Note:* If b3 is not significant it is better to use the model with one slope for both groups.

<br>


```r
m.01 <- lm( wages ~ female )
m.02 <- lm( wages ~ years )
m.03 <- lm( wages ~ years + female )
m.04 <- lm( wages ~ years + female + years*female )


stargazer( m.01, m.02, m.03, m.04, type="html", digits=0,
           intercept.bottom = FALSE,
           omit.stat = c("ser","f","rsq","adj.rsq") )
```


<table style="text-align:center"><tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="4"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="4" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="4">wages</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td><td>(4)</td></tr>
<tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Constant</td><td>23,007<sup>***</sup></td><td>10,187<sup>***</sup></td><td>6,701<sup>***</sup></td><td>7,906<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(500)</td><td>(387)</td><td>(355)</td><td>(433)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">female</td><td>7,638<sup>***</sup></td><td></td><td>7,154<sup>***</sup></td><td>4,588<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(701)</td><td></td><td>(324)</td><td>(629)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">years:female</td><td></td><td></td><td></td><td>170<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td>(36)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">years</td><td></td><td>1,109<sup>***</sup></td><td>1,099<sup>***</sup></td><td>1,018<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td>(22)</td><td>(18)</td><td>(25)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td><td></td></tr>
<tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>1,000</td><td>1,000</td><td>1,000</td><td>1,000</td></tr>
<tr><td colspan="5" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="4" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>





```r
palette( c( adjustcolor( "steelblue", alpha.f=0.3), adjustcolor( "darkorange3", alpha.f=0.3)  ) )
plot( female+rnorm(1000,0,0.08), wages, col=female+1, pch=19, bty="n",
      xlab="", xaxt="n", cex=2, main="Hypothesis 1: Difference in Wages Between Men and Women?" )
axis( side=1, at=c(0,1), c("Male","Female") )
abline( h=coefficients( m.01 )[1], col="steelblue", lwd=2 )
abline( h=sum(coefficients( m.01 )), col="darkorange3", lwd=2 )

text( 0.5, coefficients( m.01 )[1], "b0", pos=3, col="steelblue" )
text( 0.5, sum(coefficients( m.01 )), "b0 + b1", pos=3, col="darkorange3" )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-23-1.png" width="960" />






```r
palette( c( adjustcolor( "steelblue", alpha.f=0.3), adjustcolor( "darkorange3", alpha.f=0.3)  ) )
plot( years+rnorm(1000,0,0.25), wages, pch=19, col=(female+1), cex=1.5, bty="n", 
      main="Hypothesis 2: Relationship Between Experience and Wages?", xlab="Years of Experience" )
abline( lm(wages~years), col="black", lwd=2 )

points( 25, 1500, col="steelblue", pch=19, cex=2 )
text( 25, 1500, "Male", col="steelblue", pos=4 )
points( 25, 7000, col="darkorange3", pch=19, cex=2 )
text( 25, 7000, "Female", col="darkorange3", pos=4 )
text( 5, 50000, "Wages = b0 + b1*Years", cex=1, col="gray10" )
text( 5, 45000, "Slope = b1", cex=1, col="gray10" )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-24-1.png" width="960" />





```r
b0 <- coefficients(m.03)[1]
b1 <- coefficients(m.03)[2]
b2 <- coefficients(m.03)[3]

palette( c( adjustcolor( "steelblue", alpha.f=0.3), adjustcolor( "darkorange3", alpha.f=0.3)  ) )
plot( years+rnorm(1000,0,0.25), wages, pch=19, col=(female+1), cex=1.5, bty="n", 
      main="Hypothesis 3: Difference Wages for Women Conditional on Experience?", xlab="Years of Experience", xlim=c(-5,30) )
abline( a=b0, b=b1, col="steelblue", lwd=2 )
abline( a=b0+b2, b=b1, col="darkorange3", lwd=2 )

points( 25, 1500, col="steelblue", pch=19, cex=2 )
text( 25, 1500, "Male", col="steelblue", pos=4 )
points( 25, 7000, col="darkorange3", pch=19, cex=2 )
text( 25, 7000, "Female", col="darkorange3", pos=4 )
text( 5, 50000, "Wages = b0 + b1*Years + b2*Female", cex=1, col="gray10" )
text( 5, 45000, "Slope = b1", cex=1, col="gray10" )

text( -3.5, 10000, "b0 + b2", col="darkorange3", pos=3 )
text( -3.5, 2450, "b0", col="steelblue", pos=1 )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-25-1.png" width="960" />





```r
b0 <- coefficients(m.04)[1]
b1 <- coefficients(m.04)[2]
b2 <- coefficients(m.04)[3]
b3 <- coefficients(m.04)[4]


palette( c( adjustcolor( "steelblue", alpha.f=0.3), adjustcolor( "darkorange3", alpha.f=0.3)  ) )
plot( years+rnorm(1000,0,0.25), wages, pch=19, col=(female+1), cex=1.5, bty="n", 
      main="Hypothesis 4: Difference Rates of Raises for Women?", xlab="Years of Experience", xlim=c(0,35) )
abline( a=b0, b=b1, col="steelblue", lwd=2 )
abline( a=b0+b2, b=b1+b3, col="darkorange3", lwd=2 )

points( 25, 1500, col="steelblue", pch=19, cex=2 )
text( 25, 1500, "Male", col="steelblue", pos=4 )
points( 25, 7000, col="darkorange3", pch=19, cex=2 )
text( 25, 7000, "Female", col="darkorange3", pos=4 )
text( 10, 55000, "Wages = b0 + b1*Years + b2*Female + b3*Years*Female", cex=1, col="gray10" )

text( 33, 55000, "b1 + b3", col="darkorange3", pos=3 )
text( 33, 40000, "b1", col="steelblue", pos=1 )
```

<img src="specification-part-I_files/figure-html/unnamed-chunk-26-1.png" width="960" />
















```css
p {
color: black;
margin: 0 0 20px 0;
}

td {
    padding: 3px 10px 3px 10px;
    text-align: center;
}

table
{ 
    margin-left: auto;
    margin-right: auto;
    margin-top:80px;
    margin-bottom:100px;
}

h1, h2{
  margin-top:100px;
  margin-bottom:20px;
}

H5{
    text-align: center;
    color: gray;
    font-size:0.8em;
}

img {
    max-width: 90%;
    display: block;
    margin-right: auto;
    margin-left: auto;
    margin-top:30px;
    margin-bottom:20px;
}

pre {
  overflow-x: auto;
}

pre code {
   display: block; 
   padding: 0.5em;
   margin-bottom:20px;
}

code {
  font-size: 92%;
  border: 10px solid #F8F8F8;
  margin-bottom: 2px;
}

code[class] {
  background-color: #F8F8F8;
}

```


<style type="text/css">
p {
color: black;
margin: 0 0 20px 0;
}

td {
    padding: 3px 10px 3px 10px;
    text-align: center;
}

table
{ 
    margin-left: auto;
    margin-right: auto;
    margin-top:80px;
    margin-bottom:100px;
}

h1, h2{
  margin-top:100px;
  margin-bottom:20px;
}

H5{
    text-align: center;
    color: gray;
    font-size:0.8em;
}

img {
    max-width: 90%;
    display: block;
    margin-right: auto;
    margin-left: auto;
    margin-top:30px;
    margin-bottom:20px;
}

pre {
  overflow-x: auto;
}

pre code {
   display: block; 
   padding: 0.5em;
   margin-bottom:20px;
}

code {
  font-size: 92%;
  border: 10px solid #F8F8F8;
  margin-bottom: 2px;
}

code[class] {
  background-color: #F8F8F8;
}

</style>
