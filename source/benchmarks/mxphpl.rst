HPL-MxP rules and validation
----------------------------

The HPL Mixed Precision (HPL-MxP) benchmark is described at https://hpl-mxp.org/.

Different vendors provide optimized implementation of the HPL-MxP benchmarks and teams are encouraged to use these vendor-optimized implementations to get the maximum performance on their clusters.

Nvidia HPL-MxP implementation: https://docs.nvidia.com/nvidia-hpc-benchmarks/HPL_MxP_benchmark.html

AMD HPL-MxP implementation: https://www.amd.com/en/developer/resources/infinity-hub/hpl-mxp.html

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

There is no minimum run time associated with HPL-MxP.


Running HPL-MxP
~~~~~~~~~~~~~~~

HPL-MxP solves the same kind of dense, randomly generated linear system :math:`Ax = b` as HPL, but
performs the expensive LU factorization in low precision (e.g. FP16, BF16, or FP8) on the matrix
units of the accelerators. The low-precision solution is then refined to full FP64 accuracy using an
iterative solver, and the run must pass the same style of FP64 scaled residual check as HPL. The reported score is the HPL-MxP GFLOPS, computed by dividing
the FP64 operation count of the solve by the total time (including the iterative refinement).

HPL-MxP implementations are usually configured through command-line arguments rather 
than an ``HPL.dat`` file. The most important parameters mirror those of HPL:

- **N** is the problem size, i.e. the order of the matrix :math:`A`. Larger N gives better
  performance up to the limit of accelerator memory, so start with an N that fills most of the
  available GPU memory.
- **NB** is the block size.
- **NPROW** and **NPCOL** define the 2D process grid (the equivalent of P and Q in HPL).
  ``NPROW X NPCOL`` must equal the number of MPI ranks launched, which is normally one rank per GPU.

A minimal run that also captures the files required for submission looks like the following (shown
for certification attempt ``1`` using the NVIDIA implementation on two nodes with 4 GPUs each,
matching the 4x2 grid in the example output above):

.. code-block:: bash
   :caption: cert-1-hplmxp.run

   NUMBER=1
   ARGS="--n 258048 --nb 1024 --nprow 4 --npcol 2 --gpu-affinity 0:1:2:3"

   # Save input parameters
   echo "${ARGS}" > cert-${NUMBER}-hplmxp.input

   # Initial Timestamp
   echo `TZ=America/Chicago date` > cert-${NUMBER}-hplmxp.tstamps

   # Launch HPL-MxP with 8 MPI ranks (4 per node). Output Directed to results file
   mpirun -np 8 --hostfile ./hostfile ./hpl-mxp.sh ${ARGS} &> cert-${NUMBER}-hplmxp.rslts

   # Final Timestamp(note >> to append)
   echo `TZ=America/Chicago date` >> cert-${NUMBER}-hplmxp.tstamps


**Files to submit**

- HPL-MXP output saved to a file ``cert-{N}-hplmxp.rslts``
- HPL-MXP input parameters saved to a file ``cert-{N}-hplmxp.input``
- The script (or commands) used to run HPL-MXP saved to a file ``cert-{N}-hplmxp.run``
- HPL-MXP run timestamps saved to a file ``cert-{N}-hplmxp.tstamps``

Example submissions for each file are given below

.. tab-set::

   .. tab-item:: HPL-MxP output

      .. code-block:: text
         :caption: cert-{N}-hplmxp.rslts
         :linenos:

         [... full HPL-MxP log output; submit the complete, unedited file ...]

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

   .. tab-item:: HPL-MxP input

      .. code-block:: text
         :caption: cert-{N}-hplmxp.input
         :linenos:

         --n 258048 --nb 1024 --nprow 4 --npcol 2 --sloppy-type 2 --gpu-affinity 0:1:2:3

   .. tab-item:: Script

      .. code-block:: bash
         :caption: cert-{N}-hplmxp.run
         :linenos:

         #!/bin/bash
         NUMBER=1
         # HPL-MxP parameters. NPROW x NPCOL = 4 x 2 = 8 MPI ranks (one per GPU)
         ARGS="--n 258048 --nb 1024 --nprow 4 --npcol 2 --sloppy-type 2 --gpu-affinity 0:1:2:3"

         # Save input parameters
         echo "${ARGS}" > cert-${NUMBER}-hplmxp.input

         # Initial Timestamp
         echo `TZ=America/Chicago date` > cert-${NUMBER}-hplmxp.tstamps

         # Launch HPL-MxP with 8 MPI ranks (4 per node). Output Directed to results file
         mpirun -np 8 --hostfile ./hostfile ./hpl-mxp.sh ${ARGS} &> cert-${NUMBER}-hplmxp.rslts

         # Final Timestamp(note >> to append)
         echo `TZ=America/Chicago date` >> cert-${NUMBER}-hplmxp.tstamps

   .. tab-item:: Timestamps

      .. code-block:: text
         :caption: cert-{N}-hplmxp.tstamps
         :linenos:

         Mon Nov 16 10:05:12 CST 2026
         Mon Nov 16 10:06:03 CST 2026
