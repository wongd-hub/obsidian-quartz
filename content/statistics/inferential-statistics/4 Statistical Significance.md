---
title: "4 Statistical Significance"
---
#coursera-inferential-stats #statistics 

## Table of contents

1. [[#Inference for Other Estimators]]
2. [[#Decision Errors]]
3. [[#Significance vs. Confidence Level]]
4. [[#Statistical vs. Practical Significance]]]

## Inference for Other Estimators

The methods we've been learning can be applied to other estimators that have nearly-normal sampling distributions, which are listed here:

- The sample mean ($ar{x}$)
- The difference between two sample means ($ar{x}_1 - ar{x}_2$)
- Sample proportion ($\hat{p}$)
- The difference between sample proportions ($\hat{p}_1 - \hat{p}_2$)

### Unbiased estimator

An important assumption about these point estimates is that they are unbiased - their sampling distribution is centred on the true population parameter it estimates. In other words, it does not naturally overor under-estimate the population parameter.

For nearly-normal estimators, the confidence interval equation remains $	ext{point estimate} \pm z^*SE$; all we'll need to do is swap out the formula for the standard error of a different estimate.

We'll go through high level examples here using different standard errors, but we'll go into more detail on each of these later on.

#### Example 1

A 2010 Pew Research foundation poll indicates that among 1,099 college graduates, 33% watch the Daily Show. An American late-night TV Show. The standard error of this estimate is 0.014. We are asked to estimate the 95% confidence interval for the proportion of college graduates who watch The Daily Show.

We have a few pieces of information given to us:

- $n = 1,099$
- $\hat{p} = 0.33$
- $SE = 0.014$

Now to our [[2 Confidence Intervals|confidence interval]]: $	ext{point estimate} \pm z^*SE 	o 0.33 \pm 1.96 	imes 0.014 	o \left(0.303, 0.357ight)$. The critical value is still 1.96 because this point estimate is still nearly-normal.

So we are 95% confident that between 30.3% and 35.7% of college graduates watch the Daily Show.

Since this estimator is unbiased, and again - the sampling distribution of this estimator is nearly-normal, we can calculate a z-score from which we can use to produce probabilities.

$$Z = rac{	ext{point estimate} - 	ext{null value}}{SE}$$

#### Example 2

The third national health and nutrition examination survey NHANES, collected body fat percentage and gender data from over 13,000 subjects in ages between 20 to 80. The average body fat percentage for the 6,580 men in the sample was 23.9%. And this value was 35% for the 7,021 women. The standard error for the difference between the average male and female body fat percentages was 0.114. Do these data provide convincing evidence that men and women have different average body fat percentages? You may assume that the distribution of the point estimate is nearly normal.

The population parameter we'll be estimating here is $\mu = 	ext{the average body fat percentage in the population}$; but across two different population groups.

1. Set hypotheses: $H_0: \mu_{men} = \mu_{women}$, $H_A = \mu_{men} 
eq \mu_{women}$

2. Calculate the point estimate: $\mu_{men} - \mu_{women} = 23.9 - 35 = -11.1$

3. Check conditions: We're told we can assume that the distribution is nearly normal; and since this is a nationwide survey we can assume that random sampling has been used and the observations are independent with respect to their body fat percentages.

4. Calculate the test statistic

What does our sampling distribution centre on? It usually centres on the null hypothesis value of the population parameter, but the original form of our hypothesis does not have an asserted value. For tests between two population means, we can re-write the null hypothesis as:

$$H_0: \mu_{men} = \mu_{women} 	o \mu_{men} - \mu_{women} = 0$$

So our sampling distribution here centres on 0, and our p-value is looking at the area under the CDF for values smaller than -11.1 and values greater than 11.1.

We do not yet know how to calculate the sample standard error of the difference between two population means, but we are given this value for now, so we can calculate our z-score as $Z = rac{-11.1 - 0}{0.114} = -97.4$.

$$P(Z < -97.4) + P(Z > 97.4) pprox 0$$

1. What is the interpretation of this p-value in the context of the research question?

There is strong evidence to reject the null hypothesis as the p-value is very small.

If there were truly no difference between the average body fat percentage of males and females, there would be a nearly 0% chance of getting data like or more extreme than ours. This is strong evidence to reject the hypothesis that there is no difference between males' and females' average body fat percentage.

### Decision Errors

[[3 Hypothesis Testing|Hypothesis tests]] are not flawless. Just like in court cases, innocent people can be incarcerated and guilty people can walk free. However, we have the tools necessary to quantify how often we make errors in statistics.

There are two main types of errors in hypothesis testing, the Type I and Type II errors - whose rates are inversely proportional.

- A Type I error is rejecting $H_0$ when $H_0$ is true
	- This is like declaring the defendant guilty, when they are actually innocent
- A Type II error is failing to reject $H_0$ (i.e. rejecting $H_A$) when $H_A$ is true
	- This is like declaring the defendant innocent, when they are actually guilty

![[Pasted image 20230712215313.png]]

We almost never know if the null or the alternative hypothesis are true with 100% certainty, but we need to consider all possibilities.

#### Which error is the worst to make?

This is a subjective question; however this brings to mind a quote by William Blackstone: 'better that 10 guilty persons escape, than one innocent person suffers' (so a Type I is worse in this case).

#### Type I errors

- We reject $H_0$ when the p-value is below the significance level ($lpha$), which is generally set at 5%.

- This translates to us not wanting to reject $H_0$ incorrectly at least 5% of the time.

Hence, the probability of making a Type I error is equal to $P\left(	ext{Type I error} | H_0	ext{ true}ight) = lpha$; this is why we prefer smaller values of $lpha$.

So how do we choose our $lpha$?

- If a Type I error is especially dangerous or costly, set your significance level small to reduce the chance of a Type I error. By doing this, we demand very strong evidence that the null hypothesis is not the case in order to reject it.

- However, if a Type II error is more costly, choose a larger significance level. This translates to us being cautious about failing to reject the null hypothesis when the null hypothesis is actually false.

#### Probabilities

![[Pasted image 20230712215340.png]]

So:

- Given that the null hypothesis is true; you can either:
	- Incorrectly reject the null hypothesis, with probability $lpha$
	- Correctly retain the null hypothesis, with probability $1 - lpha$

- Given that the alternative hypothesis is true; you can either:
	- Incorrectly reject the alternative hypothesis, with probability $eta$ (your Type II error)
	- Correctly reject the null hypothesis, with probability $1 - eta$. This probability is referred to as the *power* of your test, and is the probability of correctly rejecting $H_0$ (given $H_A$ is true). Note that $eta$ is not trivial to calculate.

Our goal is to keep both $lpha$ and $eta$ low at the same time; but we know that if we push one down, the other will shoot up. We generally want to find a good balance between the two.

#### Type II errors

- If the alternative hypothesis is actually true, what is the probability that we incorrectly reject it?

- The answer is not obvious.
	- If the true population mean is very close to the null hypothesis, then it will be hard to detect a difference and therefore reject $H_0$.
	- Clearly then, $eta$ is dependent on the effect size ($\delta$) - which is the difference between the point estimate and the null value.

## Significance vs. Confidence Level

So far we've looked at two inferential techniques; [[2 Confidence Intervals|confidence intervals]] and [[3 Hypothesis Testing|hypothesis testing]]. These methods are related and therefore it makes sense that their results will agree with each other if the confidence and significance levels are set the same.

Broadly speaking, we can say that the significance level and confidence level are complements of each other.

- The most commonly used significance level is 5%, whereas the most commonly used confidence level is 95%. These are complements.

- However, whether this complement rule works or not depending on if we're doing a oneor two-sided hypothesis test.
	- Recall that confidence intervals are centred on the mean value of the sampling distribution. This means that they will be most related to two-sided hypothesis tests that are also centered on the mean of the sampling distribution.

So:

- A two-sided hypothesis test with significance level of $lpha$ is equivalent to a confidence interval with CL = $1 - lpha$.
- A one-sided hypothesis test with significance level of $lpha$ is equivalent to a confidence interval with CL = $1 - 2 	imes lpha$.
- If $H_0$ is rejected, a CI that agrees with its corresponding hypothesis test should *not include* the null value.
- If $H_0$ is failed to be rejected, a CI that agrees with its corresponding HT should *include* the null value.

## Statistical vs. Practical Significance

While sometimes statistical and practical significance go hand in hand, that is not always the case.

Consider two scenarios, ceteris paribus, in which scenario will the p-value be lower if hypothesis tests were done with:

1. We have n = 100; and,
2. We have n = 10,000

We can see how this will affect the hypothesis test by considering the components that it affects first: the standard error.

$$Z = rac{ar{x} - \mu}{SE} = rac{ar{x} - \mu}{rac{s}{\sqrt{n}}}$$

As $n$ increases, the SE will decrease, driving up the Z-score which means that the point at which you measure your p-value on the distribution (i.e. your test statistic) is being moved further from the center; thus decreasing p-value.

#### Another example

Consider the following example where the sample statistic is very close to the null hypothesis parameter value:

$$H_0: \mu = 49.5, H_A: \mu > 49.5, ar{x} = 50, s = 2$$

If n = 100; $Z = rac{50-49.5}{rac{2}{100}} = rac{50-49.5}{rac{2}{10}} = 0.5/0.2 = 2.5$

If n = 10,000; $Z = rac{50-49.5}{rac{2}{10000}} = rac{50-49.5}{rac{2}{100}} = 0.5/0.02 = 25$

Clearly this is a very large difference in outcomes. A z-score of 25 will have a near-zero p-value, and so is a highly statistically significant finding - however, is it practically significant?

When we're thinking about practical significance, we focus on the effect size ($\delta$), the difference between your po int estimate and your null value (the numerator in the z-score calculation). In both of the scenarios above, we have a relatively small $\delta$ which may not be practically significant, we are able to find a statistically significant result *simply by inflating our sample size*.

To recap:

- Real difference between the point estimate and the null value are easier to detect with larger sample sizes.

- However, very large samples will result in statistical significance for very small effect sizes that may not be practically significant.

- Usually to mitigate this, analysis is done a priori to the hypothesis test to determine what size sample should be gathered. This likely will include consultation with statisticians.

"To call in the statistician after the experiment is done may be no more than asking him to perform a post-mortem examination. He may be able to say what the experiment died of." - R.A. Fisher
