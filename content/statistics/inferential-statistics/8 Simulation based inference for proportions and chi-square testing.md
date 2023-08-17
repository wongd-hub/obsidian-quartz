---
title: "8 Simulation based inference for proportions and chi-square testing"
---
#course_coursera-inferential-stats #statistics 
## Small sample proportions

If the success-failure ≥ 10 rule (i.e. the Sample Size/Skew condition) is not satisfied, then the sampling distribution for your given scenario will not adhere to the CLT, reducing the effectiveness of CLT-based methods.

Here, we turn to simulation-based methods such as bootstrapping.

**Example**

Paul the Octopus once picked the winner of all 8 World Cup games in a year correctly. The 'experiment' design was to give Paul the choice of eating out of one box or another, where each box represented the flag of the competing teams that day.

Our hypothesis test will aim to determine whether Paul is psychic, or if he picked all games right just by chance.

$$H_0: p = 0.5, H_A: p > 0.5, n = 8, \hat{p} = 1$$

Checking to see if the conditions for CLT inference are met:

- Independence
	- We can assume that Paul's picks each day are independent from each other for now

- Sample Size/Skew
	- $0.5 	imes 8 = 4$ which does not meet this condition.

Hence, CLT assumptions are not met and so CLT-based methods do not apply here. Instead, let's use simulation-based methods.

- The p-value for this hypothesis test is $P(	ext{observed or more extreme outcome}\space|\space H_0 	ext{ is true}) \Rightarrow P(\hat{p} \ge 1.0 \space|\space p = 0.5)$

- So what we must do is devise a simulation scheme that can replicate Paul's trials, and perform these simulations under the assumption that the null hypothesis is true

- Repeat this simulation N times, recording the relevant sample statistic each time

- Calculate the p-value as the proportion of simulations that yielded sample statistics that were favourable to the alternative hypothesis

To do this, we must simulate Paul's trials. Since he is picking between two boxes, with an assumed null probability of 50% chance for each box, we can approximate these trials by flipping a coin 8 times per simulation (flipping a coin is used as an example since it is conceptually equivalent to Paul's trial, no other significance here).

Below we:

- Flip a coin 8 times each simulation, naming 'heads' a success and binding this to 1 (and tails to 0)

- The proportion of heads in each 8 flips is then $\hat{p}_i$, for the $i$th simulation; we repeat this N = 100,000 times

- These 100,000 $\hat{p}_i$s form the sampling/bootstrap distribution, see the histogram of results below

- The p-value for this test will then be $P(\hat{p} \ge 1.0 \space|\space p = 0.5) \Rightarrow rac{	ext{num. simulations where }\hat{p} \ge 1}{N} pprox 0.00390$

```r
suppressPackageStartupMessages(library(dplyr));
suppressPackageStartupMessages(library(ggplot2))

options(repr.plot.width = 15, repr.plot.height = 7)

simulate_binary <- function(N, n = 8) {

	results <- vector('double', N)
	
	sapply(
		results,
		function(z) sample(c(0, 1), n, replace = T) |> mean()
	)
	
}

results <- simulate_binary(100000)

tibble(p_hat = results) |
	ggplot(aes(x = p_hat)) +
	geom_histogram(bins = 30) +
	labs(
		title = 'Simulated p-hat for this experiment',
		y = 'Count',
		x = 'Simulated p-hat'
	) +
	scale_y_continuous(labels = scales::comma) +
	scale_x_continuous(labels = scales::percent)
```

![[Pasted image 20230712230158.png]]

```r
scales::percent(
	Filter(function(x) {x >= 1}, results) |> length() / length(results), 
		accuracy = 0.001
)
#> '0.390%'
```

This is under the 5% significance level, hence we reject the null hypothesis as there is strong evidence for the alternative. Does this really mean that Paul is psychic? Probably not. We're likely making a Type I error here or possibly asking the wrong question.

## Examples

Suppose we recruit 12 volunteers to test the validity of the idiom "to know it like the back of one's hand". Each volunteer is shown 10 pictures of gloved hands and must correctly choose which picture contains their hand. Say 11 of 12 people complete this task successfully - is this sufficiently strong evidence that people truly know the backs of their hands?

$$H_0: 	ext{volunteers are no better than randomly guessing, ie }p = rac{1}{10}; H_A: p > rac{1}{10}$$

To devise a simulation scheme that is suitable for this experiment, we note that if the volunteers were truly randomly guessing then their true chance of picking their hand correctly is 0.1.

- We can roll a 10-sided fair die to represent the sampling space, calling '1' a success (recognising one's own hand) and all other outcomes a failure

- For each simulation ($i$), we roll the die 12 times (to represent the 12 original volunteers), calculating $\hat{p}_i$ each time.

- Run the simulation 100 times, creating a bootstrap distribution, then finding the proportion of simulations where the proportion is equal to or more extreme than the observed point estimate: $11/12 pprox 0.9167$.

Performing this in a similar manner to the simulation demo shown previously, we get a p-value of very close to 0 and hence we reject the null hypothesis. There is strong evidence that the volunteers are better than just randomly guessing which of the 10 pictures was their own hand.

## Comparing two small sample proportions

Suppose we extend the experiment we performed in the previous example to also test whether volunteers can recognise the palm side of their hands. We get the following results:

| Outcome | Back | Palm | Total |
| :- | -: | -: | -: |
| Correct | 11 | 7 | 18 |
| Incorrect | 1 | 5 | 6 |
| Total | 12 | 12 | 24 |
| $\hat{p}$ | 0.9167 | 0.5833 | 0.7500 |

Do these data provide sufficient evidence that there is a difference in how good individuals are at recognising the backs of their hands vs. recognising the palm of their hands?

$H_0: p_{back} - p_{palm} = 0, H_A: p_{back} - p_{palm} 
eq 0$

Checking assumptions

- Independence
	- Within groups: we can assume that the guess of each volunteer is independent from the other volunteers' guesses
	- Between groups: we have the same volunteers guessing across two tests - this is in fact a paired structure. For the purposes of illustration we will continue with this example though.

- Sample Size/Skew
	- Since we're comparing between two population proportions here, we need to calculate number of successes and failures using the pooled proportion. $\hat{p}_{pool}$ is calculated in the table above as 0.75.
	- $12 	imes 0.75 = 9$, and $12 	imes 0.25 = 3$; neither of these meet the standard of 10, so CLT assumptions are not met - use simulation methods

Simulation scheme - the same principles as before apply, apart from the need to use the pooled proportion in this scheme

- Set up 24 index cards - each representing a volunteer and their two guesses across the two tests

- Using the pooled proportion - mark 18 cards as 'correct' and 6 cards as 'incorrect'; note that this is done *before* apportioning the cards into the two groups

- Apportion the 24 cards into two groups of 12 cards, representing back and palm tests

- Calculate the difference in proportion between correctness in back and palm test groups. Resimulate many times, each difference in proportion is another entry in your bootstrap distribution

Once your bootstrap distribution is constructed

- The point estimate of the difference is ~33% ($0.9167 - 0.5833$), so we want to find the proportion of simulations where $|\hat{p}| > 0.33$

- This comes out to around 16%, so we would fail to reject the null hypothesis and we say there isn't strong evidence to suggest that there is a difference between people recognising the palm of their hand vs. the back of it.

```r
simulate_mythbusters <- function(
	N, n = 12, 
	sample_space = c(rep(1, 18), rep(0, 6))
) {

	results <- vector('double', N)
	
	sapply(
		results,
		function(x) {
		
			idx <- sample(1:(2*n), n, replace = F)
		
			p_hat_back <- sample_space[idx] |> mean()
			p_hat_palm <- sample_space[-idx] |> mean()
			
			p_hat_back - p_hat_palm
		
		}
	)
}

results_mb <- simulate_mythbusters(100000)

tibble(p_hat_diff = results_mb) |>
	ggplot(aes(x = p_hat_diff)) +
	geom_histogram(bins = 30) +
	labs(
		title = 'Simulated p-hat difference between back and palms for this experiment',
		y = 'Count',
		x = 'Simulated p-hat(back) - p-hat(palm)'
	) +
	scale_y_continuous(labels = scales::comma) +
	scale_x_continuous(labels = scales::percent)
```
![[Pasted image 20230712230444.png]]
```r
point_est_diff <- 0.9167 - 0.5833 

scales::percent( 
	Filter(function(x) {abs(x) >= point_est_diff}, results_mb) |> length() / length(results_mb), 
	accuracy = 0.001 
)
#> '1.397%'
```

This is below the significance level so we can say we have strong evidence for the alternative hypothesis that there is indeed a difference in people's ability to pick out the back of their hand vs. their palm.

*Note: the course quotes a p-value of 15.66; unclear why this is so high and far off the value we get as they seem to do a substantial number of simulations as well*

## Chi-Square GOF test

Suppose the court of a small county is being accused of racial discrimination, with the claim being that the jury is not being picked in line with the ethnicity seen in the population of the county. See below the true proportion of each ethnicity within the county, as well as the number of individuals from each ethnicity seen in the jury. The expected row is the true population proportion multipled by 2500 which is the number of jurors picked.

| Population | White | Black | Native American | Asian | Other | Total |
| :- | -: | -: | -: | -: | -: | -: |
| Census | 80.29% | 12.06% | 0.79% | 2.92% | 3.94% | 100% |
| Jury | 1920 | 347 | 19 | 84 | 130 | 100% |
| # Expected | 2007 | 302 | 20 | 73 | 98 | 2500 |

Based on hypothesis testing principles, our null hypothesis would be that there is nothing going on (i.e. the distribution of jurors matches the distribution of the population); whereas our alternative hypothesis would be that the distributions are not the same.

Here we want to be able to quantify how different the observed counts are from the expected counts, large differences from the expected counts provide evidence to the alternative hypothesis

- This is called a goodness-of-fit (GOF) test since we're evaluating how well the observed distribution *fits* to the expected distribution.
- The name of this test is the Chi-Square ($