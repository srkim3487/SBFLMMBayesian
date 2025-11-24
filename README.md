This R package implements four binary response models that incorporate functional covariates and, optionally, spatial dependence and random effects:
(1) BFLM, a binary model based on generalized functional linear models (GFLM),
(2) SBFLM, a binary spatial model based on spatial GFLM,
(3) BFLMM, a binary functional linear mixed model, and
(4) SBFLMM, a spatial binary functional linear mixed model.

The methodology is based on:

Kim, S., Kaiser, M. S., and Berg, E., (2025+). Bayesian analysis of binary Markov random field models with functional covariates and random effects. Statistical Modeling

To install and load the package:
```r
devtools::install_github("srkim/SBFLMMBayesian")
library(SBFLMMBayesian)
```

Below is an example using the included simulated dataset.
```r
# Load example data included in the package
data(data_example)
X_list <- data_example$X_list
data_list <-data_example$data_list
nbd <- data_example$nbd
nbd_list <- data_example$nbd_list

X_mat <- Reduce("rbind", X_list)
data_vec <- unlist(data_list)

# Simulate time point on [0, 1]
t <- seq(from = 0, to = 1, length.out = ncol(X_mat))

# BFLM
res_BFLM <- posterior_samples_bflm(M=5000, initial=c(0.09, 0.8, -0.4, 1.3), proposal_var=0.009, autotune=FALSE, data=data_vec, X=X_mat, t=t, p=3, basis_type="fourier")

# SBFLM
initial_values <- c(0.4, 0.1, 0.9, -0.6, 1.4)
res_SBFLM <- posterior_samples_sbflm(M=8000, initial=initial_values, proposal_var=0.005, autotune=FALSE, M_auxz = 20, data=data_vec, nbd = nbd,X = X_mat, t=t, p=3, basis_type="fourier") 

# BFLMM
initial_values <- c(-4.6, 1.1, -0.9, 2.2, -1.4, 0.4, 0.0, 0.1, -2.5, 2.0, -0.2, -1.1, -0.3, 1.0, 0.2, 0.4, 1.4)
res_BFLMM <- posterior_samples_bflmm(M=10000, initial=initial_values, proposal_var_fixed=0.001, proposal_var_sigma2=1.1, proposal_var_rand=1, autotune=FALSE, prior_a=1, prior_b=1, M_auxz = 20, 
data_list=data_list, X_list = X_list, t=t, p=3, basis_type="fourier") 

# SBFLMM
initial_values <- c(0.5, -4.6, 1.1, -0.9, 2.2, -1.4, 0.4, 0.0, 0.1, -2.5, 2.0, -0.2, -1.1, -0.3, 1.0, 0.2, 0.4, 1.4)
res_SBFLMM <- posterior_samples_sbflmm(M=8000, initial=initial_values, proposal_var_fixed=0.001, proposal_var_sigma2=1.1, proposal_var_rand=1, autotune=FALSE, prior_a=1, prior_b=1, M_auxz = 20, 
data_list=data_list, nbd_list = nbd_list, X_list = X_list, t=t, p=3, basis_type="fourier") 
```

