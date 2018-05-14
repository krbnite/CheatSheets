

# Time Series
You have AR models, MA models, and ARMA models.  You've also got those ARIMA models, and how
about a FARIMA model?

Maybe you understand them all... But how do you know which one to use in practice?
* look at autocorrelation fcn
* look at partial autocorrelation fcn


=================================================

## AR Models
* [Wiki](https://en.wikipedia.org/wiki/Autoregressive_model)

An autoregressive (AR) model represents a variable that depends only on its past values (and noise):
```
x[i] = f(x[i-1], ..., x[i-p], eps[i])  # AR[p] model
```

Usually you do not use an AR(p) model w/ p>3.

### Example: AR(0) Model
This is just white noise, possibly centered around some constant/mean, c.

```
x[i] = eps[i] + c
```

### Example: AR(1) Model
```
x[i] = eps[i] + c + a[1]x[i-1]
```

### Example: AR(2) Model
```
x[i] = eps[i] + c + a[1]x[i-1] + a[2]x[i-2] 
```

### General Model
```
x[i] = eps[i] + c + a[1]x[i-1] + ... + a[p]x[i-p]
```

This can be written more compactly:
```
x[i] = eps[i] + c + SUM{j=1,p}{a[j]x[i-j]}
```

You can get fancier w/ a backshift operator, where
```
1. Bx[i] = B[1]x[i] = x[i-1]
2. B[j] := B^j = BBB...B (j multiples)

x[i] = eps[i] + c + SUM{j=1,p}{a[j]B[j]x[i]}
```

Hmm, you say, this looks like we can use an algebra of operators to
get even more fancy:
```
(1 - SUM{j=1,p}{a[j]B[j]})x[i] = eps[i] + c
```

This new operator acting on x[i] is a polynomial of the backshift operator, B:
```
f(B;p) = 1 - (a[1]B + a[2]B^2 + ... + a[p]B^p)
```

So we have:
```
f(B;p)x[i] = eps[i] + c
```

Or, getting really fancy and sophisticated:
```
x[i] = (eps[i] + c) / f(B;p)
```

=========================================================================

## MA Models
A moving average (MA) model is one that relies only on random error terms.
```
x[i] = f(eps[i], eps[i-1], ..., eps[i-q])
```

In a MA model, the error terms are assumed to be white noise processes w/ zero mean. Unlike the 
AR model, a MA model is always stationary.

Importantly, we see that error terms (sometimes called "random shocks") are elevated to a more 
direct status than found in the AR(p) models, where older error terms indirectly affect the current
value. This direct affect also has the property that a noise term (random shock) only affects a time
series for q periods (data points, sample intervals, etc).  In the AR model, though indirect, these
terms affect the current value indefinitely.

(NOTE: I don't like when samples are called
periods...at the least they should be called sample periods... It's just weird b/c there can be so many
"periods" under investigation when you are doing frequency/period analysis.)


### Example: MA(0) Model
Looks the same as an AR(0) model:
```
x[i] = c + eps[i]
```

### Example: MA(1) Model
```
x[i] = c + eps[i] + m[1]eps[i-1]
```

### General Model
```
x[i] = c + eps[i] + m[1]eps[i-1] + ... + m[p]eps[i-p]
```

You can again invoke the idea of a backshift operator, B, and rewrite this in 
a more compact, but perhaps more complicated way:
```
x[i] = c + (1 + m[1]B[1] + ... + m[q]B[q])eps[i]

Or, more succinctly:
x[i] = c + f(B;q)eps[i]
```

This notation links it to the more "electrical engineering" side of time series analysis, where
one might interpret a MA model as a finite impulse response (FIR) filter applied to white noise.

### In Practice: Choosing the Order q for MA(q)
Basically, we know for a MA(q) model, the white noise terms from longer than q samples ago will
no longer affect the current value -- thus have no correlation.  So, create a sample autocorrelation
function (lag your data by n samples and compute correlation for n in [1,...someLargeNumber]), and
determine that lag at which the ACF becomes insignificantly different from zero for all lags beyond
that lag...  Call that lag q and you can estimate a MA(q) model for you data.


## ARMA Models
When we covered the MA(0) model, it looked like the AR(0) model... And, in fact, for any
MA(q) model, it's clear that it can be interpreted as a AR(0)+MA(q) model in a way.  So an
obvious next question is: Can we have a process that looks like a AR(1)+MA(q) model?  And from there,
it is easy to wonder:  Or in general, can we have a process that looks like a AR(p)+MA(q) model?

Obviously, since you can so easily ask the question, you can at as easily write out the 
equation.  The marriage of the two models is no surprise -- one of the models focuses on a previous
values of the target variable, but de-emphasizes the affects of the noise, while the other focuses
only on the random noise components, but does not take into account the previus value of the 
model...  So, at least to me, it seems that the ARMA model starts tending towards something that
is probably more realistic than either model by itself AR(p) or MA(q).  

An autoregressive moving average (ARMA) model is a function of its past values and previous noise inputs:
```
x[i] = f(x[i-1], ..., x[i-p], eps[i], eps[i-1], ..., eps[i-q])
```

More explicitly:
```
x[i] = c + a[1]x[i-1] + ... + a[p]x[i-p] + eps[i] + m[1]eps[i-1] + ... + m[q]eps[i-q]
```

If you split c into two constants, 
then the ARMA is literally the sum of an AR model with an MA model (minus an error term):
```
x[i] = AR(p)[i] + MA(q)[i] - eps[i]
     = (c1 + eps[i] + a[1]x[i-1] + ... + a[p]x[i-p]) 
       + (c2 + eps[i] + m[1]eps[i-1] + ... + eps[i-1])
       - eps[i]
```

## Stationarity
Basically, if a process is stationary, its mean, variance, and covariance does not change
over time... Obviously most things are not like this, and this is why you may go through
some procedures to "whiten" a time series.

Weakly stationary time series are ones where the covariance depends only on the lag k,
not where you compute covariance along the axis of time, e.g.:
```
cov(x[i],x[i-k]) = cov(x[i+10],x[i+10-k])
```

To make a time series stationary, you might find that differencing/differentiating works
wonders... In some of my past empirical work, I've seen many just assume a simple first difference 
does the trick.  Sometimes it's clear that a second difference actually does the job better.  For
most of the time series I've worked with, neither are perfect, which wraps right into some of
my more mathematical work/research, specifically in fractional calculus (there is a relationship
between fractional derivatives and whitening a time series).

## Integrated Processes
If a non-stationary series can be made stationary after a first difference (or first derivative)
is applied, it is said to be "integrated of order 1" and denoted I(1). 

Similarly, if you find that a non-stationary series is rendered stationary after a second difference,
then it is integrated of order 2, I(2).  

Since all MA(q) models are stationary by definition, they can also be thought of as I(0) models (i.e.,
MA(q) models are integrated of order 0).  However, not all AR(p) models are stationary (the coefficients
have to respect certain constraints for this).  So one can imagine that some AR(p) models can be considered
I(0) models...but not all.  The natural next question is: can they be considered I(d) models for some
integration order d?  

At least some of them can!  So there are ARI(p,d) models... But we already showed that you might
as well more generally think of ARMA(p,q) models, so no reason to exclude the MA componen here --- and so
we have ARIMA(p,d,q) models.

