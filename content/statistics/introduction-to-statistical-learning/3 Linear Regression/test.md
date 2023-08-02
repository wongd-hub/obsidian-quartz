---
title: "test"
---
$eta_1 = rac{\sum_{i=1}^{n} (x_i - ar{x})(y_i - ar{y})}{\sum_{i=1}^{n} (x_i - ar{x})^2}$
$ar{y} quiv rac{1}{n}\sum_{i=1}^{n} y_i$


**

> [!info] Why are there multiple lines?
> - In practice, the population linear relationship (red line) is not known and must be approximated with data (any of the blue lines). 
> - Fundamentally, this is a natural extension of the standard statistical approach of using information from a sample to estimate characteristics of a large population.
>
> - For example, say we’re interested in knowing the population mean $\mu$ of some random variable $Y$.
> - Unfortunately, $\mu$ is unknown, but we do have access to $n$ observations from $Y$, $y_1, \dots, y_n$ from which we can reasonably estimate $mu pprox ar{y} = rac{1}{n} \sum_{i=1}^{n} y_i$ (we are estimating the population mean using the sample mean).
> - In other words, the sample mean and population mean are different, but the sample mean provides a good approximation of the population mean. This same concept applies to the approximation of $eta_0$ and $eta_1$ using $\hat{eta_0}$ and $\hat{eta_1}$ estimated from sample data.


$	ext{SE}(\hat{eta}_0)^2 = \sigma^2 \left( rac{1}{n} + rac{ar{x}^2}{\sum_{i=1}^{n} (x_i - ar{x})^2} ight)$
**$	ext{SE}(\hat{eta}_1)^2 = rac{\sigma^2}{\sum_{i=1}^{n} (x_i - ar{x})^2}$**
