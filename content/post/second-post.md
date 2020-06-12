+++
title = "Another *test* post"
description = ""
tags = [
    "test", "layout"
]
date = "2017-11-16"
+++

## Testing math and other markup

This is a link to [the github repo](https://github.com/carljv/therocksbelow/) for the site.

Here's some **important words** and some other *important words*. Sometimes we'll need formulas, so, for $ x_i \in \mathbb{R}^2 $ : 

<div>
\begin{align}
X        &= \left\{x_i\right\}_{i=1}^N    \\\\
y        &= X\beta + N(0, \sigma^2I + Q) \\\\
Q_{i,j}  &= Cov(x_i, x_j)
\end{align}
</div>

Matrices are also, cool.

$$
\left(
  \begin{matrix}
    a & b \\\ 
    c & d
  \end{matrix}
\right)
$$


## Tables and charts.

Here's a table spit out by `kable` from R.

<!--more-->


|Days |  Cum. %|Decision |
|:----|-------:|:--------|
|1    |   0.26%|Wait     |
|2    |   2.61%|Wait     |
|3    |  16.00%|Wait     |
|4    |  34.51%|Wait     |
|5    |  59.32%|Wait     |
|6    |  76.46%|Wait     |
|7    |  87.43%|Wait     |
|8    |  93.94%|Wait     |
|9    |  96.77%|Wait     |
|10   |  98.24%|Panic    |
|11   |  98.95%|Panic    |
|12   |  99.40%|Panic    |
|13   |  99.74%|Panic    |
|14   |  99.92%|Panic    |
|15   | 100.00%|Panic    |


There's also a graph of this table:

<img alt="When to panic" src="../cdf.png" width=460px 
 style='margin: 1em 1em;'></img>


## And here's some copied text from the Hugo docs.

Hugo has its own example site which happens to also be the documentation site
you are reading right now.

Follow the following steps:

 1. Clone the [hugo repository](http://github.com/spf13/hugo)
 2. Go into the repo
 3. Run hugo in server mode and build the docs
 4. Open your browser to http://localhost:1313

Or, if you don't like counting:

  - Connect to the repo
  - Change directory into it
  - Run the server command
  - Open your browser to localhost and the port.

