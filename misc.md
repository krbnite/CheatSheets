

# Time Series

## AR Models
* [Wiki](https://en.wikipedia.org/wiki/Autoregressive_model)

### Example: AR(0) Model
This is just white noise, possibly centered around some constant/mean, c.

```
x[i] = eps[i] + c
```

### Example: AR(1) Model
```
x[i] = a[1]x[i-1] + eps[i] + c
```

### Example: AR(2) Model
```
x[i] = a[1]x[i-1] + a[2]x[i-2] + eps[i] + c
```

General Model:
```
x[i] = a[i]x[i-1] + ... + a[p]x[i-p] + eps[i] + c
```

This can be written more compactly:
```
x[i] = c + eps[i] + SUM{j=1,p}{a[j]x[i-j]}
```

You can get fancier w/ a backshift operator, where
```
1. Bx[i] = B[1]x[i] = x[i-1]
2. B[j] := B^j = BBB...B (j multiples)

x[i] = c + eps[i] + SUM{j=1,p}{a[j]B[j]x[i]}
```

Hmm, you say, this looks like we can use an algebra of operators to
get even more fancy:
```
(1 - SUM{j=1,p}{a[j]B[j]})x[i] = c + eps[i]
```

This new operator acting on x[i] is a polynomial of the backshift operator, B:
```
f(B;p) = 1 - (a[1]B + a[2]B^2 + ... + a[p]B^p)
```

So we have:
```
f(B;p)x[i] = c + eps[i]
```

Or, getting really fancy and sophisticated:
```
x[i] = (c + eps[i]) / f(B;p)
```

=========================================================================


