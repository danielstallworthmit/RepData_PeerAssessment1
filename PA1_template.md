---
title: "Reproducible Research Knitr"
author: "Daniel Stallworth"
date: "4/12/2017"
output: html_document
---



## Loading and preprocessing the data

<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activitydata</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.data.table</span><span class="hl std">(</span><span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;/Users/danielstallworth/Downloads/activity.csv&quot;</span><span class="hl std">,</span> <span class="hl kwc">na.strings</span> <span class="hl std">=</span> <span class="hl str">'NA'</span><span class="hl std">))</span>
<span class="hl kwd">head</span><span class="hl std">(activitydata)</span>
</pre></div>
<div class="output"><pre class="knitr r">##    steps       date interval
## 1:    NA 2012-10-01        0
## 2:    NA 2012-10-01        5
## 3:    NA 2012-10-01       10
## 4:    NA 2012-10-01       15
## 5:    NA 2012-10-01       20
## 6:    NA 2012-10-01       25
</pre></div>
</div></div>

## What is mean total number of steps taken per day?  

### Steps taken per day histogram

<div class="chunk" id="fig1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">totsteps</span> <span class="hl kwb">&lt;-</span> <span class="hl std">activitydata[,</span><span class="hl kwc">j</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">totalsteps</span><span class="hl std">=</span><span class="hl kwd">sum</span><span class="hl std">(steps,</span><span class="hl kwc">na.rm</span> <span class="hl std">= T)),</span><span class="hl kwc">by</span> <span class="hl std">= date]</span>
<span class="hl kwd">qplot</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=totsteps</span><span class="hl opt">$</span><span class="hl std">totalsteps)</span> <span class="hl opt">+</span> <span class="hl kwd">geom_histogram</span><span class="hl std">(</span><span class="hl kwc">bins</span> <span class="hl std">=</span> <span class="hl num">30</span><span class="hl std">)</span>
</pre></div>
<div class="message"><pre class="knitr r">## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
</pre></div>
<div class="rimage default"><img src="./figure/fig1-1.png" title="plot of chunk fig1" alt="plot of chunk fig1" width="600px" height="600px" class="plot" /></div>
</div></div>

### Mean of Steps per day

<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">mean</span><span class="hl std">(totsteps</span><span class="hl opt">$</span><span class="hl std">totalsteps,</span><span class="hl kwc">na.rm</span> <span class="hl std">= T)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 9354.23
</pre></div>
</div></div>


### Median of Steps per day

<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">median</span><span class="hl std">(totsteps</span><span class="hl opt">$</span><span class="hl std">totalsteps,</span><span class="hl kwc">na.rm</span> <span class="hl std">= T)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 10395
</pre></div>
</div></div>


## What is the average daily activity pattern?  

### Average steps per day interval

<div class="chunk" id="fig2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">intervalstepsavg</span> <span class="hl kwb">&lt;-</span> <span class="hl std">activitydata[,</span><span class="hl kwc">j</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">avgintervalsteps</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps,</span><span class="hl kwc">na.rm</span> <span class="hl std">= T)),</span> <span class="hl kwc">by</span> <span class="hl std">= interval]</span>
<span class="hl kwd">head</span><span class="hl std">(intervalstepsavg)</span>
</pre></div>
<div class="output"><pre class="knitr r">##    interval avgintervalsteps
## 1:        0        1.7169811
## 2:        5        0.3396226
## 3:       10        0.1320755
## 4:       15        0.1509434
## 5:       20        0.0754717
## 6:       25        2.0943396
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">with</span><span class="hl std">(intervalstepsavg,</span> <span class="hl kwd">plot</span><span class="hl std">(interval, avgintervalsteps,</span><span class="hl kwc">type</span> <span class="hl std">=</span> <span class="hl str">&quot;l&quot;</span><span class="hl std">))</span>
</pre></div>
<div class="rimage default"><img src="./figure/fig2-1.png" title="plot of chunk fig2" alt="plot of chunk fig2" width="600px" height="600px" class="plot" /></div>
</div></div>


### Max average interval steps

<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">intervalstepsavg[avgintervalsteps</span> <span class="hl opt">==</span> <span class="hl kwd">max</span><span class="hl std">(avgintervalsteps,</span><span class="hl kwc">na.rm</span> <span class="hl std">= T)]</span>
</pre></div>
<div class="output"><pre class="knitr r">##    interval avgintervalsteps
## 1:      835         206.1698
</pre></div>
</div></div>

## Imputing missing values  

### Count of missing values

<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activitydata))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 2304
</pre></div>
</div></div>


### Imputing missing values with the mean of the interval

<div class="chunk" id="fig3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">imputedactivitydata</span> <span class="hl kwb">&lt;-</span> <span class="hl std">activitydata</span>
<span class="hl com"># If na, set it to the mean steps for that interval which has already been computed in intervalstepsavg data table, so just look up the average value there and input into the missing value</span>
<span class="hl std">imputedactivitydata</span><span class="hl opt">$</span><span class="hl std">steps</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ifelse</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activitydata</span><span class="hl opt">$</span><span class="hl std">steps)</span> <span class="hl opt">==</span> <span class="hl std">T, intervalstepsavg</span><span class="hl opt">$</span><span class="hl std">avgintervalsteps[activitydata</span><span class="hl opt">$</span><span class="hl std">interval</span> <span class="hl opt">%in%</span> <span class="hl std">intervalstepsavg</span><span class="hl opt">$</span><span class="hl std">interval], activitydata</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl com"># If average value is still na set to 0</span>
<span class="hl std">imputedactivitydata</span><span class="hl opt">$</span><span class="hl std">steps[</span><span class="hl kwd">is.na</span><span class="hl std">(imputedactivitydata</span><span class="hl opt">$</span><span class="hl std">steps)]</span> <span class="hl kwb">&lt;-</span> <span class="hl num">0</span>
<span class="hl com"># Get the total steps per day including imputed values and plot histogram</span>
<span class="hl std">imputedtotsteps</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputedactivitydata[,</span><span class="hl kwc">j</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">totalsteps</span><span class="hl std">=</span><span class="hl kwd">sum</span><span class="hl std">(steps)),</span><span class="hl kwc">by</span> <span class="hl std">= date]</span>
<span class="hl kwd">qplot</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=imputedtotsteps</span><span class="hl opt">$</span><span class="hl std">totalsteps)</span> <span class="hl opt">+</span> <span class="hl kwd">geom_histogram</span><span class="hl std">(</span><span class="hl kwc">bins</span> <span class="hl std">=</span> <span class="hl num">30</span><span class="hl std">)</span>
</pre></div>
<div class="message"><pre class="knitr r">## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
</pre></div>
<div class="rimage default"><img src="./figure/fig3-1.png" title="plot of chunk fig3" alt="plot of chunk fig3" width="600px" height="600px" class="plot" /></div>
</div></div>

### Mean of Imputed Steps per day

<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">mean</span><span class="hl std">(imputedtotsteps</span><span class="hl opt">$</span><span class="hl std">totalsteps)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 9530.724
</pre></div>
</div></div>


### Median of Imputed Steps per day

<div class="chunk" id="unnamed-chunk-7"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">median</span><span class="hl std">(imputedtotsteps</span><span class="hl opt">$</span><span class="hl std">totalsteps)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 10439
</pre></div>
</div></div>

Looks like the average total steps per day decreases slightly and the median total steps per day increases slightly.

## Are there differences in activity patterns between weekdays and weekends?  

### Creating Weekday variable and plot of average steps per interval for weekday vs weekend

<div class="chunk" id="fig4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">imputedactivitydata</span><span class="hl opt">$</span><span class="hl std">date</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.Date</span><span class="hl std">(imputedactivitydata</span><span class="hl opt">$</span><span class="hl std">date)</span>
<span class="hl std">weekenddays</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Saturday&quot;</span><span class="hl std">,</span><span class="hl str">&quot;Sunday&quot;</span><span class="hl std">)</span>
<span class="hl std">imputedactivitydata</span><span class="hl opt">$</span><span class="hl std">weekday</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputedactivitydata[,</span><span class="hl kwc">j</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">weekday</span><span class="hl std">=</span><span class="hl kwd">factor</span><span class="hl std">((</span><span class="hl kwd">weekdays</span><span class="hl std">(date)</span> <span class="hl opt">%in%</span> <span class="hl std">weekenddays),</span> <span class="hl kwc">levels</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(T,F),</span> <span class="hl kwc">labels</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">'weekend'</span><span class="hl std">,</span> <span class="hl str">'weekday'</span><span class="hl std">)))]</span>
<span class="hl kwd">head</span><span class="hl std">(imputedactivitydata)</span>
</pre></div>
<div class="output"><pre class="knitr r">##        steps       date interval weekday
## 1: 1.7169811 2012-10-01        0 weekday
## 2: 0.3396226 2012-10-01        5 weekday
## 3: 0.1320755 2012-10-01       10 weekday
## 4: 0.1509434 2012-10-01       15 weekday
## 5: 0.0754717 2012-10-01       20 weekday
## 6: 2.0943396 2012-10-01       25 weekday
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl com"># Average steps for interval by weekend vs weekday</span>
<span class="hl std">imputedintervalstepsavg</span> <span class="hl kwb">&lt;-</span> <span class="hl std">imputedactivitydata[,</span><span class="hl kwc">j</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(</span><span class="hl kwc">avgintervalsteps</span><span class="hl std">=</span><span class="hl kwd">mean</span><span class="hl std">(steps)),</span> <span class="hl kwc">by</span> <span class="hl std">=</span> <span class="hl kwd">list</span><span class="hl std">(weekday,interval)]</span>
<span class="hl kwd">head</span><span class="hl std">(imputedintervalstepsavg)</span>
</pre></div>
<div class="output"><pre class="knitr r">##    weekday interval avgintervalsteps
## 1: weekday        0       2.06037736
## 2: weekday        5       0.40754717
## 3: weekday       10       0.15849057
## 4: weekday       15       0.18113208
## 5: weekday       20       0.09056604
## 6: weekday       25       1.35765199
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">par</span><span class="hl std">(</span><span class="hl kwc">mfrow</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">2</span><span class="hl std">,</span><span class="hl num">1</span><span class="hl std">),</span><span class="hl kwc">mar</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">4</span><span class="hl std">,</span><span class="hl num">4</span><span class="hl std">,</span><span class="hl num">2</span><span class="hl std">,</span><span class="hl num">0</span><span class="hl std">))</span>
<span class="hl kwd">with</span><span class="hl std">(imputedintervalstepsavg[weekday</span> <span class="hl opt">==</span> <span class="hl str">&quot;weekday&quot;</span><span class="hl std">],</span> <span class="hl kwd">plot</span><span class="hl std">(interval, avgintervalsteps,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Weekdays&quot;</span><span class="hl std">,</span> <span class="hl kwc">type</span> <span class="hl std">=</span> <span class="hl str">&quot;l&quot;</span><span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylim</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">0</span><span class="hl std">,</span><span class="hl num">210</span><span class="hl std">)))</span>
<span class="hl kwd">with</span><span class="hl std">(imputedintervalstepsavg[weekday</span> <span class="hl opt">==</span> <span class="hl str">&quot;weekend&quot;</span><span class="hl std">],</span> <span class="hl kwd">plot</span><span class="hl std">(interval, avgintervalsteps,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Weekends&quot;</span><span class="hl std">,</span> <span class="hl kwc">type</span> <span class="hl std">=</span> <span class="hl str">&quot;l&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylim</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">0</span><span class="hl std">,</span><span class="hl num">210</span><span class="hl std">)))</span>
</pre></div>
<div class="rimage default"><img src="./figure/fig4-1.png" title="plot of chunk fig4" alt="plot of chunk fig4" width="600px" height="600px" class="plot" /></div>
</div></div>







