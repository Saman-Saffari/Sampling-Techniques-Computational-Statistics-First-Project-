# Sampling-Techniques-Computational-Statistics-First-Project-
In this project I explored different sampling methods using Python. This file is a jupyter notebook. It can be run here 
without any dependency issues:
https://colab.research.google.com/drive/1rJdBntUs8fOqxOi2sOjEpIHE_OXz_2un
## Libraries
Libraries required are: numpy,math,matplotlib,seaborn and statistics.
## Question 1
### a
the function binomial_sample(n,p,s) generates samples from binomial distrubition only using samples from uniform distribution. 


n: parameter of binomial distribution corresponding to number of trials\
p: parameter of binomial distribution corresponding to probability of success in each trial\
s: number of samples to be generated 


This function uses inversion method. The logic behind is generating a random number u from uniform distribution(0,1) (intuitively corresponding to a probability) then
using a loop finding for what value, P(X<value) exceeds u. The value should be returned. The values are generated with binomial distribution pdf.\
Histogram for 1000 samples:\
![image](https://github.com/user-attachments/assets/32bb8f43-02eb-47e0-b439-800d0863c2d6)
### b
In binomial_sample2(n,p,s), function parameters remain the same.\
In this function, I have taken advantage of the fact that the binomial distribution with parameters n & p is the sum of n Bernoulli distributions with parameter p
Proof:

<img src="https://github.com/user-attachments/assets/19e4443c-3821-4d2a-975a-cace00aa83ff" alt="image" width="200" height="150"/>

The simple combinatorial argument for this is that there are n choose y ways to have y successes out of n trials. The probability of each one occuring being p^y*(1-p)^(n-y)
(the probability of having exactly y successes and n-y failures given the probability of success in each trial is p.

In the innermost loop, for each of n iterations, inversion method for Bernoulli is implemented. This is done by generating a from U(0,1), returning 1 if u<p and 0 otherwise.\
These samples from Bernoulli are summed up to created a single Binomial sample. This is iterated s times to obtain a binomial sample of size s.
Histogram of a sample of size 1000 using this method:
![image](https://github.com/user-attachments/assets/bc7d6136-354e-442f-8ee4-2d9202240dde)

### c
In this section, with the help of central limit theorem. Mean of Binomial distribution is approximated.
The theory behind this approximation and confidence interval is as follows:
CLT states that the distribution of sample means, regardless of the main distribution is normal as long as the sample size is large enough.
We use this knowledge to find a confidence interval for the mean. Knowing that the distribution of mean is normal allows us to use z-scores to find the values that provide an
interval where the mean lays with 95% confidence (95% was my choice of confidence, for which the corresponding z score will be 1.96).

The values are: Estimated men +/- 1.96 * SampleStandardDeviation/sqrt(SampleSize)

To illustrate CLT is true I have plotted a histogram for the mean of 100 samples of size 100 to indicate it follows a normal distribution:

![image](https://github.com/user-attachments/assets/00a5adc8-0573-4068-9726-1527ddb4fad7)

## 2
The function I will elaborate on is poisson_sampler2 (though both work, this is the one that uses the hint in the project)


poisson_sampler2(parameter,number):

parameter: poisson distribution parameter (t in question)
number: number of samples to be generated

This function uses transformation method. Specifically, it takes advantage of its relation to exponential distribution.
using  this hint:

![image](https://github.com/user-attachments/assets/4d34e26c-14f1-4697-9d5d-645d9c9feddf)

What I did was sum exponentials with rate 1 until the sum exceeds the parameter. The exponentials were generated using inversion method. Then return the number of
terms in the sum. This intuitively corresponds to the number of arrivals/occurences till time=t in a poisson process that has rate 1. This is equal to the number of occurences
in unit time in a poisson process with rate/parameter t! 

histograms for 1000 samples was plotted:

![image](https://github.com/user-attachments/assets/7febe962-252d-450f-b007-02e9ee28eac4)

using the same theory as part 1c, mean and confidence bounds for different sample sizes of poisson distribution were calculated.

10:
sample mean=1.3
sample standard deviation=1.5670212364724212
95% Confidence Interval is 0.328750175175191 , 2.271249824824809

100:
sample mean=1.19
sample standard deviation=1.1073218214058713
95% Confidence Interval is 0.9729649230044491 , 1.4070350769955506

1000:
sample mean=1.045
sample standard deviation=1.0217724888736333
95% Confidence Interval is 0.9816698850193412 , 1.1083301149806588

10000:
sample mean=1.045
sample standard deviation=1.0217724888736333
95% Confidence Interval is 0.9816698850193412 , 1.1083301149806588

## 3
### a
In this part, rejection sampling is used to approximate the mixture of two guassian distributions.
mixture(x) function: returns the mixture value at x.

proposed(x):returns the value of proposed distribution at x.

The distribution I proposed was a normal distribution with mean=2. This was to insure that the maximum occurs at the argmax of the mixture function.
Afterwards, I set standard deviation to be 1. This was to insure that the distribution stays more heavy tail than the target function through the real line.

M was calculated=max of target/proposed. Which occurs at x=2.

rejection_sampler():
Has no argument\
Returns a sample of size 10000 and the acceptance rate.

How it works: Generates sample of size 10000 from proposed distribution. 
For each sample element x, accepts it if u<=(mixture(x)/proposed(x)*M) where u is generated from U(0,1).

Histogram:

![image](https://github.com/user-attachments/assets/fecb78be-685c-412e-98e2-17fa05270c49)


As a sanity check, I used a composition sampler which generates a sample of size 1200 (approximately the number of accepted samples from rejection sampling)/
This is its histogram which is very similar to that of the rejection sampler:

![image](https://github.com/user-attachments/assets/b47c321f-c1f9-4461-8b43-5a8f406eb0ef)


### b
exponential_sampler(parameter):\
Samples from exponential distribution. Has parameter (rate of exponential distribution) as input. Uses inversion method.

exp_rejection_sampler(a,parameter):\
parameter=rate of exponential for both the proposed and target distribution\
a= a in the question (how much it is moved along the x axis)\
returns sample of size 10000 and rate of acceptance for rejection sampler.

It is important to note that M=max(target/proposed) occurs at a and is equal to e^a using basic algebra.

The histogram of the samples generated using exponential_sampler has been plotted for a=3 and parameter=1:

![image](https://github.com/user-attachments/assets/a244febd-ddc7-44d3-ac46-8cbbf8a30d08)


as a increases, the efficiency/acceptance rate of the rejection sampler using proposed Y decreases exponentially. This is easy to see as Y has to be multiplied by
exponentially bigger M's to cover X and this leads to an exponentially bigger rejection area. 

An scatterplot for 20 different "a" between 0 and 3 has been plotted to illustrate this exponential decay. The y axis corresponds to acceptance rate:

![image](https://github.com/user-attachments/assets/18e3a4d7-812c-4a58-a0d0-994df24abb91)

