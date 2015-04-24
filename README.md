# TestH

*TestH* is an ANSI C library implementing several pseudo-random number generators (PRNGs) for **self-similar sequences** and several **Hurst parameter** estimators (also known as the Hurst exponent) described in the literature. It is an on-going research and development project, whose main objective is to provide researchers with the means to estimate the Hurst parameter and to generate sequences with the self-similarity property embedded in the data. It represents an effort to integrate the different methods in a normalized and comprehensive manner in a single software package, which is not found in other tools.

*TestH* provides a friendly API that allows writing customized programs with ease and few programming skills. The library provides generic methods to instantiate processes (by means of any of the generators or by reading data files), compute statistical information, and submit the processes to estimators of the Hurst parameter while pretty-printing the output. 

*TestH* has been written for researchers and for research purposes. With that in mind, the library can be used in a wide array of research fields.

# Minimal Working Example

Below is a minimal working example showing a *fractional Brownian motion* (fBm) process being generated with a pre-defined target Hurst parameter, which is then transformed to a *fractional Gaussian noise* (fGn) process. Afterwards, the aggregate processes that are needed by the procedure to estimate the Hurst parameter are computed and finally the Variance Time (VT) method is called upon.

```
#include "process.h"
#include "generators.h"
#include "estimators.h"
#include "io.h"

// fBm-SGA
#define N		100000
#define H		(double) 0.80
#define S		16

int main (void) {
  TestHVerbosity = TestH_HIGH;

  proc_Process *proc = gen_fBmSequentialGenerationAlgorithm (N, H, S, TestH_fBm);
  proc_FractionalGaussianNoise (proc);
  
  proc_ScalesConfig *conf = proc_CreateScalesConfig (TestH_POW, 7, 11, 2);
  proc_CreateScales (proc, conf);
  proc_PrintProcess (proc);
  
  est_VarianceTime (proc);
  
  proc_DeleteProcess (proc);
  io_PrintDone ();
  return EXIT_SUCCESS;
}

```

# Dependencies and Usage

A `Makefile` is used to define dependencies and linkages of the library at compile time. The file `testh/src/test.c` provides several usage examples of the library and contains the `main` function of a program that can be modified as one sees fit. 

The object creation output and the binary output of the compilation, as defined in the `Makefile`, is respectively the following: 

* `testh/src/obj/`; and
* `testh/bin/test`.

The following command needs to be issued at a command line whose current working directory is `testh/src/` in order to compile and run the program:

* `make clean; make; make run`.

# References and Materials

For detailed information about *TestH*, please refer to the following publications:

* Diogo A. B. Fernandes, Miguel Neto, Liliana F. B. Soares, Mário M. Freire, and Pedro R. M. Inácio. [*A Tool for Estimating the Hurst Parameter and for Generating Self-Similar Sequences*](http://dl.acm.org/citation.cfm?id=2685657). In Proc. of the 46th Summer Computer Simulation Conference (SCSC) (part of the Summer Simulation Multi-Conference (SummerSim)), pages 1–8, Monterey, CA, USA, Jul. 6–10 2014. SCS. Acceptance ratio: 69%.
* Diogo A. B. Fernandes, Miguel Neto, Liliana F. B. Soares, Mário M. Freire, and Pedro R. M. Inácio. [*On the Self-Similarity of Traffic Generated by Network Traffic Simulators*](https://www.researchgate.net/publication/274953756_On_the_Self-Similarity_of_Traffic_Generated_by_Network_Traffic_Simulators). In Mohammad S. Obaidat, Faouzi Zarai, and Petros Nicopolitidis, editors, Modeling and Simulation of Computer Networks and Systems, pages 285–311. Elsevier, 2015.

For a more detailed insight about self-similarity and the Hurst Parameter, please consider de following references:

* Pedro R. M. Inácio, [*Study of the Impact of Intensive Attacks in the Self-Similarity Degree of the Network Traffic in Intra-Domain Aggregation Points*](http://www.di.ubi.pt/~mario/files/PhDThesis-PedroInacio.pdf), Ph.D. Thesis, Ph.D. in Computer Science and Engineering, Department of Computer Science, University of Beira Interior, Covilhã, December, 2009. 
* Pedro R. M. Inácio, Mário M. Freire, Manuela Pereira, and Paulo P. Monteiro, [*Fast Synthesis of Persistent Fractional Brownian Motion*](http://dl.acm.org/citation.cfm?id=2133395), ACM Transactions on Modelling and Computer Simulation (TOMACS), 22(2): Article 11, 21 pages, March 2012. ISSN 1049-3301. ISI Impact Factor (2011): 1.114.
* Pedro R. M. Inácio, Branka Lakic, Mário M. Freire, Manuela Pereira, and Paulo P. Monteiro, [*The Design and Evaluation of the Simple Self-Similar Sequences Generator*](http://www.sciencedirect.com/science/article/pii/S0020025509003429), Elsevier Information Sciences, Vol. 179, Issue 23, pp. 4029-4045, 2009. ISI Impact Factor (2009): 3.291.
