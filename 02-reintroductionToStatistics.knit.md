---
output:
  pdf_document: 
    keep_tex: yes
  html_document: default
header-includes:
- \usepackage{amsmath}
- \usepackage{color}
---

#(R)e-Introduction to statistics {#chapter2}






The previous material served to get us started in R and to get a quick review of same basic
descriptive statistics. Now we will begin to engage some new material and
exploit the power of R to do some statistical inference. Because inference is
one of the hardest topics to master in statistics, we will also review some
basic terminology that is required to move forward in learning more
sophisticated statistical methods. To keep this "review" as short as possible, 
we will not consider every situation you learned in introductory statistics and
instead focus exclusively on the situation where we have a quantitative
response variable measured on two groups, adding a new graphic called a "bean
plot" to help us see the differences in the observations in the groups. 

##Histograms, boxplots, and density curves {#section2-1}

Part of learning statistics is learning to correctly use the terminology, some of which is used colloquially
differently than it is used in formal statistical settings. The most commonly
"misused" term is ***data***. In statistical parlance, we want to note the plurality of
data. Specifically, ***datum*** is a single measurement, possibly on multiple random
variables, and so it is appropriate to say that "**a datum is...**". 
Once we move to discussing data, we are now referring to more than one 
observation, again on one, or possibly more than one, random variable, and 
so we need to use "**data are...**" when talking about our observations. We want 
to distinguish our use of the term "data" from its more
colloquial^[You will more typically hear "data is" but that more often refers to
information, sometimes even statistical summaries of data sets, than to
observations collected as part of a study, suggesting the confusion of this
term in the general public. We will explore a data set in Chapter 4 related to
perceptions of this issue collected by researchers at http://fivethirtyeight.com/.]
usage that often involves treating it as singular. In a statistical setting
"data" refers to measurements of our cases or units. When we summarize the
results of a study (say providing the mean and SD), that information is not
"data". We used our data to generate that information. Sometimes we also use
the term "data set" to refer to all our observations and this is a singular
term to refer to the group of observations and this makes it really easy to
make mistakes on the usage of this term. 

It is also really important to note that ***variables*** have to vary --
if you measure the sex of your subjects but are only measuring females, then you do
not have a "variable". You may not know if you have real variability in a
"variable" until you explore the results you obtained. 

The last, but probably most important, aspect of data is the context 
of the measurement. The "who, what, when, and where" of the collection 
of the observations is critical to the
sort of conclusions we can make based on the results. The information on the
study design provides information required to assess the scope of inference of
the study. Generally, remember to think about the research questions the
researchers were trying to answer and whether their study actually would answer
those questions. There are no formulas to help us sort some of these things
out, just critical thinking about the context of the measurements. 

To make this concrete, consider the data collected from a study 
[@Plaster1989] to investigate whether
perceived physical attractiveness had an impact on the sentences or perceived
seriousness of a crime that male jurors might give to female defendants. The
researchers showed the participants in the study (men who volunteered from a
prison) pictures of one of three young women. Each picture had previously been
decided to be either beautiful, average, or unattractive by the researchers. 
Each "juror" was randomly assigned to one of three levels of this factor (which
is a categorical predictor or explanatory variable) and then each rated their
picture on a variety of traits such as how warm or sincere the woman appeared. 
Finally, they were told the women had committed a crime (also randomly assigned
to either be told she committed a burglary or a swindle) and were asked to rate
the seriousness of the crime and provide a suggested length of sentence. We
will bypass some aspects of their research and just focus on differences in the
sentence suggested among the three pictures. To get a sense of these data, 
let's consider the first and last parts of the data set:

(ref:tab2-1) First 5 and last 6 rows of the MockJury data set

\begin{table}

\caption{(\#tab:Table2-1)(ref:tab2-1)}
\centering
\begin{tabular}[t]{l|l|l|l|l|l|l}
\hline
Subject & Attr & Crime & Years & Serious & Independent & Sincere\\
\hline
1 & Beautiful & Burglary & 10 & 8 & 9 & 8\\
\hline
2 & Beautiful & Burglary & 3 & 8 & 9 & 3\\
\hline
3 & Beautiful & Burglary & 5 & 5 & 6 & 3\\
\hline
4 & Beautiful & Burglary & 1 & 3 & 9 & 8\\
\hline
5 & Beautiful & Burglary & 7 & 9 & 5 & 1\\
\hline
\$\textbackslash{}vdots\$ & \$\textbackslash{}vdots\$ & \$\textbackslash{}vdots\$ & \$\textbackslash{}vdots\$ & \$\textbackslash{}vdots\$ & \$\textbackslash{}vdots\$ & \$\textbackslash{}vdots\$\\
\hline
108 & Average & Swindle & 3 & 3 & 5 & 4\\
\hline
109 & Average & Swindle & 3 & 2 & 9 & 9\\
\hline
110 & Average & Swindle & 2 & 1 & 8 & 8\\
\hline
111 & Average & Swindle & 7 & 4 & 9 & 1\\
\hline
112 & Average & Swindle & 6 & 3 & 5 & 2\\
\hline
113 & Average & Swindle & 12 & 9 & 9 & 1\\
\hline
114 & Average & Swindle & 8 & 8 & 1 & 5\\
\hline
\end{tabular}
\end{table}

When working with data, we should always start with 
summarizing the sample size. We will use ***n*** for the 
number of subjects in the sample and denote the population size (if
available) with ***N***. Here, the sample size is ***n=114***. In
this situation, we do not have a random sample from a population 
(these were volunteers from the population of prisoners at the
particular prison) so we cannot make inferences to a larger group. 
But we can assess whether there is a ***causal effect***^[As noted previously, we reserve the term "effect" for situations where random assignment allows us to consider causality as the reason for the differences in the response variable among levels of the explanatory variable, but this is only the case if we find evidence against the null hypothesis of no difference
in the groups.]: if sufficient evidence is found to conclude that there is some difference in
the responses across the treated groups, we can attribute those differences to
the treatments applied, since the groups should be same otherwise due to the
pictures being randomly assigned to the "jurors". The story of the data set --
that it was collected on prisoners -- becomes pretty important in thinking about
the ramifications of any results. Are male prisoners different from the
population of college males or all residents of a state such as Montana? If so, 
then we should not assume that the detected differences, if detected, would
also exist in some other group of male subjects. The lack of a random sample
makes it impossible to assume that this set of prisoners might be like other
prisoners. So there are definite limitations to the inferences in the following
results. But it is still interesting to see if the pictures caused a difference
in the suggested mean sentences, even though the inferences are limited to this
group of prisoners. If this had been an observational study (suppose that the
prisoners could select one of the three pictures), then we would have to avoid
any of the "causal" language that we can consider here because the pictures
were not randomly assigned to the subjects. Without random assignment, the
explanatory variable of picture choice could be ***confounded*** with another characteristic of prisoners that was related to which picture they selected and
the rating they provided. Confounding is not the only reason to avoid causal
statements with non-random assignment but the inability to separate the effect
of other variables (measured or unmeasured) from the differences we are
observing means that our inferences in these situations need to be carefully
stated. 

Instead of loading this data set into R using the "Import Dataset" 
functionality, we can load an R
package that contains the data, making for easy access to this data set. The
package called ``heplots`` [@R-heplots] contains a data set called MockJury 
that contains the results of the study. We also rely the R package called 
``mosaic`` [@R-mosaic] that was introduced previously. First (but only once), 
you need to install both packages, which can
be done either using the Packages tab in the lower right panel of R-studio or
using the ``install.packages`` function with quotes around the package name:

```r
> install. packages("heplots")
```

After making sure that both packages are installed, we use the ``require`` 
function around the package name (no quotes now!) to load the package, something that
you need to do any time you want to use features of a package. 


```r
require(heplots)
require(mosaic)
```


There will be some results of the loading process that may discuss loading other
required packages. If the output says that it needs a package that is
unavailable, then follow the same process noted above to install that package
as well. 

To load the data set that is available in an active package, we use the 
``data`` function. 


```r
data(MockJury)
```

Now there will be a data.frame called ``MockJury`` available for us to 
analyze and some information about it in the Environment tab. Again, we 
can find out more about the data set in a couple of ways. First, 
we can use the ``View`` function to provide a spreadsheet type of display 
in the upper left panel. Second, we can use the ``head`` and ``tail`` 
functions to print out the beginning and end of the data set. Because 
there are so many variables, it may wrap around to show all the columns. 


```r
View(MockJury)
head(MockJury)
```

```
##        Attr    Crime Years Serious exciting calm independent sincere warm
## 1 Beautiful Burglary    10       8        6    9           9       8    5
## 2 Beautiful Burglary     3       8        9    5           9       3    5
## 3 Beautiful Burglary     5       5        3    4           6       3    6
## 4 Beautiful Burglary     1       3        3    6           9       8    8
## 5 Beautiful Burglary     7       9        1    1           5       1    8
## 6 Beautiful Burglary     7       9        1    5           7       5    8
##   phyattr sociable kind intelligent strong sophisticated happy ownPA
## 1       9        9    9           6      9             9     5     9
## 2       9        9    4           9      5             5     5     7
## 3       7        4    2           4      5             4     5     5
## 4       9        9    9           9      9             9     9     9
## 5       8        9    4           7      9             9     8     7
## 6       8        9    5           8      9             9     9     9
```

```r
tail(MockJury)
```

```
##        Attr   Crime Years Serious exciting calm independent sincere warm
## 109 Average Swindle     3       2        7    6           9       9    6
## 110 Average Swindle     2       1        8    8           8       8    8
## 111 Average Swindle     7       4        1    6           9       1    1
## 112 Average Swindle     6       3        5    3           5       2    4
## 113 Average Swindle    12       9        1    9           9       1    1
## 114 Average Swindle     8       8        1    9           1       5    1
##     phyattr sociable kind intelligent strong sophisticated happy ownPA
## 109       4        7    6           8      6             5     7     2
## 110       8        9    9           9      9             9     9     6
## 111       1        9    4           1      1             1     1     9
## 112       1        4    9           3      3             9     5     3
## 113       1        9    1           9      9             1     9     1
## 114       1        9    1           1      9             5     1     1
```

When data sets are loaded from packages, there is often extra documentation
available about the data set which can be accessed using the help function. In
this case, it will bring up a screen with information about the study and each
variable that was measured. 


```r
help(MockJury)
```

The help function is also useful with functions in R to help you understand options and, at the bottom of the help, 
see examples of using the function. 

With many variables in a data set, it is often useful to get some 
quick information about all of them; the ``summary`` function provides 
useful information whether the variables are categorical or 
quantitative and notes if any values were missing. 


```r
summary(MockJury)
```

```
##            Attr         Crime        Years           Serious     
##  Beautiful   :39   Burglary:59   Min.   : 1.000   Min.   :1.000  
##  Average     :38   Swindle :55   1st Qu.: 2.000   1st Qu.:3.000  
##  Unattractive:37                 Median : 3.000   Median :5.000  
##                                  Mean   : 4.693   Mean   :5.018  
##                                  3rd Qu.: 7.000   3rd Qu.:6.750  
##                                  Max.   :15.000   Max.   :9.000  
##     exciting          calm        independent       sincere     
##  Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000  
##  1st Qu.:3.000   1st Qu.:4.250   1st Qu.:5.000   1st Qu.:3.000  
##  Median :5.000   Median :6.500   Median :6.500   Median :5.000  
##  Mean   :4.658   Mean   :5.982   Mean   :6.132   Mean   :4.789  
##  3rd Qu.:6.000   3rd Qu.:8.000   3rd Qu.:8.000   3rd Qu.:7.000  
##  Max.   :9.000   Max.   :9.000   Max.   :9.000   Max.   :9.000  
##       warm         phyattr        sociable          kind      
##  Min.   :1.00   Min.   :1.00   Min.   :1.000   Min.   :1.000  
##  1st Qu.:2.00   1st Qu.:2.00   1st Qu.:5.000   1st Qu.:3.000  
##  Median :5.00   Median :5.00   Median :7.000   Median :5.000  
##  Mean   :4.57   Mean   :4.93   Mean   :6.132   Mean   :4.728  
##  3rd Qu.:7.00   3rd Qu.:8.00   3rd Qu.:8.000   3rd Qu.:7.000  
##  Max.   :9.00   Max.   :9.00   Max.   :9.000   Max.   :9.000  
##   intelligent        strong      sophisticated       happy      
##  Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000  
##  1st Qu.:4.000   1st Qu.:4.000   1st Qu.:3.250   1st Qu.:3.000  
##  Median :7.000   Median :6.000   Median :5.000   Median :5.000  
##  Mean   :6.096   Mean   :5.649   Mean   :5.061   Mean   :5.061  
##  3rd Qu.:8.750   3rd Qu.:7.000   3rd Qu.:7.000   3rd Qu.:7.000  
##  Max.   :9.000   Max.   :9.000   Max.   :9.000   Max.   :9.000  
##      ownPA      
##  Min.   :1.000  
##  1st Qu.:5.000  
##  Median :6.000  
##  Mean   :6.377  
##  3rd Qu.:9.000  
##  Max.   :9.000
```

If we take a few moments to explore the output we can discover some 
useful aspects of the data set. The output is organized by variable, 
providing summary information based on the type of
variable, either counts by category for categorical variables ``Attr`` 
and ``Crime`` mean for quantitative variables. If present, you would 
also get a count of missing values that are called "NAs" in R. For the 
first variable, called ``Attr`` in the data.frame and that we might 
we find counts of the number of subjects shown each picture: 37/114 viewed the 
"Unattractive" picture, 38 viewed "Average", and 39 viewed "Beautiful". 
We can also see that suggested prison sentences (data.frame variable 
``Years`` ) ranged from 1 year to 15 years with a median of 3 years. 
It seems that all the other variables except for *Crime* (type of crime 
that they were told the pictured woman committed) contained responses 
between 1 and 9 based on rating scales from 1 = low to 9 = high.

To accompany the numerical summaries, histograms, and boxplots can 
provide some initial information on the shape of the distribution of 
the responses for the Figure \@ref(fig:Figure2-1) contains the histogram 
and boxplot of Years, ignoring any information on which picture the 
"jurors" were shown. The calls to the two plotting functions are 
enhanced slightly to add better labels.

(ref:fig2-1) Histogram and boxplot of suggested sentences in years.


```r
par(mfrow=c(1,2))
hist(MockJury$Years, xlab="Years", labels=T, main="Histogram of Years")
boxplot(MockJury$Years, ylab="Years", main="Boxplot of Years")
```

![(\#fig:Figure2-1)(ref:fig2-1)](02-reintroductionToStatistics_files/figure-latex/Figure2-1-1.pdf) 

The distribution appears to have a strong right skew with three 
observations at 15 years flagged as potential
outliers. You can only tell that there are three observations and that they are
at 15 by looking at both plots -- the bar around 15 years in the histogram has a
count of three and the boxplot only shows a single point at 15 which is
actually three tied points at exactly 15 years plotted on top of each other (we
call this "overplotting"). These three observations really seem to be the upper
edge of the overall pattern of a strongly right skewed distribution, so even
though they are flagged in the boxplot, we likely would not want to remove them
from our data set. In real data sets, outliers are commonly encountered and the
first step is to verify that they were not errors in recording. The next step
is to study their impact on the statistical analyses performed, potentially
considering reporting results with and without the influential observation(s)
in the results. If the analysis is unaffected by the "unusual" observations, 
then it matters little whether they are dropped or not. If they do affect the
results, then reporting both versions of results allows the reader to judge the
impacts for themselves. It is important to remember that sometimes the outliers
are the most interesting part of the data set. 

Often when statisticians think of distributions, we think of the smooth underlying
shape that led to the data set that is being displayed in the histogram. 
Instead of binning up observations and making bars in the histogram, we can
estimate what is called a ***density curve *** as a smooth curve 
that represents the observed distribution of the responses. Density curves can
sometimes help us see features of the data sets more clearly. 

To understand the density curve, it is useful to initially see 
the histogram and density curve together. The density curve is scaled 
so that the total area^[If you've taken calculus, you will know that the 
curve is being constructed so that the integral from $-\infty$ to 
$\infty$ is 1. If you don't know calculus, think of a rectangle with area 
of 1 based on its height and width. These cover the same area but the top of the 
region wiggles.] under the curve is 1. To make a comparable histogram, the 
y-axis needs to be scaled so that the histogram is also on the "density" 
scale which makes the bar heights required so that the proportion of the 
total data set in each bar is represented by the area in each bar 
(remember that area is height times width). So the height depends on the 
width of the bars and the total area across all the bars has to be 1. In the 
``hist`` function, the ``freq=F`` to get density-scaled histogram bars. The 
density curve is added to the histogram using the R code of 
``lines(density())``, producing the result in Figure \@ref(fig:Figure2-2) with 
added modifications of options for ``lwd`` (line width) and ``col`` (color)
to make the plot more interesting. You can see how the density curve 
somewhat matches the histogram bars but deals with the bumps up and down 
and edges a little differently. We can pick out the strong right skew using 
either display and will rarely make both together. 

(ref:fig2-2) Histogram and density curve of Years data.



```r
hist(MockJury$Years,freq=F,xlab="Years",main="Histogram of Years")
lines(density(MockJury$Years),lwd=3,col="red")
```

![(\#fig:Figure2-2)(ref:fig2-2)](02-reintroductionToStatistics_files/figure-latex/Figure2-2-1.pdf) 

Histograms can be sensitive to the choice of the number of bars and 
even the cut-offs used to define the bins for a given number of bars.
Small changes in the definition of cut-offs for the bins can have 
noticeable impacts on the shapes observed but
this does not impact density curves. We are not going to tinker with the
default choices for bars in histogram as they are reasonably selected, but we
can add information on the original observations being included in each bar to
better understand the choices that ``hist`` is making. In the previous 
display, we can add what is called a ***rug*** to the plot, were a tick 
mark is made on the x-axis for each observation. Because the responses 
were provided as whole years (1, 2, 3, ..., 15), we need to use a graphical 
technique called ***jittering*** to add a little noise^[Jittering typically 
involves adding random variability to each observation that
is uniformly distributed in a range determined based on the spacing of the
function, the results will change. For more details, type ``help(jitter)`` 
in R.] to each observation so all the observations at each year value do not 
plot as a single line. In Figure \@ref(fig:Figure2-3), the added tick marks 
on the x-axis show the approximate locations of the original observations. 
We can see how there are 3 observations at 15 (all were 15 and the noise 
added makes it possible to see them all). The limitations of the 
histogram arise around the 10 year sentence
area where there are many responses at 10 years and just one at both 9 and 11 years, 
but the histogram bars sort of miss this that aspect of the data set. The
density curve did show a small bump at 10 years. Density curves are, however, 
not perfect and this one shows area for sentences less than 0 years which is
not possible here. 

(ref:fig2-3) Histogram with density curve and rug plot of the jittered responses. 


```r
hist(MockJury$Years, freq=F, xlab="Years",
     main="Histogram of Years with density curve and rug")
lines(density(MockJury$Years),lwd=3,col="red")
rug(jitter(MockJury$Years),col="blue",lwd=2)
```

![(\#fig:Figure2-3)(ref:fig2-3)](02-reintroductionToStatistics_files/figure-latex/Figure2-3-1.pdf) 
 
The graphical tools we've just discussed are going to help us move to comparing the
distribution of responses across more than one group. We will have two displays
that will help us make these comparisons. The simplest is 
*the **side-by-side boxplot***, where a boxplot is displayed for each group 
of interest using the same y-axis scaling. In R, we can use its ***formula*** notation to see if the response (``Years``) differs based on the group 
(``Attr``) by using something like ``Y~X`` or, here, ``Years~Attr``. 
We also need to tell R where to find the variables -- use the last option in the command, ``data=DATASETNAME`` , to inform R of the data.frame to look in 
to find the variables. In this example, ``data=MockJury``. We will use 
the formula and ``data=...`` options in almost every function we use
from here forward. Figure \@ref(fig:Figure2-4) contains the side-by-side 
boxplots showing right skew for all the groups, slightly higher median and 
more variability for the *Unattractive* group along with some potential outliers indicated in two of the three groups. 

(ref:fig2-4) Side-by-side boxplot of Years based on picture groups. 


```r
boxplot(Years~Attr,data=MockJury)
```

![(\#fig:Figure2-4)(ref:fig2-4)](02-reintroductionToStatistics_files/figure-latex/Figure2-4-1.pdf) 

The "~" (which is read as the *tilde* symbol, which you can find in the
upper left corner of your keyboard) notation will be used in two ways this 
semester. The formula use in R employed previously declares that the 
response variable here is *Years* and the explanatory variable is *Attr*. 
The other use for "~" is as shorthand for "is distributed as" and is used in
the context of ``Y~N(0,1)``, which translates (in statistics) to defining the 
random variable *Y* as following a Normal distribution^[Remember the 
bell-shaped curve you encountered in introductory statistics? If not, you can 
see some at https://en.wikipedia.org/wiki/Normal_distribution] with mean 0 
and standard deviation of 1. In the current situation, we could ask whether
the ``Years`` variable seems like it may follow a normal distribution, in 
other words, is *Years*``~N(0,1)``? Since the responses are right
skewed with some groups having outliers, it is not reasonable to assume that
the *Years* variable for any of the three groups may follow a Normal 
distribution (more later on the issues this creates!). Remember that 
$\mu$ and $\sigma$ are parameters where 
$\mu$ ("mu") is our standard symbol for the ***population mean***
and that $\sigma$ ("sigma") is the symbol of the 
***population standard deviation***. 

##Beanplots {#section2-2}

The other graphical display for comparing multiple groups we will use is a newer
display called a ***beanplot*** [@Kampstra2008]. Figure \@ref(fig:Figure2-5) 
shows an example of a beanplot that provides a side-by-side display that 
contains the density curves, the original observations that generated the 
density curve in a (jittered) rug-plot, the mean of each group, and the 
overall mean of the entire data set. For each group, the density curves 
are mirrored to aid in visual assessment of the shape of the distribution, 
which makes a "bean" in some cases. This mirroring also
creates a shape that resembles a violin with skewed distributions so this
display has also been called a "violin plot". The innovation in the beanplot is
to add bold horizontal lines at the mean for each group. It also adds a lighter
dashed line for the overall mean. All together this plot shows us information
on the center (mean), spread, and shape of the distributions of the responses. 
Our inferences typically focus on the means of the groups and this plot allows
us to compare those across the groups while gaining information on the shapes
of the distributions of responses in each group. 

To use the ``beanplot`` function we need to install and load the ``beanplot`` 
package [@R-beanplot]. The function works like the boxplot used previously
except that options 
for ``log``, ``col``, and ``method`` need to be specified. Use these^[Well, you 
can use other colors (try "lightblue" for example), but I think bisque
looks nice in these plots.] options for any beanplots you make: 
``log="", col="bisque", method="jitter"`` 

(ref:fig2-5) Beanplot of Years by picture group. Long, bold lines correspond 
to mean of each group.


```r
require(beanplot)
beanplot(Years~Attr,data=MockJury,log="",col="bisque",method="jitter")
```

![(\#fig:Figure2-5)(ref:fig2-5)](02-reintroductionToStatistics_files/figure-latex/Figure2-5-1.pdf) 
 
Figure \@ref(fig:Figure2-5) reinforces the strong right skews that were also 
detected in the boxplots previously. The three large sentences of 15 years 
can now be clearly identified, with one in the *Beautiful* group and two in 
the *Unattractive* group. The *Unattractive* group seems to have more high
observations than the other groups even though the *Beautiful* group had the 
largest number of observations around 10years. The mean sentence was highest 
for the *Unattractive* group and the difference in the means between 
*Beautiful* and *Average* was small. 

In this example, it appears that the mean for *Unattractive* is larger 
than the other two groups. But is this difference real? We will never 
know the answer to that question, but we 
can assess how likely we are to have seen a result as extreme or more 
extreme than our result, assuming that there is no difference in the 
means of the groups. And if the observed result is
(extremely) unlikely to occur, then we can reject the hypothesis that the
groups have the same mean and conclude that there is evidence of a real
difference. To start exploring whether there are differences in the means, we
need to have numerical values to compare. We can get means and standard
deviations by groups easily using the same formula notation with the ``mean``
and ``sd`` functions if the ``mosaic`` package is loaded.


```r
mean(Years ~ Attr, data = MockJury)
```

```
##    Beautiful      Average Unattractive 
##     4.333333     3.973684     5.810811
```

```r
sd(Years ~ Attr, data = MockJury)
```

```
##    Beautiful      Average Unattractive 
##     3.405362     2.823519     4.364235
```

We can also use the ``favstats`` function to get those summaries and others.


```r
favstats(Years ~ Attr, data = MockJury)
```

```
##           Attr min Q1 median   Q3 max     mean       sd  n missing
## 1    Beautiful   1  2      3  6.5  15 4.333333 3.405362 39       0
## 2      Average   1  2      3  5.0  12 3.973684 2.823519 38       0
## 3 Unattractive   1  2      5 10.0  15 5.810811 4.364235 37       0
```

Based on these results, we can see that there is an estimated difference of 
almost 2 years in the mean sentence between *Average* and *Unattractive* groups. Because there are three groups being compared in this study, we will have to 
wait until Chapter 3 and the One-Way ANOVA test to fully assess evidence 
related to some difference among the three groups. For now, we are going to 
focus on comparing the mean *Years* between *Average* and *Unattractive* groups
-- which is a ***2 independent sample mean*** situation and something you 
should have seen before. Remember that the "independent" sample part of 
this refers to observations that are independently observed for the two 
groups as opposed to the paired sample situation that you may have 
explored where one observation from the first group is related to an 
observation in the second group (repeated measures on the same person 
or the famous "twin" studies with one twin assigned to each group). 

Here we are going to use the "simple" two independent group scenario to 
review some basic statistical concepts and connect two different 
frameworks for conducting statistical inference: randomization and 
parametric inference techniques. ***Parametric*** statistical methods 
involve making assumptions about the distribution of the
responses and obtaining confidence intervals and/or p-values using a 
*named* distribution (like the z or $t$-distributions). Typically these
results are generated using formulas and looking up areas under curves or
cutoffs using a table or a computer. ***Randomization***-based statistical
methods use a computer to shuffle, sample, or simulate observations in ways
that allow you to obtain distributions of possible results to find areas and
cutoffs without resorting to using tables and named distributions. 
Randomization methods are what are called ***nonparametric*** methods 
that often make fewer assumptions (they are ***not free of assumptions***!)
and so can handle a larger set of problems more easily than parametric 
methods. When the assumptions involved in the parametric procedures are 
met by a data set, the randomization methods often provide very similar 
results to those provided by the parametric techniques. To be a more 
sophisticated statistical consumer, it is useful to have some knowledge 
of both of these approaches to statistical inference and the fact that 
they can provide similar results might deepen your understanding of both 
approaches. 

We will start with comparing the *Average* and *Unattractive* groups to 
compare these two ways of doing inference. We could remove the *Beautiful* 
group observations in a spreadsheet program and read that new data set 
back into R, but it is actually pretty easy to use R to do data
management once the data set is loaded. To remove the observations that came
from the *Beautiful* group, we are going to generate a new variable 
that we will call ``NotBeautiful`` that is true when observations came
from another group (*Average* or *Unattractive*) and false for observations
from the *Beautiful* group. To do this, we will apply the ***not equal***
logical function (``!=`` ) to the variable ``Attr``, inquiring whether it 
was different from the ``"Beautiful"`` level. You can see the content of the 
new variable in the output:


```r
MockJury$NotBeautiful <- MockJury$Attr != "Beautiful"
MockJury$NotBeautiful
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
##  [23]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
##  [34]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
##  [45]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
##  [56]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
##  [67]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
##  [78] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [89] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE
## [100]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [111]  TRUE  TRUE  TRUE  TRUE
```

This new variable is only FALSE for the *Beautiful* responses as we can see
if we compare some of the results from the original and new variable:


```r
head(data.frame(MockJury$Attr, MockJury$NotBeautiful))
```

```
##   MockJury.Attr MockJury.NotBeautiful
## 1     Beautiful                 FALSE
## 2     Beautiful                 FALSE
## 3     Beautiful                 FALSE
## 4     Beautiful                 FALSE
## 5     Beautiful                 FALSE
## 6     Beautiful                 FALSE
```

```r
tail(data.frame(MockJury$Attr, MockJury$NotBeautiful))
```

```
##     MockJury.Attr MockJury.NotBeautiful
## 109       Average                  TRUE
## 110       Average                  TRUE
## 111       Average                  TRUE
## 112       Average                  TRUE
## 113       Average                  TRUE
## 114       Average                  TRUE
```

To get rid of one of the groups, we need to learn a little bit about data 
management in R. ***Brackets*** ``([, ])`` are used to modify the rows or
columns in a data.frame with entries before the comma operating on rows and
entries after the comma on the columns. For example, if you want to see the
results for the 5$^{th}$ subject, you can reference the 5$^{th}$ row of the 
data.frame using ``[5, ]`` after the data.frame name:


```r
MockJury[5,]
```

```
##        Attr    Crime Years Serious exciting calm independent sincere warm
## 5 Beautiful Burglary     7       9        1    1           5       1    8
##   phyattr sociable kind intelligent strong sophisticated happy ownPA
## 5       8        9    4           7      9             9     8     7
##   NotBeautiful
## 5        FALSE
```
We could just extract the *Years* response for the 5$^{th}$ 
subject by incorporating information on the row and column of interest 
(``Years`` is the 3$^{rd}$ column):


```r
MockJury[5,3]
```

```
## [1] 7
```

In R, we can use logical vectors to keep any rows of the data.frame where 
the variable is true and drop any rows where it is false by placing the 
logical variable in the first element of the brackets. The reduced version 
of the data set should be saved with a different name such as ``MockJury2`` 
that is used here to reduce the chances of confusing it with the previous 
full data set:


```r
MockJury2 <- MockJury[MockJury$NotBeautiful,]
```

You will always want to check that the correct observations were dropped 
either using ``View(MockJury2)`` or by doing a quick summary of the 
``Attr`` variable in the new data.frame. 


```r
summary(MockJury2$Attr)
```

```
##    Beautiful      Average Unattractive 
##            0           38           37
```

It ends up that R remembers the *Beautiful* category even though there are
0 observations in it now and that can cause us some problems. When we remove a
group of observations, we sometimes need to clean up categorical variables to
just reflect the categories that are present. The ``factor`` function 
creates categorical variables based on the levels of the variables that are 
observed and is useful to run here to clean up ``Attr``. 


```r
MockJury2$Attr <- factor(MockJury2$Attr) 
summary(MockJury2$Attr)
```

```
##      Average Unattractive 
##           38           37
```

Now if we remake the boxplots and beanplots, they only contain results for 
the two groups of interest here as seen in Figure \@ref(fig:Figure2-6). 

(ref:fig2-6) Boxplot and beanplot of the Years responses on the reduced data set. 


```r
boxplot(Years ~ Attr,data=MockJury2) 
beanplot(Years ~ Attr,data=MockJury2,log="",col="bisque",method="jitter")
```

![(\#fig:Figure2-6)(ref:fig2-6)](02-reintroductionToStatistics_files/figure-latex/Figure2-6-1.pdf) ![(\#fig:Figure2-6)(ref:fig2-6)](02-reintroductionToStatistics_files/figure-latex/Figure2-6-2.pdf) 

The two-sample mean techniques you learned in your previous course all 
start with comparing the means the two groups. We can obtain the two 
means using the ``mean`` function or directly obtain the difference 
in the means using the ``diffmean`` function (both require the ``mosaic``
package). The ``diffmean`` function provides 
$\bar{x}_{Unattractive} - \bar{x}_{Average}$ where $\bar{x}$
(read as "x-bar") is the sample mean of observations in the subscripted 
group. Note that there are two directions that you could compare the
means and this function chooses to take the mean from the second group 
name *alphabetically* and subtract the mean from the first alphabetical group 
name. It is always good to check the direction of this calculation as 
having a difference of $-1.84$ years versus $1.84$ years could be important. 


```r
mean(Years ~ Attr, data=MockJury2)
```

```
##      Average Unattractive 
##     3.973684     5.810811
```

```r
diffmean(Years ~ Attr, data=MockJury2)
```

```
## diffmean 
## 1.837127
```

## Models, hypotheses, and permutations for the 2 sample mean situation {#section2-3}

There appears to be some evidence that the *Unattractive* group is 
getting higher average lengths of sentences from the prisoner "jurors" than
the *Average* group, but we want to make sure that the difference is 
real -- that there is evidence to reject the assumption that the means 
are the same "in the population". First, a ***null hypothesis***^[The 
hypothesis of no difference that is typically generated in the hopes of
being rejected in favor of the alternative hypothesis which contains the sort
of difference that is of interest in the application.] which 
defines a ***null model***^[The null model is the statistical model that 
is implied by the chosen null hypothesis. Here, a null hypothesis of no 
difference translates to having a model with the same mean for both groups.] 
needs to be determined in terms of ***parameters*** (the true values in 
the population). The research question should help you determine the form of the
hypotheses for the assumed population. In the 2 independent sample mean
problem, the interest is in testing a null hypothesis of $H_0: \mu_1 = \mu_2$
versus the alternative hypothesis of $H_A: \mu_1 \ne \mu_2$, where 
$\mu_1$ is the parameter for the true mean of the first group and $\mu_2$
is the parameter for the true mean of the second group. The alternative
hypothesis involves assuming a statistical model for the $i^{th} (i=1,\ldots,n_j)$
response from the $j^{th} (j=1,2)$ group, $\boldsymbol{y}_{ij}$, that 
involves modeling it as $y_{ij} = \mu_j + \varepsilon_{ij}$, 
where we assume that $\varepsilon_{ij} \sim N(0,\sigma^2)$. For the moment, 
focus on the models that either assume the means are the same (null) or 
different (alternative), which imply:

* Null Model: $y_{ij} = \mu + \varepsilon_{ij}$ There is **no**
difference in **true** means for the two groups.

* Alternative Model: $y_{ij} = \mu_j + \varepsilon_{ij}$ There is **a**
difference in **true** means for the two groups.


Suppose we are considering the alternative model for the 4<sup>th</sup>
observation ($i=4$) from the second group ($j=2$), then the model for 
this observation is $y_{42} = \mu_2 +\varepsilon_{42}$, that defines the 
response as coming from the true mean for the second group plus a 
random error term for that observation, $\varepsilon_{42}$. For, say, the 
5<sup>th</sup> observation from the first group ($j=1$), the model is 
$y_{51} = \mu_1 +\varepsilon_{51}$. If we were working with the null model, 
the mean is always the same ($\mu$) - the group specified does not change 
the mean we use for that observation. 

It can be helpful to think about the null and alternative models graphically.
By assuming the null hypothesis is true (means are equal) and that the random
errors around the mean follow a normal distribution, we assume that the truth
is as displayed in the left panel of Figure \@ref(fig:Figure2-7) -- two 
normal distributions with the same mean and variability. The alternative 
model allows the two groups to potentially have different means, such as 
those displayed in the right panel of Figure \@ref(fig:Figure2-7) where the 
second group has a larger mean. Note that in this scenario, we assume that 
the observations all came from the same distribution except that they had 
different means. Depending on the statistical procedure we are using, we 
basically are going to assume that the observations ($y_{ij}$) either were 
generated as samples from the null or alternative model. You can imagine 
drawing observations at random from the pictured distributions. For hypothesis 
testing, the null model is assumed to be true and then the unusualness of 
the actual result is assessed relative to that assumption. In hypothesis 
testing, we have to decide if we have enough evidence to reject the assumption 
that the null model (or hypothesis) is true. If we reject the null hypothesis, 
then we would conclude that the other model considered (the alternative 
model) is more reasonable. The researchers obviously would have hoped to 
encounter some sort of noticeable difference in the sentences provided for the
different pictures and been able to find enough evidence to reject the null 
model where the groups "look the same". 

(ref:fig2-7) Illustration of the assumed situations under the null (left) 
and a single possibility that could occur if the alternative were true 
(right) and the true means were different. 

![(\#fig:Figure2-7)(ref:fig2-7)](chapter1_files/image015.png) 


In statistical inference, null hypotheses (and their implied models) are set 
up as "straw men" with every interest in rejecting them even though we assume 
they are true to be able to assess the evidence <u>against them</u>. Consider 
the original study design here, the pictures were randomly assigned to the 
subjects. If the null hypothesis were true, then we would have no difference 
in the population means of the groups. And this would apply if we had done a 
different random assignment of the pictures to the subjects. So let's try this: 
assume that the null hypothesis is true and randomly re-assign the treatments
(pictures) to the observations that were obtained. In other words, keep the 
sentences (*Years*) the same and shuffle the group labels randomly. The
technical term for this is doing a ***permutation*** (a random shuffling of 
the treatments relative to the responses). If the null is true and the means 
in the two groups are the same, then we should be able to re-shuffle the 
groups to the observed sentences (*Years*) and get results similar to those we
actually observed. If the null is false and the means are really different in
the two groups, then what we observed should differ from what we get under
other random permutations. The differences between the two groups should be
more noticeable in the observed data set than in (most) of the shuffled data
sets. It helps to see an example of a permutation of the labels to understand
what this means here. 

In the ``mosaic`` package, the ``shuffle`` function allows us to easily perform
a permutation^[We'll see the ``shuffle`` function in a more common usage below; 
while the code to generate ``Perm1`` is provided, it isn't something to worry 
about right now.]. Just one time, we can explore what a permutation of the 
treatment labels could look like in the ``PermutedAttr`` variable below. Note 
that the ``Years`` are held in the same place the group labels are shuffled. 


```r
set.seed(1234)
```


```r
Perm1 <- with(MockJury2,data.frame(Years,Attr,PermutedAttr=shuffle(Attr)))
Perm1
```

```
##    Years         Attr PermutedAttr
## 1      1 Unattractive Unattractive
## 2      4 Unattractive      Average
## 3      3 Unattractive      Average
## 4      2 Unattractive      Average
## 5      8 Unattractive      Average
## 6      8 Unattractive      Average
## 7      1 Unattractive Unattractive
## 8      1 Unattractive Unattractive
## 9      5 Unattractive      Average
## 10     7 Unattractive Unattractive
## 11     1 Unattractive      Average
## 12     5 Unattractive Unattractive
## 13     2 Unattractive Unattractive
## 14    12 Unattractive      Average
## 15    10 Unattractive      Average
## 16     1 Unattractive      Average
## 17     6 Unattractive Unattractive
## 18     2 Unattractive      Average
## 19     5 Unattractive Unattractive
## 20    12 Unattractive Unattractive
## 21     6 Unattractive      Average
## 22     3 Unattractive      Average
## 23     8 Unattractive      Average
## 24     4 Unattractive Unattractive
## 25    10 Unattractive Unattractive
## 26    10 Unattractive      Average
## 27    15 Unattractive Unattractive
## 28    15 Unattractive      Average
## 29     3 Unattractive      Average
## 30     3 Unattractive      Average
## 31     3 Unattractive Unattractive
## 32    11 Unattractive      Average
## 33    12 Unattractive      Average
## 34     2 Unattractive Unattractive
## 35     1 Unattractive Unattractive
## 36     1 Unattractive Unattractive
## 37    12 Unattractive      Average
## 38     5      Average Unattractive
## 39     5      Average Unattractive
## 40     4      Average Unattractive
## 41     3      Average Unattractive
## 42     6      Average      Average
## 43     4      Average      Average
## 44     9      Average      Average
## 45     8      Average Unattractive
## 46     3      Average      Average
## 47     2      Average Unattractive
## 48    10      Average      Average
## 49     1      Average Unattractive
## 50     1      Average Unattractive
## 51     3      Average Unattractive
## 52     1      Average      Average
## 53     3      Average      Average
## 54     5      Average      Average
## 55     8      Average Unattractive
## 56     3      Average      Average
## 57     1      Average      Average
## 58     1      Average Unattractive
## 59     1      Average      Average
## 60     2      Average      Average
## 61     2      Average Unattractive
## 62     1      Average      Average
## 63     1      Average Unattractive
## 64     2      Average Unattractive
## 65     3      Average Unattractive
## 66     4      Average Unattractive
## 67     5      Average      Average
## 68     3      Average Unattractive
## 69     3      Average      Average
## 70     3      Average      Average
## 71     2      Average Unattractive
## 72     7      Average Unattractive
## 73     6      Average Unattractive
## 74    12      Average Unattractive
## 75     8      Average      Average
```

If you count up the number of subjects in each group by counting the number 
of times each label (Average, Unattractive) occurs, it is the same in both the 
``Attr`` and ``PermutedAttr`` columns. Permutations involve randomly 
re-ordering the values of a variable -- here the ``Attr`` group labels -- without
changing the content of the variable. This result can also be generated using 
what is called ***sampling without replacement***: sequentially select $n$ labels 
from the original variable, removing each used label and making sure that each 
original ``Attr`` label is selected once and only once. The new, randomly 
selected order of selected labels provides the permuted labels. Stepping
through the process helps to understand how it works: after the initial random
sample of one label, there would $n - 1$ choices possible; on the $n^{th}$
selection, there would only be one label remaining to select. This makes sure
that all original labels are re-used but that the order is random. Sampling
without replacement is like picking names out of a hat, one-at-a-time, and not
putting the names back in after they are selected. It is an exhaustive process
for all the original observations. ***Sampling with replacement*** , in contrast,
involves sampling from the specified list with each observation having an equal 
chance of selection for each sampled observation -- in other words, observations 
can be selected more than once. This is like picking $n$ names out of a hat that
contains $n$ names, except that every time a name is selected, it goes back into 
the hat -- we'll use this technique in Section \@ref(section2-8)
to do what is called ***bootstrapping***. Both sampling mechanisms can be 
used to generate inferences but each has particular situations
where they are most useful. For hypothesis testing, we will use permutations
(sampling without replacement). 

The comparison of the beanplots for the real data set and permuted version of 
the labels is what is really interesting (Figure \@ref(fig:Figure2-8)). The 
original difference in the sample means of the two groups was 1.84 years 
(Unattractive minus Average). The sample means are the ***statistics*** 
that estimate the parameters for the true means of the two groups. In the 
permuted data set, the difference in the means is 1.15 years in the opposite 
direction (Average had a higher mean than Unattractive in the permuted data). 


```r
mean(Years ~ PermutedAttr, data=Perm1)
```

```
##      Average Unattractive 
##     5.447368     4.297297
```

```r
diffmean(Years ~ PermutedAttr, data=Perm1)
```

```
##  diffmean 
## -1.150071
```

(ref:fig2-8) Boxplots of Years responses versus actual treatment groups and 
permuted groups. 


![(\#fig:Figure2-8)(ref:fig2-8)](02-reintroductionToStatistics_files/figure-latex/Figure2-8-1.pdf) 

These results suggest that the observed difference was larger than what we got 
when we did a single permutation although it was only a little bit larger than 
a difference we could observe in permutations if we ignore the difference in 
directions. Conceptually, permuting observations between group labels is 
consistent with the null hypothesis -- this is a technique to generate results 
that we might have gotten if the null hypothesis were true since the responses 
are the same in the two groups if the null is true. We just need to repeat the
permutation process many times and track how unusual our observed result is 
relative to this distribution of potential responses if the null were true. 
If the observed differences are unusual relative to the results under 
permutations, then there is evidence against the null hypothesis, the null 
hypothesis should be rejected ( Reject $H_0$), and a conclusion should be made, 
in the direction of the alternative hypothesis, that there is evidence that the 
true means differ. If the observed differences are similar to (or at least not 
unusual relative to) what we get under random shuffling under the null model, 
we would have a tough time concluding that there is any real difference between 
the groups based on our observed data set. 

## Permutation testing for the 2 sample mean situation {#section2-4}

In any testing situation, you must define some function of the observations that
gives us a single number that addresses our question of interest. This quantity
is called a ***test statistic***. These often take on complicated forms and 
have names like $t$ or $z$ statistics that relate to their parametric (named)
distributions so we know where to look up ***p-values***^[P-values are the 
probability of obtaining a result as extreme as or more extreme than we observed 
given that the null hypothesis is true.]. In randomization settings, they can
have simpler forms because we use the data set to find the distribution of the statistic and don't need to rely on a named distribution. We will label our test statistic ***T*** (for **T** statistic) unless the test statistic has a commonly
used name. Since we are interested in comparing the means of the two groups, we
can define

$$T=\bar{x}_{Unattractive}-\bar{x}_{Average},$$

which coincidentally is what the``diffmean`` function provided us previously. 
We label our ***observed test statistic*** (the one from the original data 
set) as

$$T_{obs}=\bar{x}_{Unattractive}-\bar{x}_{Average},$$

which happened to be 1.84 years here. We will compare this result to the results 
for the test statistic that we obtain from permuting the group labels. To 
denote permuted results, we will add a * to the labels:

$$T^*=\bar{x}_{Unattractive}-\bar{x}_{Average^*}.$$

We then compare the $T_{obs}=\bar{x}_{Unattractive}-\bar{x}_{Average} = 1.84$ to
the distribution of results that are possible for the permuted results ($T^*$)
which corresponds to assuming the null hypothesis is true. 

We need to consider lots of permutations to do a permutation test. In contrast to
your introductory statistics course where, if you did this, it was just a click
away, we are going to learn what was going on under the hood. Specifically, we
need a ***for loop*** in R to be able to repeatedly generate the permuted data
sets and record $T^*$ for each one. Loops are a basic programming task that make
randomization methods possible as well as potentially simplifying any repetitive
computing task. To write a "for loop", we need to choose how many times we want 
to do the loop (call that ``B``) and decide on a counter to keep track of where 
we are at in the loops (call that ``b``, which goes from 1 up to ``B``). The 
simplest loop just involves printing out the index, ``print(b)`` at each step. 
This is our first use of curly braces, { and}, that are used to group the code 
we want to repeatedly run as we proceed through the loop. By typing the following 
code in the script window and then highlighting it all and hitting the run button, 
R will go through the loop 5 times, printing out the counter:

```
B <- 5
for (b in (1:B)){
  print(b)
}
```

Note that when you highlight and run the code, it will look about the same with 
"+" printed after the first line to indicate that all the code is connected when 
it appears in the console, looking like this:

```r
> for(b in (1:B)){
+   print(b)
+}
```

When you run these three lines of code, the console will show you the following 
output:

```r
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
```

Instead of printing the counter, we want to use the loop to repeatedly compute 
our test statistic across B random permutations of the observations. The 
``shuffle`` function performs permutations of the group labels relative to 
responses and the ``diffmean`` difference in the two group means in the permuted 
data set. For a single permutation, the combination of shuffling ``Attr`` and 
finding the difference in the means, storing it in a variable called ``Ts`` is:


```r
Ts <- diffmean(Years ~ shuffle(Attr), data=MockJury2)
Ts
```

```
##  diffmean 
## -0.616643
```

And putting this inside the ``print`` function allows us to find the test 
statistic under 5 different permutations easily:


```r
B <- 5
for (b in (1:B)){
  Ts <- diffmean(Years ~ shuffle(Attr), data=MockJury2)
  print(Ts)
}
```

```
##   diffmean 
## -0.8300142 
##   diffmean 
## -0.1365576 
##    diffmean 
## -0.08321479 
##  diffmean 
## 0.5035562 
## diffmean 
## 1.677098
```

Finally, we would like to store the values of the test statistic instead of 
just printing them out on each pass through the loop. To do this, we need to 
create a variable to store the results, let's call it ``Tstar``. We know that 
we need to store ``B`` results so will create a vector of length B, which 
contains B elements, full of missing values (NA) using the ``matrix`` function:


```r
Tstar <- matrix(NA, nrow=B)
Tstar
```

```
##      [,1]
## [1,]   NA
## [2,]   NA
## [3,]   NA
## [4,]   NA
## [5,]   NA
```

Now we can run our loop B times and store the results in ``Tstar``. 


```r
for (b in (1:B)){
  Tstar[b] <- diffmean(Years ~ shuffle(Attr), data=MockJury2)
}
Tstar
```

```
##             [,1]
## [1,] -0.08321479
## [2,]  0.23684211
## [3,] -0.24324324
## [4,] -0.61664296
## [5,]  0.66358464
```

Five permutations are still not enough to assess whether our $T_{obs}$
of 1.84 is unusual and we need to do many permutations to get an accurate 
assessment of the possibilities under the null hypothesis. It is common practice 
to consider something like 1,000 permutations. The ``Tstar`` vector when we set 
*B* the permutation distribution for the selected test statistic under^[We often say
"under" in statistics and we mean "given that the following is true".] the null
hypothesis -- what is called the ***null distribution*** of the statistic. The 
null distribution is the distribution of possible values of a statistic
under the null hypothesis. We want to visualize this distribution and use it to
assess how unusual our $T_{obs}$ result of 1.84 years was relative to all the
possibilities under permutations (under the null hypothesis). So we repeat the
loop, now with $B=1000$ and generate a histogram, density curve and summary
statistics of the results:

(ref:fig2-9) Histogram (left, with counts in bars) and density curve (right) of 
values of test statistic for 1,000 permutations. 


```r
B <- 1000
Tstar <- matrix(NA, nrow=B)
for (b in (1:B)){
  Tstar[b] <- diffmean(Years ~ shuffle(Attr), data=MockJury2)
}
hist(Tstar, label=T)
plot(density(Tstar), main="Density curve of Tstar")
```

![(\#fig:Figure2-9)(ref:fig2-9)](02-reintroductionToStatistics_files/figure-latex/Figure2-9-1.pdf) ![(\#fig:Figure2-9)(ref:fig2-9)](02-reintroductionToStatistics_files/figure-latex/Figure2-9-2.pdf) 


```r
favstats(Tstar)
```

```
##        min         Q1     median        Q3      max       mean        sd
##  -2.910384 -0.5099573 0.07681366 0.6102418 2.530583 0.04694168 0.8497364
##     n missing
##  1000       0
```

Figure \@ref(fig:Figure2-9) contains visualizations of $T^*$ and the ``favstats``
summary provides the related numerical summaries. Our observed $T_{obs}$
of 1.84 seems fairly unusual relative to these results with only 
20 $T^*$ values over 2 based on the 
histogram. We need to make more specific comparisons of the permuted results 
versus our observed result to be able to clearly decide whether our observed 
result is really unusual. 

To make the comparisons more concrete, first we can enhance the previous graphs 
by adding the value of the test statistic from the real data set, as shown in 
Figure \@ref(fig:Figure2-10), using the ``abline`` function. 

(ref:fig2-10) Histogram (left) and density curve (right) of
values of test statistic for 1,000 permutations with bold vertical line for
value of observed test statistic. 


```r
Tobs <- 1.837
hist(Tstar, labels=T)
abline(v=Tobs, lwd=2, col="red")
plot(density(Tstar),main="Density curve of Tstar")
abline(v=Tobs, lwd=2, col="red")
```

![(\#fig:Figure2-10)(ref:fig2-10)](02-reintroductionToStatistics_files/figure-latex/Figure2-10-1.pdf) ![(\#fig:Figure2-10)(ref:fig2-10)](02-reintroductionToStatistics_files/figure-latex/Figure2-10-2.pdf) 

Second, we can calculate the exact number of permuted results that were larger 
than what we observed. To calculate the proportion of the 1,000 values that were 
larger than what we observed, we will use the ``pdata`` function. To use this 
function, we need to provide the distribution of values to compare to the cut-off
(``Tstar``), the cut-off point (``Tobs``), and whether we want calculate the
proportion that are below (left of) or above (right of) the cut-off 
(``lower.tail=F`` option provides the proportion of values above the cutoff 
of interest). 


```r
pdata(Tstar, Tobs, lower.tail=F)
```

```
## [1] 0.02
```



The proportion of 0.02 tells us that 20 of the 1,000 permuted results 
(2%) were larger than what we observed. This type of work is how we can
generate ***p-values*** using permutation distributions. P-values, as you should
remember, are the probability of getting a result as extreme as or more extreme than what we observed, <u>given that the null is true</u>. Finding only 20
permutations of 1,000 that were larger than our observed result suggests that it 
is hard to find a result like what we observed if there really were no difference,
although it is not impossible. 

When testing hypotheses for two groups, there are two types of alternative
hypotheses, one-sided or two-sided. ***One-sided tests*** involve only considering
differences in one-direction (like $\mu_1 > \mu_2$) and are performed when 
researchers can decide ***a priori***^[This is a fancy way of saying "in advance",
here in advance of seeing the observations.] which group should have a larger mean 
if there is going to be any sort of difference. In this situation, we did not 
know enough about the potential impacts of the pictures to know which group should 
be larger than the other so should do a two-sided test. It is important to 
remember that you can't look at the responses to decide on the hypotheses. It is
often safer and more ***conservative***^[Statistically, a conservative method is 
one that provides less chance of rejecting the null hypothesis in comparison to 
some other method or less than some pre-defined standard.] to start with a
***two-sided alternative*** ($\mathbf{H_A: \mu_1 \ne \mu_2}$). To do a 2-sided 
test, find the area larger than what we observed as above. We also need to add 
the area in the other tail (here the left tail) similar to what we observed in the
right tail. Some people suggest doubling the area in one tail but we will collect
information on the number that were more extreme than the same value in the other 
tail. In other words, we count the proportion over 1.84 and below -1.84. So 
we need to also find how many of the permuted results were smaller than -1.84years 
to add to our previous proportion. Using ``pdata`` ``-Tobs`` as the cut-off and 
``lower.tail=T`` provides this result:


```r
pdata(Tstar, -Tobs, lower.tail=T)
```

```
## [1] 0.014
```



So the p-value to test our null hypothesis of no difference in the true means 
between the groups is 0.02 + 0.014, providing a p-value of 0.034.
Figure \@ref(fig:Figure2-11) shows both cut-offs on the histogram and density curve. 

(ref:fig2-11) Histogram and density curve of values of test statistic for 1,000
permutations with bold lines for value of observed test statistic and its opposite
value required for performing two-sided test.


```r
hist(Tstar, labels=T)
abline(v=c(-1,1)*Tobs, lwd=2, col="red")
```

![(\#fig:Figure2-11)(ref:fig2-11)](02-reintroductionToStatistics_files/figure-latex/Figure2-11-1.pdf) 

```r
plot(density(Tstar),main="Density curve of Tstar")
abline(v=c(-1,1)*Tobs, lwd=2, col="red")
```

![(\#fig:Figure2-11)(ref:fig2-11)](02-reintroductionToStatistics_files/figure-latex/Figure2-11-2.pdf) 

In general, the ***one-sided test p-value*** is the proportion of the permuted
results that are more extreme than observed in the direction of the *alternative*
hypothesis (lower or upper tail, remembering that this also depends on the direction 
of the difference taken). For the 2-sided test, the p-value is the proportion of the
permuted results that are *less than the negative version of the observed statistic
and greater than the positive version of the observed statistic*. Using absolute 
values (| |), we can simplify this: the ***two-sided p-value*** is the 
*proportion of the |permuted statistics| that are larger than |observed statistic|*.
This will always work and finds areas in both tails regardless of whether the 
observed statistic is positive or negative. In R, the ``abs`` function provides the
***absolute value*** and we can again use ``pdata`` to find our p-value in one line 
of code: 


```r
pdata(abs(Tstar), abs(Tobs), lower.tail=F)
```

```
## [1] 0.034
```

We will discuss the choice of ***significance level*** below, but for the moment,
assume that $\alpha$ is chosen to be our standard value of 0.05. Since the p-value 
is smaller than $\alpha$, this suggests that we can ***reject the null hypothesis***
and conclude that there is evidence of some difference in the true mean sentences 
given between the two types of pictures. 

Before we move on, let's note some interesting features of the permutation 
distribution of the difference in the sample means shown in 
Figure \@ref(fig:Figure2-11). 

1. It is basically centered at 0. Since we are performing permutations assuming 
the null model is true, we are assuming that $\mu_1 = \mu_2$ which implies that
$\mu_1 - \mu_2 = 0$. This also suggests that 0 should be the center of the 
permutation distribution and it was. 

2. It is approximately normally distributed. This is due to the ***Central Limit Theorem***^[We'll leave the discussion of the CLT to your previous stat coursework 
or an internet search. Remember that it has something to do with distributions
looking more normal as the sample size increases.], where the 
***sampling distribution*** (distribution of all possible results for samples 
of this size) of the difference in sample means ($\bar{x}_1 - \bar{x}_2$) becomes 
more and normally distributed as the sample sizes increase. With 38 and 37 
observations in the groups, we are likely to have a relatively normal looking
distribution of the difference in the sample means. This result will allow us to 
use a parametric method to approximate this sampling distribution under the null 
model if some assumptions are met, as we'll discuss below. 

3. Our observed difference in the sample means (1.84 years) is a fairly unusual 
result relative to the rest of these results but there are some permuted data 
sets that produce more extreme differences in the sample means. When the 
observed differences are really large, we may not see any permuted results 
that are as extreme as what we observed. When ``pdata`` gives you 0, the p-value
should be reported to be smaller than 0.0001 (**not 0!**) since it happened 
in less than 1 in 1000 tries but does occur once -- in the actual data set. 

4. Since our null model is not specific about the direction of the difference, 
considering a result like ours but in the other direction (-1.84 years) needs to 
be included. The observed result seems to put about the same area in both tails 
of the distribution but it is not exactly the same. The small difference in the 
tails is a useful aspect of this approach compared to the parametric method 
discussed below as it accounts for slight asymmetry in the sampling distribution. 

Earlier, we decided that the p-value was small enough to reject the null 
hypothesis since it was smaller than our chosen level of significance. In this 
course, you will often be allowed to use your own judgment about an appropriate
significance level in a particular situation (in other words, if we forget to 
tell you an $\alpha$ -level, you can still make a decision using a reasonably 
selected significance level). Remembering that the p-value is the probability 
you would observe a result like you did (or more extreme), assuming the null 
hypothesis is true, this tells you that the smaller the p-value is, the more 
evidence you have against the null. The next section provides a more formal 
review of the hypothesis testing infrastructure, terminology, and some of 
things that can happen when testing hypotheses. 

## Hypothesis testing (general) {#section2-5}

In hypothesis testing, it is formulated to answer a specific question about
a population or true parameter(s) using a statistic based on a data set.
In your previous statistics course, you (hopefully) considered one-sample 
hypotheses about population means and proportions and the two sample mean 
situation we are focused on here. Our hypotheses relate to trying to answer 
the question about whether the population mean sentences between the two 
groups are different, with an initial assumption of no difference. 

Hypothesis testing is much like a criminal trial where you are in the role
of a jury member or judge, if no jury is present. Initially, the defendant
is assumed innocent. In our situation, the true means are assumed to be 
equal between the groups. Then evidence is presented and, as a juror,
you analyze it. In statistical hypothesis testing, data are collected and 
analyzed. Then you have to decide if we had "enough" evidence to reject 
the initial assumption ("innocence" that is initially assumed). To make 
this decision, you want to have previously decided on the standard of 
evidence required to reject the initial assumption. In criminal cases, 
"beyond a reasonable doubt" is used. Wikipedia's definition
(https://en.wikipedia.org/wiki/Reasonable_doubt) suggests that this 
standard is that "there can still be a doubt, but only to the extent 
that it would not affect a reasonable person's belief regarding whether 
or not the defendant is guilty". In civil trials, a lower standard called 
a "preponderance of evidence" is used. Based on that defined and pre-decided
(*a priori*) measure, you decide that the defendant is guilty or not guilty.
In statistics, we compare our p-value to a significance level, $\alpha$, 
which is most of the time selected to be 5%. If our p-value is less than 
$\alpha$, we reject the null hypothesis. The choice of the significance level 
is like the variation in standards of evidence between criminal and civil 
trials -- and in all situations everyone should know the standards required 
for rejecting the initial assumption before any information is "analyzed".
Once someone is found guilty, then there is the matter of sentencing which 
is related to the impacts ("size") of the crime. In statistics, this is 
similar to the estimated size of differences and the related judgments 
about whether the differences are practically important or not. If the 
crime is proven beyond a reasonable doubt but it is a minor crime, then 
the sentence will be small. With the same level of evidence and a more 
serious crime, the sentence will be more dramatic. 

There are some important aspects of the testing process to note that inform 
how we interpret statistical hypothesis test results. When someone is found 
"not guilty", it does not mean "innocent", it just means that there was not 
enough evidence to find the person guilty "beyond a reasonable doubt".
Not finding enough evidence to reject the null hypothesis does not imply 
that the true means are equal, just that there was not enough evidence to 
conclude that they were different. There are many potential reasons why we 
might fail to reject the null, but the most common one is that our sample 
size was too small (which is related to having too little evidence). 

Throughout this material, we will continue to re-iterate the distinctions 
between parameters and statistics and want you to be clear about the 
distinctions between estimates based on the sample and inferences for the
population or true values of the parameters of interest. Remember that 
statistics are summaries of the sample information and parameters are 
characteristics of populations (which we rarely know). In the two-sample 
mean situation, the sample means are always at least a little different 
-- that is not an interesting conclusion. What is interesting is whether
we have enough evidence to prove that the population or true means
differ "beyond a reasonable doubt". 

The scope of any inferences is constrained based on whether there is a 
***random sample*** (RS) and/or ***random assignment*** (RA).
Table \@ref(tab:Table2-2) contains the four possible combinations of these 
two characteristics of a given study. Random assignment allows for causal
inferences for differences that are observed -- the difference in treatment
levels causes differences in the mean responses. Random sampling (or at least
some sort of representative sample) allows inferences to be made to the
population of interest. If we do not have RA, then causal inferences cannot be
made. If we do not have a representative sample, then our inferences are
limited to the sampled subjects. 

(ref:tab2-2) Scope of inference summary.

















































































