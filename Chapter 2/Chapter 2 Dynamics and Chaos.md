## 2.2 Introduction to Dynamics

Dynamics

+ Definition

  + how their complex behaviors unfold

  + how things change over time

+ Different types
  + planetary dynamics
  + fluid dynamics
  + electrical dynamics
  + climate dynamics
  + crowd dynamics
  + population dynamics
  + financial dynamics
  + group dynamics
  + social dynamics
+ Dynamical systems theory
  + the branch of math
  + gives us a vocabulary and set of mathematical tools for describing dynamics
+ Brief history
  + Aristotle
  + Nicolaus Copernieus
  + Galileo Galilei
  + Isaac Newton(the founder of the modern science of dynamics)
  + Pierre-Simon Laplace(intellect[1])
  + Henri Poincaré(some system -> butterfly effect)

Chaos

+ One particular type of dynamics of a system
+ Defined as "sensitive dependence on initial condition"
+ Chaos in the nature
  + dripping faucets
  + electrical circuits
  + solar system orbits
  + weather and climate
  + brain activity(EEG)
  + heart activity(EKG)
  + computer networks
  + population growth and dynamics
  + financial data
+ Difference between chaos and randomness?
+ Notion of "deterministic chaos"



## 2.3 Iteration

Download [SimplePopulationGrowth.nlogo](https://complexityexplorer.s3.amazonaws.com/IntroToComplexity/Unit2/SimplePopulationGrowth_v_6.1.1.nlogo)



an example is population growth

![1](./1.gif)



set n as the population, which follows an exponential function
$$
n_{t}=birthrate^t
$$
linear system(no interactions among bunnies)
$$
n_{t+1}=birthrate*n_t
$$


Question 1: n4 = 81



## 2.4 Linear versus Nonlinear Systems

Download [LogisticModel.nlogo](https://complexityexplorer.s3.amazonaws.com/IntroToComplexity/Unit2/LogisticModel_v_6.1.1.nlogo)



linearity: the whole is the sum of the parts

example: 10 different runs of the initial population at 1 = an initial population at 10



a number of bunnies die due to overcrowding -> nonlinear interaction

Logistic Model(By Verhulst)
$$
n_{t+1}=(birthrate-deathrate)*[n_t-\frac{n_t^2}{maxpopulation}]
$$
number will be rounded off(real number)



stick at 25

![2](./2.gif)

why not reach capacity? $(25-625/50)*2=25$



whole is different from sum of the parts

## References

- [1] [A philosophical essay on probabilities](https://bayes.wustl.edu/Manual/laplace_A_philosophical_essay_on_probabilities.pdf)

