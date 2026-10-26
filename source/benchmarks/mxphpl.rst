HPL-MXP rules and validation
------------------------

The HPL Mixed Precision (HPL-MXP) benchmark is described at https://hpl-mxp.org/.

Different vendors provide optimized implementation of the HPL-MXP benchmarks and teams are encouraged to use these vendor-optimized implementations to get the maximum performance on their clusters.

Nvidia HPL-MXP implementation: https://docs.nvidia.com/nvidia-hpc-benchmarks/HPL_MxP_benchmark.html

AMD HPL-MXP implementation: https://www.amd.com/en/developer/resources/infinity-hub/hpl-mxp.html

An example of the HPL-MXP result output is shown below:

.. code-block:: bash

  ****** HPL MxP Result    ****** 

    EPS           . . . . . . . . . . . . . . . . .          =    2.000000E-16
    Threshold     . . . . . . . . . . . . . . . . .          =    1.600000E+01
  ||Ax-b||_oo     . . . . . . . . . . . . . . . . .          =    8.737455E-14
  ||A   ||_oo     . . . . . . . . . . . . . . . . .          =    2.594294E+05
  ||x   ||_oo     . . . . . . . . . . . . . . . . .          =    7.799867E-06
  ||b   ||_oo     . . . . . . . . . . . . . . . . .          =    9.999990E-01
  ||Ax-b||_oo / (EPS * (||A||_oo * ||x||_oo + ||b||_oo) * N) =    5.599413E-04 ...... PASSED

    N = 258048, NB = 1024, NPROW = 4, NPCOL = 2, SLOPPY-TYPE = 2
       GFLOPS = 4.3468e+05, per GPU =   54335.58 ------ The HPL-MxP performance to report
    LU GFLOPS = 2.4940e+06, per GPU =  311748.98 ------ The performance excluding the iterative solver part

 ****** HPL MxP Result    ******

The residual that is used to assess the validity of the run is given on this line:

.. code-block:: bash
  
  ||Ax-b||_oo / (EPS * (||A||_oo * ||x||_oo + ||b||_oo) * N) =    5.599413E-04 ...... PASSED


**Files to submit**

- HPL-MXP output saved to a file ``cert-{N}-mxphpl.rslts``
- HPL-MXP input parameters saved to a file ``cert-{N}-mxphpl.input``
- The script (or commands) used to run HPL-MXP saved to a file ``cert-{N}-mxphpl.run``
- HPL-MXP run timestamps saved to a file ``cert-{N}-mxphpl.tstamps``

