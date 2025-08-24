---
title: "PHiLiP"
excerpt: "A parallel high-order library for PDEs using Discontinuous Galerkin methods<br/><img src='/images/KHI.gif'>"
collection: portfolio
date: 2025-08-24
---

This is a fork of PHiLiP, a parallel high-order library for PDEs using Discontinuous Galerkin method. In addition to the fork,
which contains a majority of my work in my thesis, I have contributed to the main repo several times. My focus on this package 
is to develop leading edge reduced order models (ROMs) and to ensure scalability of the program on HPCs.

[![GitHub](https://img.shields.io/badge/GitHub-tofstie/PHiLiP-green.svg)](https://github.com/tofstie/PHiLiP)
[![GitHub](https://img.shields.io/badge/GitHub-dougshidong/PHiLiP-green.svg)](https://github.com/tofstie/PHiLiP)
[![Documentation](https://img.shields.io/badge/Documentation-blue.svg)](https://codedocs.xyz/dougshidong/PHiLiP/)

## Contribution to dougshidong/PHiLiP
### [PR #283 - Remove Raw Pointers](https://github.com/dougshidong/PHiLiP/pull/283)
- Multiple raw pointers existed in the codebase and some were not cleaned up. This caused memory leaks in some tests
- Replaced the raw pointers with `std::unique_ptr`

### [PR #282 - Addressing Issue 223](https://github.com/dougshidong/PHiLiP/pull/282)
- Multiple tests were failing and causing OOM. This caused Ctest to be killed, resulting in the tests after these failing tests to not run
- Added "ulimit -v \$((2 * 1024 * 1024 * ${MPIMAX}))" to have a maximum of 2GB of virtual memory per core

### [PR #269 - Adding Labels to Ctest](https://github.com/dougshidong/PHiLiP/pull/269)
- Added labels to Ctests to ensure that certain tests can be run for testing functionality
- Removed outdated tests that do not contribute to ensuring functionality

### [PR #267 - Runge Kutta Base Class + Reduced Order Runge Kutta](https://github.com/dougshidong/PHiLiP/pull/267)
- Added a new Runge-Kutta base class to contain the basic logic of the Runga-Kutta methods.
- Modified existing Runge-Kutta solvers to fit with the new Runge-Kutta base class
- Added a new Runge-Kutta method for solving equations with a Galerkin ROM.
- Added Functionality for making unsteady ROMs as well as steady ROMs
- Added a new test to test for the unsteady ROM
- Fixed [Issue #266](https://github.com/dougshidong/PHiLiP/issues/266)
- Fixed [Issue #271](https://github.com/dougshidong/PHiLiP/issues/271)

### [PR #263 - Multicore Non-Negative Least Squares Code and Tests](https://github.com/dougshidong/PHiLiP/pull/263)
- The Non-Negative Least Squares solver originally worked on one core, causing all hyperreduction cases to be run on a single core
- This PR modified the NNLS_solver.cpp file to map a distributed Matrix onto a single core to run the NNLS algorithm
- The result will then be redistributed to multiple cores.
- Added tests to ensure that the NNLS_solver is giving correct results on multiple cores

## Branches in tofstie/PHiLiP

### RRKROM
This branch implements the relaxation techniques for the ESROM. Some small changes were required to recover the RHS and
stage solutions to calculate the relaxation factor. 

### quadHyperReduction
This branch implements the hyperreduction techniques using the quadrature POD basis. 

## Future Work

### Merging into dougshidong/PHiLiP
- As I wrap up my thesis, a few additional things need to be merged into the main branch of dougshidong/PHiLiP. 
Not all of my work will be merged in as there are large scale consequences of the work I have done on others in the group.

### Hyperreduction and FR
- I want to implement hyperreduction and FR to allow for more dissipation in the flow. There are a few things that need to
be solved first mathematically before this can be implemented.
