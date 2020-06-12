+++
title = "Test Post, Part I: Stuff"
description = ""
tags = ["test"]
date = "2017-11-16"
+++

## Language syntax highlighting

Right now, I'm using highlight.js. This post will test out some 
language syntax highlighting.

### Python

Python is pretty straightforward. There also seems to be support for console code, but not IPython.


``` python
#! /usr/bin/env python
-*- coding: utf-8 -*-

def combined_filter(*preds):
    """Creates a filtering function that combines multiple predicates.

    :param *preds: Zero or more predicate functions.
    :returns: A function that takes data, and returns a generator
    of the filtered subset.
    """
    def apply_filters(data):
        keep = (all(f(d) for f in preds) for d in data)
        return (d for d, k in zip(data, keep) if k)
    return apply_filters


def combined_parser(*parsers):
    """Creates a parser that combines multiple sub-parsers.
    """
    def apply_parsers(data):
        for parser in parsers:
            data = parser(data)
        return data
    return apply_parsers

```

A console session:

``` python
>>> def collatz(a=0):
...     return len(list(takewhile(lambda n: n > 1, accumulate(count(a), lambda n, _: 3*n+1 if n % 2 else n //2))))
...
>>> collatz(2)
1
>>> collatz(3)
7
>>> collatz(12)
9
>>> collatz(19)
20
>>> collatz(27)
111
>>> [collatz(i) for i in range(30, 50)]
[18, 106, 5, 26, 13, 13, 21, 21, 21, 34, 8, 109, 8, 29, 16, 16, 16, 104, 11, 24]
>>>
```

### R

Biggest issue is that R is not supported by highlight.js. It may be worth thinking about switching to Pygments or something else.

``` r

# Create a pipe-able logger function that print messages
# and displays incremental time elapsed since the
# last invocation of the function..
step_logger <- function() {
  ..prev_time.. <- Sys.time()
  function(x, msg) {
    this_time <- Sys.time()
    elapsed = this_time - ..prev_time..
    message(sprintf("%s (%.0f sec): %s", this_time, elapsed, msg))
    ..prev_time.. <<- this_time
    x
  }
}


# Try to run a SQL query against con.
# Handles opening and closing the EDW
# connection, even if the query fails.
try_query <- function(query) {
  
  con <- edw_connect()
  
  tryCatch(
    RPostgreSQL::dbGetQuery(con, query),
    error = function(e) e,
    finally = edw_disconnect(con))
}

# Some dplyr
get_site_info <- function(df, site_df) {
  df %>%
    left_join(.fac_table %>%
                select(facname, site),
              by="facname") %>%
    left_join(site_df %>%
	            select(site, lat, lon),
                   by=site)
```

### ELisp, Common Lisp, and Clojure

Clojure is suported. Checking some things like type hints, docstrings,
and Java interop syntax. Apparently, REPL sessions work too.

``` clojure
(def md5-digest
  (let [md5 (MessageDigest/getInstance "MD5")]
    (fn [#^bytes msg-bytes] (.digest md5 msg-bytes))))

(defn md5-hash 
  "Create an MD5 hash of msg."
[#^String msg]
  (let [msg-bytes (.getBytes msg)]
    (md5-digest msg-bytes)))

(defn secret-hash? 
  "Secret hashes have hex codes with 5 leading zeros."
  [hash]
  (and (= [0 0] (take 2 hash))
       (<= 0 (nth hash 2) 7)))
```


### C++ and Rcpp

I often write move functions inside tight loops with C++ and Rcpp. Here's 

<!--more-->

``` c++
#include <cmath>
#include <Rcpp>

double deg2rad(double deg) { return deg * M_PI / 180; }
double rad2deg(double rad) { return rad * 180 / M_PI; }

// The haversine function.
double hav(double x) { return pow(sin(x / 2), 2); }

// Haversine /great-circle distance between two lat/lon
// locations.
double haversine(double lat1, double lon1,
		 double lat2, double lon2,
		 double radius=3961) {
  double dlat = deg2rad(lat2 -  lat1);
  double dlon = deg2rad(lon2 - lon1);
  double a = hav(dlat) + hav(dlon) * (cos(deg2rad(lat1)) 
                                      * cos(deg2rad(lat2))) ;
  double c = 2 * atan2(sqrt(a), sqrt(1 - a));
  return c * radius;
}

// Find the bearing of travel, in degrees, from
// one lat/long location to another. 
double bearing(double lat1, double lon1,
               double lat2, double lon2) {
  double dlon = deg2rad(lon2 - lon1);
  double rlat1 = deg2rad(lat1);
  double rlat2 = deg2rad(lat2);

  double y = sin(dlon) * cos(rlat2);
  double x = cos(rlat1) * sin(rlat2) -
	     sin(rlat1) * cos(rlat2) * cos(dlon);

  return fmod(rad2deg(atan2(y, x)), 360);
}

// [[Rcpp::export(.haversine)]]
NumericVector haversine(const NumericVector& lat1,
			const NumericVector& lon1,
			const NumericVector& lat2,
			const NumericVector& lon2,
			double radius=3961) {
  int n = lat1.length();
  NumericVector distance(n);
  for (int i = 0; i < n; ++i) {
    distance[i] = haversine(lat1[i], lon1[i], lat2[i], lon2[i]);
  }
  return distance;
}
```


### SQL

Keywords are always tricky for highlighters, since many are not standard across vendors.

``` sql
;; CTE
with fnames (name) as (
  select 'john' 
  union select 'mary' 
  union select 'bill'),  
  
minitials (initial) as (
  select 'a' 
  union select 'b' 
  union select 'c'), 
  
lnames (name) as (
  select 'anderson' 
  union select 'hanson' 
  union select 'jones')

select f.name, m.initial, l.name
from fnames f
cross join lnames as l
cross join minitials m;


;; Window function
select posts.id      as post_id, 
       comments.id   as comment_id, 
	   comments.body as body,
       dense_rank() over (
	     partition by post_id
         order by comments.created_at desc) as comment_rank
from posts 
left outer join comments 
on posts.id = comments.post_id;
```

### Stan

Stan isn't supported, of course, but how close does the C++ highligher get us?

``` c++
data {
 int<lower=1> Ntotal;
 int<lower=1> NxLvl;
 int<lower=1> N[Ntotal];
 int<lower=0> y[Ntotal];
 int<lower=0> x[Ntotal];
}
parameters {
  real<lower=0,upper=1> mu[Ntotal];
  vector[NxLvl] a;
  real a0;
  real<lower=0> aSigma;
  real<lower=0> kappaMinusTwo;
}
transformed parameters { 
  vector<lower=0,upper=1>[NxLvl] omega;
  real<lower=0> kappa;
  vector[NxLvl] m;
  real b0;
  vector[NxLvl] b;

  omega = inv_logit( a0 + a );
  kappa = kappaMinusTwo + 2;

  // Convert a0,a[] to sum-to-zero b0,b[] :
  m = a0 + a; // cell means 
  b0 = mean( m );
  b = m - b0;
}
model {
  for ( i in 1:Ntotal ) {
    y[i] ~ binomial( N[i], mu[i] );
    mu[i] ~ beta( omega[x[i]]*(kappa-2)+1, (1-omega[x[i]])*(kappa-2)+1 );
  }
  a ~ normal( 0.0, aSigma^2 );
  a0 ~ normal( 0.0 , 2^2 );
  aSigma ~ gamma( 1.64 , 0.32 );  // mode=2, sd=4
  kappaMinusTwo ~ gamma( 0.01 , 0.01 );  // mean=1 , sd=10 (generic vague)
}
```
## Tables.

Will sometimes want to make tables, and often want to show graphs. Documentation on how tables are parsed in markdown is available at the [blackfriday parser repo.](https://github.com/russross/blackfriday#extensions). 

We'll try that and some math in the next post.
