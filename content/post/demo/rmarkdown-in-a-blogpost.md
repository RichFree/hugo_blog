---
title: "Rmarkdown, in a blog post?"
date: 2022-01-16T23:00:05+08:00
draft: false
summary: "A demonstration of rmarkdown converted to a hugo post"

---

This post is a demonstration of an exported Rmarkdown document that I made for
one of my undergraduate assignments, converted into a blog-post form via the R
package hugodown. 


## Question 8a

import the College.csv dataset using read.csv

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/getwd.html'>getwd</a></span><span class='o'>(</span><span class='o'>)</span>
<span class='c'>#&gt; [1] "/home/richard/Insync/hochuan97@gmail.com/Google Drive/1School/2122Sem1/ST3248/Homework questions/Tutorial1"</span>
<span class='nf'><a href='https://rdrr.io/r/base/getwd.html'>setwd</a></span><span class='o'>(</span><span class='s'>"/home/richard/Insync/hochuan97@gmail.com/Google Drive/1School/2122Sem1/ST3248/Homework questions/Tutorial1"</span><span class='o'>)</span>
<span class='nv'>college</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/utils/read.table.html'>read.csv</a></span><span class='o'>(</span><span class='s'>"College.csv"</span><span class='o'>)</span>
<span class='nf'><a href='https://rdrr.io/r/utils/head.html'>head</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>)</span>
<span class='c'>#&gt;                              X Private Apps Accept Enroll Top10perc Top25perc</span>
<span class='c'>#&gt; 1 Abilene Christian University     Yes 1660   1232    721        23        52</span>
<span class='c'>#&gt; 2           Adelphi University     Yes 2186   1924    512        16        29</span>
<span class='c'>#&gt; 3               Adrian College     Yes 1428   1097    336        22        50</span>
<span class='c'>#&gt; 4          Agnes Scott College     Yes  417    349    137        60        89</span>
<span class='c'>#&gt; 5    Alaska Pacific University     Yes  193    146     55        16        44</span>
<span class='c'>#&gt; 6            Albertson College     Yes  587    479    158        38        62</span>
<span class='c'>#&gt;   F.Undergrad P.Undergrad Outstate Room.Board Books Personal PhD Terminal</span>
<span class='c'>#&gt; 1        2885         537     7440       3300   450     2200  70       78</span>
<span class='c'>#&gt; 2        2683        1227    12280       6450   750     1500  29       30</span>
<span class='c'>#&gt; 3        1036          99    11250       3750   400     1165  53       66</span>
<span class='c'>#&gt; 4         510          63    12960       5450   450      875  92       97</span>
<span class='c'>#&gt; 5         249         869     7560       4120   800     1500  76       72</span>
<span class='c'>#&gt; 6         678          41    13500       3335   500      675  67       73</span>
<span class='c'>#&gt;   S.F.Ratio perc.alumni Expend Grad.Rate</span>
<span class='c'>#&gt; 1      18.1          12   7041        60</span>
<span class='c'>#&gt; 2      12.2          16  10527        56</span>
<span class='c'>#&gt; 3      12.9          30   8735        54</span>
<span class='c'>#&gt; 4       7.7          37  19016        59</span>
<span class='c'>#&gt; 5      11.9           2  10922        15</span>
<span class='c'>#&gt; 6       9.4          11   9727        55</span></code></pre>

</div>

## Question 8b

renaming the rows based on the first column of the dataset

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/colnames.html'>rownames</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>)</span> <span class='o'>&lt;-</span> <span class='nv'>college</span><span class='o'>[</span>, <span class='m'>1</span><span class='o'>]</span> <span class='c'># select all rows of first col</span>
<span class='nv'>college</span> <span class='o'>&lt;-</span> <span class='nv'>college</span><span class='o'>[</span>, <span class='o'>-</span><span class='m'>1</span><span class='o'>]</span>
<span class='nf'><a href='https://rdrr.io/r/utils/head.html'>head</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>)</span>
<span class='c'>#&gt;                              Private Apps Accept Enroll Top10perc Top25perc</span>
<span class='c'>#&gt; Abilene Christian University     Yes 1660   1232    721        23        52</span>
<span class='c'>#&gt; Adelphi University               Yes 2186   1924    512        16        29</span>
<span class='c'>#&gt; Adrian College                   Yes 1428   1097    336        22        50</span>
<span class='c'>#&gt; Agnes Scott College              Yes  417    349    137        60        89</span>
<span class='c'>#&gt; Alaska Pacific University        Yes  193    146     55        16        44</span>
<span class='c'>#&gt; Albertson College                Yes  587    479    158        38        62</span>
<span class='c'>#&gt;                              F.Undergrad P.Undergrad Outstate Room.Board Books</span>
<span class='c'>#&gt; Abilene Christian University        2885         537     7440       3300   450</span>
<span class='c'>#&gt; Adelphi University                  2683        1227    12280       6450   750</span>
<span class='c'>#&gt; Adrian College                      1036          99    11250       3750   400</span>
<span class='c'>#&gt; Agnes Scott College                  510          63    12960       5450   450</span>
<span class='c'>#&gt; Alaska Pacific University            249         869     7560       4120   800</span>
<span class='c'>#&gt; Albertson College                    678          41    13500       3335   500</span>
<span class='c'>#&gt;                              Personal PhD Terminal S.F.Ratio perc.alumni Expend</span>
<span class='c'>#&gt; Abilene Christian University     2200  70       78      18.1          12   7041</span>
<span class='c'>#&gt; Adelphi University               1500  29       30      12.2          16  10527</span>
<span class='c'>#&gt; Adrian College                   1165  53       66      12.9          30   8735</span>
<span class='c'>#&gt; Agnes Scott College               875  92       97       7.7          37  19016</span>
<span class='c'>#&gt; Alaska Pacific University        1500  76       72      11.9           2  10922</span>
<span class='c'>#&gt; Albertson College                 675  67       73       9.4          11   9727</span>
<span class='c'>#&gt;                              Grad.Rate</span>
<span class='c'>#&gt; Abilene Christian University        60</span>
<span class='c'>#&gt; Adelphi University                  56</span>
<span class='c'>#&gt; Adrian College                      54</span>
<span class='c'>#&gt; Agnes Scott College                 59</span>
<span class='c'>#&gt; Alaska Pacific University           15</span>
<span class='c'>#&gt; Albertson College                   55</span></code></pre>

</div>

comments: the first column is now Private, and each row is now named with the university

## Question 8c i

Produce numerical summary of variables in the data set

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/summary.html'>summary</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>)</span>
<span class='c'>#&gt;    Private               Apps           Accept          Enroll    </span>
<span class='c'>#&gt;  Length:777         Min.   :   81   Min.   :   72   Min.   :  35  </span>
<span class='c'>#&gt;  Class :character   1st Qu.:  776   1st Qu.:  604   1st Qu.: 242  </span>
<span class='c'>#&gt;  Mode  :character   Median : 1558   Median : 1110   Median : 434  </span>
<span class='c'>#&gt;                     Mean   : 3002   Mean   : 2019   Mean   : 780  </span>
<span class='c'>#&gt;                     3rd Qu.: 3624   3rd Qu.: 2424   3rd Qu.: 902  </span>
<span class='c'>#&gt;                     Max.   :48094   Max.   :26330   Max.   :6392  </span>
<span class='c'>#&gt;    Top10perc       Top25perc      F.Undergrad     P.Undergrad     </span>
<span class='c'>#&gt;  Min.   : 1.00   Min.   :  9.0   Min.   :  139   Min.   :    1.0  </span>
<span class='c'>#&gt;  1st Qu.:15.00   1st Qu.: 41.0   1st Qu.:  992   1st Qu.:   95.0  </span>
<span class='c'>#&gt;  Median :23.00   Median : 54.0   Median : 1707   Median :  353.0  </span>
<span class='c'>#&gt;  Mean   :27.56   Mean   : 55.8   Mean   : 3700   Mean   :  855.3  </span>
<span class='c'>#&gt;  3rd Qu.:35.00   3rd Qu.: 69.0   3rd Qu.: 4005   3rd Qu.:  967.0  </span>
<span class='c'>#&gt;  Max.   :96.00   Max.   :100.0   Max.   :31643   Max.   :21836.0  </span>
<span class='c'>#&gt;     Outstate       Room.Board       Books           Personal   </span>
<span class='c'>#&gt;  Min.   : 2340   Min.   :1780   Min.   :  96.0   Min.   : 250  </span>
<span class='c'>#&gt;  1st Qu.: 7320   1st Qu.:3597   1st Qu.: 470.0   1st Qu.: 850  </span>
<span class='c'>#&gt;  Median : 9990   Median :4200   Median : 500.0   Median :1200  </span>
<span class='c'>#&gt;  Mean   :10441   Mean   :4358   Mean   : 549.4   Mean   :1341  </span>
<span class='c'>#&gt;  3rd Qu.:12925   3rd Qu.:5050   3rd Qu.: 600.0   3rd Qu.:1700  </span>
<span class='c'>#&gt;  Max.   :21700   Max.   :8124   Max.   :2340.0   Max.   :6800  </span>
<span class='c'>#&gt;       PhD            Terminal       S.F.Ratio      perc.alumni   </span>
<span class='c'>#&gt;  Min.   :  8.00   Min.   : 24.0   Min.   : 2.50   Min.   : 0.00  </span>
<span class='c'>#&gt;  1st Qu.: 62.00   1st Qu.: 71.0   1st Qu.:11.50   1st Qu.:13.00  </span>
<span class='c'>#&gt;  Median : 75.00   Median : 82.0   Median :13.60   Median :21.00  </span>
<span class='c'>#&gt;  Mean   : 72.66   Mean   : 79.7   Mean   :14.09   Mean   :22.74  </span>
<span class='c'>#&gt;  3rd Qu.: 85.00   3rd Qu.: 92.0   3rd Qu.:16.50   3rd Qu.:31.00  </span>
<span class='c'>#&gt;  Max.   :103.00   Max.   :100.0   Max.   :39.80   Max.   :64.00  </span>
<span class='c'>#&gt;      Expend        Grad.Rate     </span>
<span class='c'>#&gt;  Min.   : 3186   Min.   : 10.00  </span>
<span class='c'>#&gt;  1st Qu.: 6751   1st Qu.: 53.00  </span>
<span class='c'>#&gt;  Median : 8377   Median : 65.00  </span>
<span class='c'>#&gt;  Mean   : 9660   Mean   : 65.46  </span>
<span class='c'>#&gt;  3rd Qu.:10830   3rd Qu.: 78.00  </span>
<span class='c'>#&gt;  Max.   :56233   Max.   :118.00</span></code></pre>

</div>

## Question 8c ii

Produce scatterplot matrix of first 10 columns

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Private</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/factor.html'>as.factor</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Private</span><span class='o'>)</span> <span class='c'># turning Private to factor</span>
<span class='nf'><a href='https://rdrr.io/r/graphics/pairs.html'>pairs</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>[</span>,<span class='m'>1</span><span class='o'>:</span><span class='m'>10</span><span class='o'>]</span><span class='o'>)</span> 
</code></pre>
<img src="/figs/unnamed-chunk-4-1.png" width="700px" style="display: block; margin: auto;" />

</div>

## Question 8c iii

boxplots of outstate Outstate verses Private

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/graphics/plot.default.html'>plot</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Private</span>, <span class='nv'>college</span><span class='o'>$</span><span class='nv'>Outstate</span>,
     xlab <span class='o'>=</span> <span class='s'>'Private'</span>,
     ylab <span class='o'>=</span> <span class='s'>'Outstate'</span><span class='o'>)</span>
</code></pre>
<img src="/figs/unnamed-chunk-5-1.png" width="700px" style="display: block; margin: auto;" />

</div>

## Question 8c iv

creating new qualitative variable Elite

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>Elite</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/rep.html'>rep</a></span><span class='o'>(</span><span class='s'>"No"</span>, <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>)</span><span class='o'>)</span> <span class='c'># create repeat vector of no's, for number of rows in college</span>
<span class='nv'>Elite</span><span class='o'>[</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Top10perc</span> <span class='o'>&gt;</span> <span class='m'>50</span><span class='o'>]</span> <span class='o'>&lt;-</span> <span class='s'>"Yes"</span> <span class='c'># replace any No with Yes conditionally for the row</span>
<span class='nv'>Elite</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/factor.html'>as.factor</a></span><span class='o'>(</span><span class='nv'>Elite</span><span class='o'>)</span> <span class='c'># return as factor instead of numeric</span>
<span class='nv'>college</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/data.frame.html'>data.frame</a></span><span class='o'>(</span><span class='nv'>college</span> , <span class='nv'>Elite</span><span class='o'>)</span> <span class='c'># append to dataframe </span></code></pre>

</div>

check how many elite universities

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/summary.html'>summary</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Elite</span><span class='o'>)</span> <span class='c'># 78 elite universities</span>
<span class='c'>#&gt;  No Yes </span>
<span class='c'>#&gt; 699  78</span></code></pre>

</div>

comments: there are 78 elite universities

boxplot of Outstate vs Elite

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/graphics/plot.default.html'>plot</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Elite</span>, <span class='nv'>college</span><span class='o'>$</span><span class='nv'>Outstate</span>,
     xlab <span class='o'>=</span> <span class='s'>'Elite'</span>,
     ylab <span class='o'>=</span> <span class='s'>'Outstate'</span><span class='o'>)</span>
</code></pre>
<img src="/figs/unnamed-chunk-8-1.png" width="700px" style="display: block; margin: auto;" />

</div>

## Question 8c v

Create histograms for a few quantitative variables with differing number of bins

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/graphics/par.html'>par</a></span><span class='o'>(</span>mfrow <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='m'>2</span>,<span class='m'>2</span><span class='o'>)</span><span class='o'>)</span> <span class='c'># set plot into 4 quadrants</span>
<span class='c'># Apps</span>
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Apps</span>, breaks<span class='o'>=</span><span class='m'>10</span>,main <span class='o'>=</span> <span class='s'>"Application histogram, bin 10"</span><span class='o'>)</span>  
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Apps</span>, breaks<span class='o'>=</span><span class='m'>50</span>,main <span class='o'>=</span> <span class='s'>"Application histogram, bin 50"</span><span class='o'>)</span>  
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Apps</span>, breaks<span class='o'>=</span><span class='m'>100</span>, main <span class='o'>=</span> <span class='s'>"Application histogram, bin 100"</span><span class='o'>)</span>
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Apps</span>, breaks<span class='o'>=</span><span class='m'>500</span>, main <span class='o'>=</span> <span class='s'>"Application histogram, bin 500"</span><span class='o'>)</span>
</code></pre>
<img src="/figs/unnamed-chunk-9-1.png" width="700px" style="display: block; margin: auto;" />
<pre class='chroma'><code class='language-r' data-lang='r'>
<span class='c'># Top25perc</span>
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Top25perc</span>, breaks<span class='o'>=</span><span class='m'>10</span>, main <span class='o'>=</span> <span class='s'>"Top 25 histogram, bin 10"</span><span class='o'>)</span> 
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Top25perc</span>, breaks<span class='o'>=</span><span class='m'>50</span>, main <span class='o'>=</span> <span class='s'>"Top 25 histogram, bin 50"</span><span class='o'>)</span> 
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Top25perc</span>, breaks<span class='o'>=</span><span class='m'>100</span>,main <span class='o'>=</span> <span class='s'>"Top 25 histogram, bin 100"</span><span class='o'>)</span>
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>Top25perc</span>, breaks<span class='o'>=</span><span class='m'>500</span>,main <span class='o'>=</span> <span class='s'>"Top 25 histogram, bin 500"</span><span class='o'>)</span>
</code></pre>
<img src="/figs/unnamed-chunk-9-2.png" width="700px" style="display: block; margin: auto;" />
<pre class='chroma'><code class='language-r' data-lang='r'>
<span class='c'># Enroll</span>
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>PhD</span>, breaks<span class='o'>=</span><span class='m'>10</span>,  main <span class='o'>=</span> <span class='s'>"PhD histogram, bin 10"</span><span class='o'>)</span> 
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>PhD</span>, breaks<span class='o'>=</span><span class='m'>50</span>,  main <span class='o'>=</span> <span class='s'>"PhD histogram, bin 50"</span><span class='o'>)</span>  
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>PhD</span>, breaks<span class='o'>=</span><span class='m'>100</span>, main <span class='o'>=</span> <span class='s'>"PhD histogram, bin 100"</span><span class='o'>)</span> 
<span class='nf'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='o'>(</span><span class='nv'>college</span><span class='o'>$</span><span class='nv'>PhD</span>, breaks<span class='o'>=</span><span class='m'>500</span>, main <span class='o'>=</span> <span class='s'>"PhD histogram, bin 500"</span><span class='o'>)</span>
</code></pre>
<img src="/figs/unnamed-chunk-9-3.png" width="700px" style="display: block; margin: auto;" />

</div>

## Question 8c vi

Continued exploration.

By observing the scatterplot, we can see several associated variables. Apps, Accept, Enroll, F.Undergrad are associated with each other. Top10perc and Top25perc are associated with each other.

Also, from the boxplots, we can also see that out-of-state (Outstate) tuition is higher for Private universities, and also for elite universities. We can see that out-of-state tuition can be explained partly by whether the university is private, or whether it is an elite university.

