Word Predictor
========================================================
author: Yue Yang
date: 2/3/2018
autosize: true

Introduction
========================================================
- This is the capstone project for the coursera course "Data science".
- The project is about building an shiny app that takes in an expression, and then makes a prediction of the most likely next word following that expression.
- The prediction is made according to the expressions people are making in their online communications such as blogs, news and twitters.
- The prediction is based on the n-gram approach. The set of corpus includes up to 4-gram.
- The predictor uses the "stupid back-off" algorithm. 
- The predictor will show the top 10 words with highest probabilities given by the algorithm.


The dataset
========================================================
- The raw data comes from the text content of Blogs, News, and Twitter.
- A small portion of the data was randomly picked from each of the en_US.blogs, en_US.news, and en_US.twitter text file.
- The data was made into a raw corpus using the "tm" package.
- All numbers, punctuations, extra whitespace, and profane words were removed.
- The raw corpus was transformed into 4 "document to frequency" matrices, for (1-4)-gram respectively.
- The ngram matrices was then summarized into 4 word-frequency dataframes and saved.


The Markov assumption of NLP
========================================================
- Statistical language models have been used to assign probabilities to strings of words.

- Suppose we have an expression made up of $L$ words: $w_1^L = \left( {{w_1},{w_2} \ldots {w_L}} \right)$, then according to probability theory, the probability of the appearance of the whole expression should be  
$P\left( {w_1^L} \right) = P\left( {{w_1}} \right)P\left( {{w_2}\left| {{w_1}} \right.} \right)P\left( {{w_3}\left| {{w_1}{w_2}} \right.} \right) \ldots P\left( {{w_L}\left| {w_1^{L - 1}} \right.} \right).$

- To simplify the calculation, Markov assumption can be made. In n-gram level, the assumption is that the appearance of any word in the expression is only related to the n-1 words before it:
$P\left( {w_1^L} \right) \approx \prod\limits_{i = 1}^L {P\left( {{w_i}\left| {w_{i - \left( {n - 1} \right)}^{i - 1}} \right.} \right)}.$

- In this project the Markov assumption has been employed.


The "stupid back-off" algorithm
========================================================
- In n-gram search, we take the last n-1 words from the text the user inputs, and search the n-gram corpus for match. 
- Suppose one of the matches gives word $w_i$ as the next word, then the probablity that this word is the proper choice can be expressed as
$P\left( {{w_i}\left| {w_{i - \left( {n - 1} \right)}^{i - 1}} \right.} \right) = f\left( {w_{i - \left( {n - 1} \right)}^i} \right)/f\left( {w_{i - \left( {n - 1} \right)}^{i - 1}} \right),$ here $f(x)$ is the number of occurence of the string x in n-gram, n is the length of x.
- The "stupid back-off" algorithm always first search possible match in the highest ngram, and return a finite probability if a match is found. If it does not find a match in ngram, it continues to search in the (n-1)gram, returning a discounted probability with a factor of $\alpha$ :
\[P\left( {{w_i}\left| {w_{i - \left( {n - 1} \right)}^{i - 1}} \right.} \right) = \left\{ {\begin{array}{*{20}{c}}
{f\left( {w_{i - \left( {n - 1} \right)}^i} \right)/f\left( {w_{i - \left( {n - 1} \right)}^{i - 1}} \right),\;\;{\rm{if}}\;w_{i - \left( {n - 1} \right)}^i\;{\rm{is}}\;{\rm{found}}\;{\rm{in}}\;{\rm{ngram}}}\\
{\alpha P\left( {{w_i}\left| {w_{i - \left( {n - 2} \right)}^{i - 1}} \right.} \right),\;\;{\rm{otherwise}}}
\end{array}} \right.\]
- The final suggestion of the next word is given according to the probabilities of possible candidates.

How to use the app?
========================================================
- To use the app, the user only need to input a sentence and click "predict" button.
- The top 10 words with highest probabilities are listed according to their generalized scores. 
- The left list is the prediction with stopwords included and the right list is the result without stopwords.
- To reset the input, click "reset".

To use the app, please go to:
<https://positron1.shinyapps.io/word_predictor/>

The source code can be found here:
<https://github.com/positron1/word_predictor>
