# RFX-LGCP
Random Effects Log Gaussian Cox Process Model for Neuroimaging Coordinate-Based Meta Analysis

This repository has code for the following paper:

Pantelis Samartsidis,
Claudia R. Eickhoff,
Simon B. Eickhoff
Tor D. Wager,
Lisa Feldman Barrett,
Shir Atzil,
Timothy D. Johnson,
Thomas E. Nichols (2018).
Bayesian log-Gaussian Cox process regression: applications to meta-analysis of neuroimaging working memory studies. _Journal of the Royal Statistical Society. Series C (Applied Statistics)_, _in press_.

# Reproducing the paper's results

To reproduce the results in our paper:

```console
make
./lgcp 7000 15000 5 50 40 15 > lgcp.log 2>&1 &
```
You will need a computer with GPU and have to [install CUDA](https://developer.nvidia.com/cuda-downloads).

You may need to issue a `disown` command.

# Manual

At present please consult the usage:
```
Usage: lgcp Burnin Iters Adjust AdjustWin Thin Save [GPU]

   Burnin  -- The burn-in period of the HMC
   Iters   -- The total number of iterations AFTER burn-in
   Adjust  -- How often to adjust the stepsize
   AdjustWindow
           -- Chain window when adjusting the stepsize
   Thin    -- How often to save the running sum of the GPs
   Save    -- How often to save snapshots of the GPs
   GPU     -- GPU device number (defaults to 0)

The following files are expected in the ./inputs directory:

   setup.txt: Contains following values, one per line:
       * Total number of elements in the initial grid. The program
         will figure out how many there are in the extended grid
       * Total number of points (foci)
       * Total number of point patterns (contrasts/studies)
       * Total number of covariates
       * Total number of spatially varying covariates
       * Total number of HMC leapfrog steps
       * Seed
       * HMC mass parameters (4 values), if one wants to see between-type
         comparisons

   seed.dat: 3 long integers

   rho.txt: Correlation decay parameters, one for each spatially varying
            covariate

   sigma.txt: Marginal standard deviations, one for each spatially varying
              covariate

   beta.txt: Overall mean parameter, one for each covariate

   gamma.txt: Standard normal variates, 144*192*144=3981312 for each spatially 
              varying covariate.  If missing, random numbers are generated.
              
   Z.txt: precomputed design matrix.
   
   counts.txt: The total number of foci per study
   
   foci.txt: The list of three-dimensional foci coordinates (after transformed into 
             voxel space) and the indices of study where they comes from (2107 foci 
             and 157 studies in total). 
   
   paper.txt: The publication identifier for each study (Studies from the same paper 
              appear consecutively).
   
   
The following files are created in the ./outputs directory:
   
   burnin.txt: A list of step size (eplison), marginal standard deviations (sigma), 
               correlation decay parameters (rho) and overall mean parameter (beta) 
               for each covariates, log-likelihood, Hamiltonian and iteration index 
               for each burnin iteration.
   
   rfx.txt: The updated random effects for 157 studies in each burnin iteration.
   
   hmc.txt: Same as `burnin.txt`, but for each HMC iteration.
   
   alpha.txt: Same as `rfx.txt`, but for each HMC iteration.
   
   starting.txt: Save a snapshot of beta for each covariates, sigma, rho and gamma 
                 for each spatially varying covariate every 500 iterations.

   gps/gp_*.txt: Save snapshots at every within-brain voxel in the extended grid for the GPs 
                 (The frequency is based on the parameter `Save`).
                 
   gp_summaries.txt: Save voxel-vise mean and variance for spatially varying covariates 
                     at every within-brain voxel in the extended grid for the GPs.
   
   
```
