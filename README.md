# InflationDynamics
Using Ordinary Differential Equations (ODEs) to model inflation compared to monetary policy in the United States. This is our Volume 4 project, started fall 2023.

## Abstract

The labor market, including the unemployment rate and the amount of workers looking for jobs, can have a large impact on the economy.
    The more people employed means more money being spent, which in turn means more money being made. 
    Furthermore, rise in unemployment can lead to a recession. Being able to predict the labor market can help us prepare for a recession and help us understand the economy better.
    In this paper, we adapt an SIR model to characterize the dynamics of employed, unemployed, and retired individuals in the labor market. 
    Additionally, we employ a quasi predator-prey model to illustrate the oscillatory behavior observed in the white-collar and blue-collar industries. 
    By comparing the SIR model to the predator-prey model, we aim to enhance our understanding of the complex interactions within the labor market, 
    providing potential insights for recession prediction and economic analysis.

## Background/Motivation

One thing that is certain in life is that people will always need jobs. Not only this, but people 
will often lose their jobs. Furthermore, people will (eventually) retire from their jobs.
The focus of our project is modeling this situation.

In recent studies exploring the complexities of employment trajectories and occupational sectors, researchers 
have employed various modeling approaches such as Agent-Based Modeling~\cite{neves2019innovation}, and Markov Chains~\cite{zais2016markov}. However, 
in a departure from conventional methodologies, our investigation takes an innovative turn by adapting 
the SIR (Susceptible-Infectious-Recovered) model~\cite{kermack1927contribution}, typically utilized for studying disease dynamics, to the 
realm of employment dynamics. This unique application aims to unravel the intricate propagation of employment 
statuses, specifically delving into the transitions between being employed, unemployed, and retired.

A discernible trend has emerged in recent times, 
notably influenced by the technological revolution. The surge in interest and demand for tech-oriented careers 
has prompted a significant shift away from traditional blue-collar professions. This migration has led to a 
dual challenge: a scarcity of skilled workers in the blue-collar sector and an oversaturation of the tech 
industry~\cite{SHRMBlueCollarDrought}. To capture this relationship between white and blue collar jobs, we also create a quasi-predator-prey 
framework inspired by ecological models, which offers insights into the cyclical dynamics between these sectors. 

Motivated by the imperative to comprehend and address the consequences of this evolving employment landscape, 
our research aims to contribute valuable insights for informing strategic policies and industry interventions. 
By analyzing the strengths of both the predator-prey framework and the SIR model, we aspire to provide a 
comprehensive understanding of the intricate dynamics shaping the contemporary employment scenario.

## Modeling

### Theoretical Framework

The Susceptible-Infectious-Recovered (SIR) model, developed by Kermack and McKendrick in 1927~\cite{kermack1927contribution}, 
is a foundational mathematical framework for understanding the spread of infectious diseases in populations. 
It divides individuals into susceptible, infectious, and recovered compartments, capturing the dynamics of disease transmission.
In our model, we adapt the SIR model to represent the dynamics of the employment market through the labor force,
unemployed, and retired populations.

### Previous Work

We begin by building off the work of ElFadily et. al.~\cite{ElFadily}. In their work, ElFadily et. al. proposed a model
 representing the labor force and unemployed populations. They begin by defining their equations as

$$\frac{dL}{dt} = \gamma U - (\sigma + \mu)L, \quad \frac{dU}{dt} = \rho \left(1 - \frac{L_{\tau} + U_{\tau}}{N_c} \right)L_{\tau} + \sigma L - (\gamma + \mu)U,$$


where $L$ is the labor force, $U$ is the unemployed population, with initial conditions for (1) defined as:

$$L(0) > 0, \quad U(0) > 0,$$

$$(L(\theta),U(\theta)) = (\varphi_1(\theta), \varphi_2(\theta)), \quad \theta \in [-\tau,0],$$

where $\varphi_i\in C([-\tau, 0], \mathbb{R}^+),$  $i=1,2$.

The parameters are defined as follows:

* $\gamma$: employment rate
* $\sigma$: rate of job loss 
* $\mu$: mortality rate
* $\rho$: maximum population growth rate 
* $N_c$: population carrying capacity 
* $\tau$: time lag needed to contribute in the reproductive process of a new individual looking for a job

### Modifications: The Retirement Group

With this information in mind, we can begin to adapt this model to fit our desired model structure. We begin by adding a third population, the retired population, $R$.
We can then define our new equations as


$$\frac{dL}{dt} = \gamma U - (\sigma + \mu)L {\color{red} - \left(\frac{\Sigma}{L + U}\right) L + \omega\left(\frac{\Sigma}{L + U}\right) R + \rho L},$$

$$\frac{dU}{dt} = \rho\left(1 - \frac{L + U}{N_c} \right)L + \sigma L -(\mu + \gamma)U,$$

$$\frac{dR}{dt} = {\color{red}\left(\frac{\Sigma}{L + U}\right) L - \omega\left(\frac{\Sigma}{L + U}\right) R - \mu R},$$

which simplify to 

$$\frac{dL}{dt} = \gamma U - (\sigma + \mu)L {\color{red} + (\omega R - L)\left(\frac{\Sigma}{L + U}\right) + \rho L},$$
        
$$\frac{dU}{dt} = \rho\left(1 - \frac{L + U}{N_c} \right)L + \sigma L -(\mu + \gamma)U$$
        
$$\frac{dR}{dt} = {\color{red}(L - \omega R)\left(\frac{\Sigma}{L + U}\right) - \mu R}.$$

One of the first things to note from our equations is the removal of the time lag $\tau$. 
This is because, instead of factoring in people when they are born, we are instead factoring them
in when they turn of working age (for simplification, age 16). This reduces unnecessary complexity in our model. 
Additionally, we make two assumptions about unemployed people, being they will
neither retire directly from unemployment, nor contribute to the growth of the population. A final thing to note
is that we have added two new parameters, $\omega$ and $\Sigma$. We define $\Sigma$ to be the number of people who
retire each year in the United States, and $\omega$ to be the rate at which retired people enter back
into the full-time workforce (which is a dimensionless constant). We can then define our new initial conditions as:

$$L(0) > 0, \quad U(0) > 0, \quad R(0) > 0,$$
        
$$(L(\theta),U(\theta), R(\theta)) = (\varphi_1(\theta), \varphi_2(\theta), \varphi_3(\theta)), \quad \theta \in [-\tau,0],$$

where $\varphi_i\in C([-\tau, 0], \mathbb{R}^+),$ $i=1,2,3$. Incorporating nuanced dynamics into our model, we introduce the following terms and elucidate their significance within the equations


* $\pm\left(\frac{\Sigma}{L + U}\right) L$: Captures retirements relative to the total workforce, considering the annual number of retirees ($\Sigma$) as a percentage of the employed population ($L$).

* $\pm \omega\left(\frac{\Sigma}{L + U}\right) R$: Models retired individuals re-entering the workforce, with $\omega$ representing the transition rate.

* $\rho L$: Represents natural job growth, proportional to the employed population.

* $-\mu R$: Represents the natural attrition of retired individuals due to mortality at each time step.

### Modifications: The White-Collar and Blue-Collar Groups

We can further model the labor market by examining the relationship between two industries commonly referred to as the white-collar and blue-collar industries.
In recent years, we have seen an explosion of jobs and interest in the white-collar field, specifically in the tech industry, while the blue-collar industry has seen a decline in interest.
This has led to an oversaturation of the tech industry and a scarcity of labor in the blue-collar industry, which in turn has slowed growth in the former and led to increased growth in the latter.
This cyclical relationship mirrors that of a predator-prey relationship, and we can model it as such. 

Choosing a predator-prey model to represent blue-collar and white-collar jobs is justified by its ability to capture dynamic interactions and 
cyclic behavior between the two job categories. This modeling approach incorporates feedback loops, reflecting the influence each job type has 
on the other, and naturally represents population dynamics in response to economic, educational, or technological factors. 
Additionally, the model's adaptability allows for the inclusion of additional factors, offering flexibility in addressing 
the multifaceted dynamics of the labor market. This choice opens avenues for research and exploration of hypothetical scenarios, 
providing a structured framework to analyze and understand the interplay between different job categories over time.

The classical predator-prey model is given by the following equations:

$$ \frac{dx}{dt} = \rho x - a x y, \quad \frac{dy}{dt} = -\mu y + \varepsilon a x y.$$

where $x$ is the prey population, $y$ is the predator population, and $\rho$, $a$, $\mu$, and $\varepsilon$ are parameters~\cite{lotka1925elements}~\cite{volterra1926fluctuations}. 
We can adapt this model by defining the following:

$$\frac{dx}{dt} = \rho x {\color{red}\left(1-\frac{x}{k}\right)} - a x y, \quad \frac{dy}{dt} = -\mu y + \varepsilon a x y + {\color{red} \beta y \left(1-\frac{y}{C}\right)}.$$

where 

* $x$: Blue-collar jobs
* $y$: White-collar jobs
* $\rho$: Growth rate of blue-collar jobs
* $a$: Rate at which people switch from blue collar to white collar jobs
* $\mu$: Decay rate of white-collar jobs
* $\varepsilon$: The rate at which job transitioning affects the labor market
* $k$: Carrying capacity for blue-collar jobs
* $C$: Carrying capacity for white-collar jobs.
* $\beta$: Growth rate of white-collar jobs

Thus, we can interpret the new additions to our model as:

* $\rho x (1-\frac{x}{k})$: Carrying capacity term for the blue-collar (prey) population, reflecting growth proportional to the number of blue-collar workers.
* $\beta y (1-\frac{y}{C})$: Carrying capacity term for the white-collar (predator) population, constrained by the workforce size and capable of independent growth.

The reason that only white-collar jobs have a decay rate is meant to model the current market situation. The white-collar market is becoming
oversaturated so as it increases in population it decays, whereas the blue-collar market has not reached this point and never does with the initial conditions given.
We also made the assumption that the white-collar market converts whereas the blue-collar market does not. This was a choice to model the specific
circumstance we attempt to analyze. In the real world, this could go both ways with varying degrees, but for simplification we made it only one way.

### Labor Force, Unemployement, and Retirement Simulations


* $\sigma = 0.013905$: Derived from comprehensive data on total layoffs and discharges in the United States (2000-2023)~\cite{FRED}.
* $\rho = 0.014577$: Maximum growth rate calculated from MacroTrends Excel data (2000-2022)~\cite{MacroTrends}.
* $\gamma = 0.6062$: Employment rate average (2000-2022) from the Bureau of Labor Statistics~\cite{BLS}.
* $\mu = 0.008498$: Mortality rate derived from 2000-2022 mortality data~\cite{usafacts}.
* $N_c = 260,000,000$: Population of individuals aged 16 and above in the United States in 2022~\cite{kidscount}.
* $\Sigma = 775,045$: Annual retirees in the U.S. (2000-2021) using Social Security Administration data~\cite{ssa}, calculated by

$$\Sigma = \frac{1}{2021 - 2001}\sum_{i=2001}^{2021}(x_i - x_{i-1})$$

* $\omega = 0.063$: Rate at which retirees re-enter the workforce based on research by Maestas~\cite{maestas}.

We began testing our model by running it for 60 years with the current numbers for the United States (see figure~\ref{fig:results_lur_1}). 

\begin{figure}[h]
    \centering
    \includegraphics[width=.8\textwidth]{figures/results_lur_1.png}
    \caption{Initial conditions: $L(0) = 157,000,000$, $U(0) = 6,500,000$, $R(0) = 48,590,000$.}
    \label{fig:results_lur_1}
\end{figure}

To test the robustness of our model, we ran it with different initial conditions that do not represent the current situation in the United States. 
We first decreased the number in the labor force, increased the number of unemployed, and decreased the number of retired. We made these changes rather conservative
by only slightly perturbing the real numbers. We then ran the model for 60 years (see figure~\ref{fig:results_lur_2}).
Parallel to figure 1, we can see that the model still reaches an equilibrium despite the initial conditions being skewed from their true values.

<figure>
  <img src="blog_files/slice_surface_genus/Figure6.png" alt="Slice Surfaces." width="90%" height="90%" id="figure6">
  <figcaption text-align=center><b>Figure 6:</b> The left most part of this figure is the surface inside $\mathbb{R}^3$. 
                                By taking level sets of this surface, we can obtain an accurate representation of the surface.</figcaption>
</figure>
