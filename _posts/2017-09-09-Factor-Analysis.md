---


---

<hr>
<h2 id="title-factor-analysisauthor-date-2017-09-15slug-factor-analysiscategories-tags-">title: Factor Analysis<br>
author: ~<br>
date: '2017-09-15’<br>
slug: factor-analysis<br>
categories: []<br>
tags: []</h2>
<pre class=" language-undefined"><code class="prism language-{r language-undefined">knitr::opts_chunk$set(collapse = TRUE)
options(scipen=999)
</code></pre>
<pre class=" language-undefined"><code class="prism language-{r} language-undefined">Movies &lt;- read.csv("https://raw.githubusercontent.com/centerandspread/centerandspread.github.io/master/data/USAMovies.csv")
library(scales)
library(ggplot2)
library(psych)
</code></pre>
<h2 id="dimension-reduction--pca">Dimension Reduction / PCA</h2>
<p>Designed to reduce the number of predictor variables (reducing multicollinearity), essentially transforms the dataset to find a reduced set of variables that still explain most of the variance.</p>
<p>This is an unsupervised learning method. It seeks to use mathematical variance to cluster variables into common factors, without guidance from the researcher.</p>
<p>For low loading (low dimensional) variables (those with low correlation with the outcome variable).</p>
<p>The model aims to capture as much information as possible, with high explained variance in each component. The first component has the highest amount of variance explained, followed by the second, third and so on.</p>
<p>“The objective of PCA is to maximize variance while minimizing covariance.”</p>
<h2 id="unsupervised-learning-models">Unsupervised Learning Models</h2>
<p>PCA<br>
K-means (nearest neighbors?)<br>
GLRM (generalized low rank models)</p>
<h4 id="transformation">Transformation</h4>
<p>You might want to transform the data before putting it into the model. Options: Standardize, Normalize, Descale, Demean, nothing.</p>
<p>Why? Because the original predictors might be on different scales (feet, meters, pints, gallons, etc.). Need to put them all on a similar scale if looking at collinearity / mutlicollinearity.</p>
<h4 id="formula">Formula</h4>
<p>Several PCA formulas exist:<br>
GramSVD, Power, Randomized, GLRM (generalized low-rank model)</p>
<h2 id="factor-analysis">Factor Analysis</h2>
<p>Sometimes variability can be explained by something you can’t seem to categorize. It seems to be hidden in the data. Such “latent variables” are what an exploratory factor analysis (EFA) looks for using mathematical computations.</p>
<p>Explaining variance by narrowing it down, finding clusters of similar responses.</p>
<p>PCA is a normalized linear combination of the predictors in a data set. It seeks to explain the total variance with a fewer number of total variables (looks to cluster variables that explain similar variance).</p>
<p>ONLY load predictor variables. Do not include outcome variable.</p>
<p>First component found, then second should be orthogonal.</p>
<p>Called orthogonal because looks for noncorrelated variables. If the two components are uncorrelated, their directions should be orthogonal (perpendicular).</p>
<p>All of the principal components found next follow the same pattern; they seek to capture remaining variation while also remaining uncorrelated with previous components.</p>
<p>Must use ONLY numeric data. Cannot use categorical (must be dummy coded first). Use one-hot encoding (dummy).</p>
<h4 id="standard-example-">Standard Example-</h4>
<p>In R, we can use prcomp() to conduce a PCA. This function centers the variables to have means equal to zero. Transformation to a normal scale can be done with scale=T (makes variables have SD equal to one).</p>
<p>After running, check the factor loadings of the variables.</p>
<blockquote>
<p>prin_comp$rotation</p>
</blockquote>
<p>Total number of factors with loadings should be minimum of (n-1), so there might be a lot!</p>
<p>Can get a visual of the components:</p>
<blockquote>
<p>biplot(prin_comp, scale = 0)</p>
</blockquote>
<p>Can also make scree plot to look for total number of components to keep, or make a cumulative scree plot.</p>
<h4 id="example">Example</h4>
<p>Hollywood blockbusters are expensive affairs. Budgets are often in the hundreds of millions of dollars, especially when big name actors/actresses and directors are involved. You could probably name a few of the top directors and expect that they would likely receive very large budgets, while those relatively new might have to make magic happen with a much smaller budget.</p>
<p>Let’s take a look at variability in budgets and total number of directors for films in dataset to see if there is variability in the first place to try to describe / explain.</p>
<pre class=" language-undefined"><code class="prism language-{r} language-undefined">dollar_format()(mean(Movies$budget))
summary(Movies$budget)
</code></pre>
<p>Whoa, so the lowest budget was a meager $1,100, and the largest was $300,000,000. That’s quite a range!</p>
<p>Okay, but what about the visual distribution of budgets?</p>
<pre class=" language-undefined"><code class="prism language-{r} language-undefined">ggplot(Movies, aes(x=budget)) + geom_histogram(binwidth=50000000, colour="black", alpha=0.4, aes(y=..count.., fill=..density..)) + scale_x_continuous(label=dollar, breaks=c(0,50000000,100000000,150000000,200000000,250000000,300000000)) + theme(axis.text.x=element_text(angle = -90, hjust = 0))
</code></pre>
<p>Now, how many directors are there for the 3,237 movies we have in our dataset?</p>
<pre class=" language-undefined"><code class="prism language-{r} language-undefined">unique(Movies$director_name)
</code></pre>
<p>Whoa, that was a lot. 1,511 different directors. Yikes. Let’s see if we can find some similarities between them (group or cluster them) using exploratory factor analysis.</p>
<p>Can we group directors in Hollywood by the type of budgets they receive to make their art? Are there clusters of directors that might help to explain the variability in budgets given to movies?</p>
<h2 id="step-one---choose-number-of-factors">Step One - Choose Number of Factors</h2>
<p>Ideally, you will have a guiding theory as to how many factors (clusters) are at work in your variable</p>
<p>*3,237 movies from the USA (reported budgets otherwise not in standard currency) from _ to _ on IMDB.</p>

