# Modelling Health Expenditure: Addressing potential endogeneity with the Cosslett correction 

For a project, we aimed to estimate the impact of health insurance on health expenditures amongst men. Since the dummy variable "being insured" may be subject to endogeneity (e.g. higher income typically influences both health expeditures and the decision to get insurance) and thus may be biased, we focused on the Cosslett (1993) method to correct for potential endogeneity. Our paper used data from the 2013 cohort of the The Medical Expenditure Panel Survey (MEPS), which can be derived from https://meps.ahrq.gov/mepsweb/. After cleaning the data, 8242 observations were used for the analysis.

## Modelling procedure
The underlying code estimates the model as follows:
Rather than modelling the choice of getting insurance $S_i$ as an exogenous variable alongside the other explanatory variables $x_i$ in the form of
$$EXP_{i}=\beta_{0}+x_{i}^{\prime} \beta+S_i \delta+\varepsilon_{i},$$
we assume that the dummy $S_i$ is endogenous, which gives rise to some form of a correction term must be added to estimate the individual health expenditures $EXP_i$. This can be done by using the Cosslett method resulting in

$$EXP_i=\beta_0+x_{i}^{\prime} \beta + S_i \delta+\theta g(z_{i}^{\prime} \gamma)+\varepsilon_{i}^{{\*}},$$

where $\varepsilon_{i}^{*}$ is now the corrected error term.
The correction term $g(z_{i}^{\prime} \gamma)$ takes the form of some general density-like function dependent on $z_{i}^{\prime} \gamma$. This function is defined separately for all observations where $S_{i} = 0$ and all observations where $S_{i} = 1$. For each situation, the domain of $g(z_{i}^{\prime} \gamma)$ is split into $K$ intervals, which are treated as bins. Dummies are created for each interval, and the dummy variable for insurance value $i\in \{0,1\}$ and bin $k\in\{1,...,K\}$ is given value 1 for an observation if it has insurance value $i$ and its value $z_{i}^{\prime} \gamma$ falls into bin $k$.
 
The $z_{i}^{\prime} \gamma$ is estimated using a probit regression of $z_i$ on $S_i$, where the $z_i$ vector consists of determinants of being insured. Estimating the probit model boils down to maximizing the joint log likelihood function
$$\ln \mathcal{L}(\gamma ; S, Z)=\sum_{i=1}^{n}\left(S_{i} \ln \Phi\left(z_{i}^{\prime} \gamma\right)+\left(1-S_{i}\right) \ln \left(1-\Phi\left(z_{i}^{\prime} \gamma\right)\right)\right),$$
which leaves us with an estimate $\hat{\gamma}$ which can be plugged into the correct $g(z_{i}^{\prime} \gamma)$ function depending on the value of $S_{i}$ in the full model.

Because of the complexity of the model and the functional form of $g$, bootstrapping is used to obtain standard errors of the parameters and perform inference. 100 paired bootstrap samples are generated by sampling N observations from the data, with repetition. The model is run on each sample, and all parameters are saved. To obtain the standard errors, and subsequently t-statistics and p-values, the standard error of the 100 bootstrap estimates is taken for each parameter.

By using the Cosslett method, instruments are used to estimate the effect that the instruments $Z$ have on the dependent variable ‘Insured’, as they are assumed to influence both the decision to take insurance and health care expenditure. The
ore, this method attempts to take away the endogeneity of the variable $S$ in the second stage regression to reduce the bias of the model and enhance the significance of the estimates.
