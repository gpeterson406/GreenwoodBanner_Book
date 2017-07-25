---
header-includes:
- \usepackage{amsmath}
- \usepackage{color}
- \usepackage{sistyle}
- \SIthousandsep{,}
output:
  html_document: default
  pdf_document:
    keep_tex: yes
---

# Chi-square tests {#chapter5}






## Situation, contingency tables, and plots	{#section5-1}

In this chapter, the focus shifts briefly from analyzing quantitative 
response variables to methods
for handling categorical response variables. This is important because in some
situations it is not possible to measure the response variable quantitatively. 
For example, we will analyze the results from a clinical trial where the results
for the subjects were measured as one of three categories: *no improvement*, 
*some improvement*, and *marked improvement*. While that type of response 
could be treated as
numerical, coded possibly as 1, 2, and 3, it would be difficult to assume that
the responses such as those follow a normal distribution since they are 
***discrete*** (not continuous, measured at whole number values only) and, 
more importantly, 
the difference between *no improvement* and *some improvement* is not 
necessarily the same as the difference between *some* and *marked improvement*. 
If it is treated numerically, then the differences are assumed to be the same
unless a different coding scheme is used (say 1, 2, and 5). It is better to
treat these types of responses as being in one of the three categories and use
statistical methods that don't make unreasonable assumptions about what the
numerical coding might mean. The study being performed here involved subjects
randomly assigned to either a treatment or a placebo (control) group and we
want to address research questions similar to those considered in 
Chapters \@ref(chapter2) and \@ref(chapter3) --
assessing differences among two or more groups. With quantitative
responses, the differences in the distributions are parameterized via the means
of the groups and we used 2-sample mean or ANOVA hypotheses and tests. With
categorical responses, the focus is on the probabilities of getting responses in
each category and whether they differ among the groups. 

We start with some useful summary techniques, both numerical and graphical, 
applied to some examples of
studies these methods can be used to analyze. Graphical techniques provide
opportunities for assessing specific patterns in variables, relationships
between variables, and for generally understanding the responses obtained. 
There are many different types of plots and each can enhance certain features
of data. We will start with a "fun" display, called a tableplot, to help us
understand some aspects of the results from a double-blind randomized clinical
trial investigating a treatment for rheumatoid arthritis that has the
categorical response variable introduced previously. These data are available
in the ``Arthritis`` data set available in the ``vcd`` package [@R-vcd].
There were $n=84$ subjects, with some demographic 
information recorded
along with the ``Treatment`` status (*Treated*, *Placebo*) and whether the 
patients' arthritis symptoms ``Improved`` (with levels of *None*, *Some*, 
and *Marked*). 

The ``tableplot`` function from the ``tabplot`` package [@R-tabplot]
displays bars for each response in a row^[In larger data sets, 
multiple subjects are displayed in each row as proportions of
the rows in each category.] based on the 
category of responses or as a bar with the height corresponding
the value of quantitative variables. It also plots a red cell if the
observations were missing on a particular variable. The plot can be obtained
simply as ``tableplot(DATASETNAME)``. But when using ``tableplot``, we may 
not want to display everything in the data.frame and often just select some 
of the variables. We use ``Treatment``, ``Improved``, ``Sex``, and ``Age``
in the ``select=...`` option with a ``c()`` and commas between the names of 
the variables we want to display. The first one in the list is also the one that
the data are sorted based on. 

(ref:fig5-1) Table plot of the arthritis data set.


```r
require(vcd)
data(Arthritis) #Double-blind clinical trial with treatment and control groups
#Homogeneity example
require(tabplot)
tableplot(Arthritis,select=c(Treatment,Improved,Sex,Age))
```

![(\#fig:Figure5-1)(ref:fig5-1)](05-chiSquaredTests_test_files/figure-latex/Figure5-1-1.pdf) 

The first thing we can gather from Figure \@ref(fig:Figure5-1) is that there 
are no red cells so there were no missing
observations in the data set. Missing observations regularly arise in real
studies when observations are not obtained for many different reasons and it is
always good to check for missing data issues -- this plot provides a quick visual
method for doing that check. Primarily we are interested in whether the
treatment led to a different pattern (or rates) of improvement responses. There
seems to be more purple (*Marked*) improvement responses in the treatment 
group and more blue (*None*) responses in the placebo group. 
This sort of plot also helps us to simultaneously consider the role of other
variables in the observed responses. You can see the sex of each subject in the
vertical panel for ``Sex`` and it seems
that there is a relatively reasonable mix of males and females in the
treatment/placebo groups. Quantitative variables are also displayed with
horizontal bars corresponding to the responses. From the panel for 
``Age``, we can see that the ages of subjects ranged from the 20s to 70s 
and that there is no clear difference in
the ages between the treated and placebo groups. If, for example, all the male
subjects had ended up being randomized into the treatment group, then we might
have worried about whether sex and treatment were confounded and whether any
differences in the responses might be due to sex instead of the treatment. The
random assignment of treatment/placebo to the subjects appears to have been
successful here with the ages and sexes appearing to be well-mixed among the
two treatment groups. The main benefit of this sort of plot is the ability to
visualize more than two categorical variables simultaneously. But now we want
to focus more directly on the researchers' main question -- does the treatment
lead to different improvement outcomes than the placebo?

To directly assess the effects of the treatment, we
want to display just the two variables of interest. ***Stacked bar charts***
provide a method of displaying the response patterns (in ``Improved``) across 
the levels of a predictor variable(``Treatment``) by displaying a bar for each
predictor variable level and the proportions of responses in each category of
the response in each of those groups. If the placebo is as effective as the
treatment, then we would expect similar proportions of responses in each
improvement category. A difference in the effectiveness would manifest in
different proportions in the different improvement categories between *Treated* 
and *Placebo*. To get information in this direction, we start with
obtaining the counts in each combination of categories using the ``tally``
function to generate contingency tables. ***Contingency tables*** with 
***R*** rows and ***C*** columns (called ***R by C tables***) summarize 
the counts of observations in each combination of the explanatory and 
response variables. In these data, there are $R=2$ rows and $C=3$ columns 
making a $2\times 3$ table -- note that you do not count the row
and column for the "Totals" in defining the size of the table. In the table, 
there seems to be many more *Marked* improvement responses (21 vs 7) and 
fewer *None* responses (13 vs 29) in the treated group compared to the 
placebo group. 


```r
require(mosaic)
tally(~Treatment+Improved, data=Arthritis, margins=T)
```

```
##          Improved
## Treatment None Some Marked Total
##   Placebo   29    7      7    43
##   Treated   13    7     21    41
##   Total     42   14     28    84
```

Using the ``tally`` function with ``~x+y`` provides a contingency table with
the ``x`` variable on the rows and the ``y`` variable on the columns, with
``margins=T`` as an option so we can obtain the totals along the rows, 
columns, and table total of $N=84$. In general, contingency tables contain 
the counts $n_{rc}$ in the $r^{th}$ row and $c^{th}$ column where
$r=1,\ldots,R$ and $c=1,\ldots,C$. We can also define the ***row totals***
as the sum across row $r$ as

$$\mathbf{n_{r\bullet}}=\Sigma^C_{c=1}n_{rc},$$
the ***column totals*** as the sum across column $c$ as

$$\mathbf{n_{\bullet c}}=\Sigma^R_{r=1}n_{rc},$$

and the ***table total*** as

$$\mathbf{N}=\Sigma^R_{r=1}\mathbf{n_{r\bullet}} = \Sigma^C_{c=1}\mathbf{n_{\bullet c}}
= \Sigma^R_{r=1}\Sigma^C_{c=1}\mathbf{n_{rc}}.$$

We'll need these quantities to do some calculations in a bit. A generic 
contingency table with added row, column, 
and table totals just like the previous result from the ``tally``
function is provided in Table \@ref(tab:Table5-1).

(ref:tab5-1) General notation for counts in an R by C contingency table.

\begin{table}

\caption{(\#tab:Table5-1)(ref:tab5-1)}
\centering
\begin{tabular}[t]{l|l|l|l|l|l|l}
\hline
 & **Response Level 1** & Response Level 2 & Response Level 3 & ... & Response Level C & Totals\\
\hline
$\textbf{Group 1}$ & $n_{11}$ & $n_{12}$ & $n_{13}$ & $\ldots$ & $n_{1C}$ & $\mathbf{n_{1\bullet}}$\\
\hline
$\textbf{Group 2}$ & $n_{21}$ & $n_{22}$ & $n_{23}$ & $\ldots$ & $n_{2C}$ & $\mathbf{n_{2\bullet}}$\\
\hline
$\mathbf{\ldots}$ & $\ldots$ & $\ldots$ & $\ldots$ & $\ldots$ & $\ldots$ & $\mathbf{\ldots}$\\
\hline
$\textbf{Group R}$ & $n_{R1}$ & $n_{R2}$ & $n_{R3}$ & $\ldots$ & $n_{RC}$ & $\mathbf{n_{R\bullet}}$\\
\hline
$\textbf{Totals}$ & $\mathbf{n_{\bullet 1}}$ & $\mathbf{n_{\bullet 2}}$ & $\mathbf{n_{\bullet 3}}$ & $\mathbf{\ldots}$ & $\mathbf{n_{\bullet C}}$ & $\mathbf{N}$\\
\hline
\end{tabular}
\end{table}

Comparing counts from the contingency table is useful, but comparing proportions
in each category is better, especially when the sample sizes in the levels of 
the explanatory variable differ. Switching the formula used in the ``tally ``
function formula to ``~ y | x`` and adding the ``format="proportion"``
option provides the proportions in the response categories conditional on the 
category of the predictor (these are
called ***conditional proportions*** or the ***conditional distribution*** of, 
here, *Improved* on *Treatment*)^[The vertical line, "``|`` ", in ``~ y|x``
is available on most keyboards on the same key as "\". It is the mathematical 
symbol that means "conditional on" whatever follows.]. 
Note that they sum to 1.0 in each level of x, *placebo* or *treated*: 
See \@ref(tab:Table5-2)

(ref:tab5-2) Table test


----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  &nbsp;            **Response\                  **Response\                  **Response\              \             **Response\                       \              
                     Level 1**                    Level 2**                    Level 3**            **...**           Level C**                    **Totals**         
----------- ---------------------------- ---------------------------- ---------------------------- --------- ---------------------------- ----------------------------
**Group 1**           $n_{11}$                     $n_{12}$                     $n_{13}$              ...              $n_{15}$           $\boldsymbol{n_{1 \bullet}}$

**Group 2**           $n_{21}$                     $n_{22}$                     $n_{23}$              ...              $n_{25}$           $\boldsymbol{n_{2 \bullet}}$

  **...**               ...                          ...                          ...                 ...                ...                        **...**           

**Group R**           $n_{R1}$                     $n_{R2}$                     $n_{R3}$              ...              $n_{R5}$           $\boldsymbol{n_{R \bullet}}$

**Totals**  $\boldsymbol{n_{\bullet 1}}$ $\boldsymbol{n_{\bullet 2}}$ $\boldsymbol{n_{\bullet 3}}$    ...    $\boldsymbol{n_{\bullet C}}$       $\boldsymbol{N}$      
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Table: (\#tab:Table5-2) (ref:tab5-2)

Here I am going to reference Table \@ref(tab:Table2-1) and we'll see if it works from the get-go.

(ref:tab2-1) First 5 and last 6 rows of the MockJury data set


----------------------------------------------------------------------
 Subject    Attr     Crime    Years   Serious   Independent   Sincere 
--------- --------- -------- ------- --------- ------------- ---------
    1     Beautiful Burglary   10        8           9           8    

    2     Beautiful Burglary    3        8           9           3    

    3     Beautiful Burglary    5        5           6           3    

    4     Beautiful Burglary    1        3           9           8    

    5     Beautiful Burglary    7        9           5           1    

   ...       ...      ...      ...      ...         ...         ...   

   108     Average  Swindle     3        3           5           4    

   109     Average  Swindle     3        2           9           9    

   110     Average  Swindle     2        1           8           8    

   111     Average  Swindle     7        4           9           1    

   112     Average  Swindle     6        3           5           2    

   113     Average  Swindle    12        9           9           1    

   114     Average  Swindle     8        8           1           5    
----------------------------------------------------------------------

Table: (\#tab:Table2-1) (ref:tab2-1)
