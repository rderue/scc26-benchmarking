HPL rules and validation
------------------------

The HPL benchmark is described at https://www.top500.org/project/linpack/.

When built from source, the HPL output shows the validity of the run on the following line:

.. code-block:: bash

   ||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=        0.0020152 ...... PASSED

The time output will look like the following for HPL:

.. code-block:: bash

   T/V                N    NB     P     Q               Time                 Gflops
   --------------------------------------------------------------------------------
   WR01C2L8      102144   192     8     8             514.92              1.380e+03

There is no minimum run time associated with HPL.


Running HPL
~~~~~~~~~~~

HPL (High-Performance Linpack) solves a dense, randomly generated system of linear equations
:math:`Ax = b` in double precision (FP64). The reported score is the sustained floating-point rate 
(Gflops) for the solve, and is the metric used to rank systems on the Top500 list.
HPL reads its parameters from a file named ``HPL.dat`` in the current working directory. A typical
``HPL.dat`` looks like the following:

.. code-block:: text
   :caption: HPL.dat
   :linenos:
   :emphasize-lines: 6,8,11,12

   HPLinpack benchmark input file
   Innovative Computing Laboratory, University of Tennessee
   HPL.out      output file name (if any)
   6            device out (6=stdout,7=stderr,file)
   1            # of problems sizes (N)
   102144       Ns
   1            # of NBs
   192          NBs
   0            PMAP process mapping (0=Row-,1=Column-major)
   1            # of process grids (P x Q)
   8            Ps
   8            Qs
   16.0         threshold
   1            # of panel fact
   0            PFACTs (0=left, 1=Crout, 2=Right)
   1            # of recursive stopping criterium
   8            NBMINs (>= 1)
   1            # of panels in recursion
   2            NDIVs
   1            # of recursive panel fact.
   1            RFACTs (0=left, 1=Crout, 2=Right)
   1            # of broadcast
   1            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
   1            # of lookahead depth
   0            DEPTHs (>=0)
   2            SWAP (0=bin-exch,1=long,2=mix)
   64           swapping threshold
   0            L1 in (0=transposed,1=no-transposed) form
   0            U  in (0=transposed,1=no-transposed) form
   1            Equilibration (0=no,1=yes)
   8            memory alignment in double (> 0)

The highlighted lines are the parameters that have the largest impact on performance and should be
tuned first:

- **N** is the problem size (i.e. the order of the matrix :math:`A`). The matrix occupies
  :math:`8N^2` bytes, so larger N gives better performance up to the limit of available memory. A
  common starting point is an N whose matrix fills roughly 80-90% of the total memory available to HPL.
- **NB** is the block size used for data distribution and computation. Good values depend on
  the hardware and math library.
- **P** and **Q** define the 2D process grid. ``P X Q`` must equal the number of MPI
  ranks launched.

With ``HPL.dat`` in the working directory, a minimal run that also captures the files required for
submission looks like the following (shown for certification attempt ``1`` on two nodes with 32 ranks
each, matching the 8x8 grid above):

.. code-block:: bash
   :caption: cert-1-hpl.run

   NUMBER=1
   # Save input file
   cp HPL.dat cert-${NUMBER}-hpl.input

   # Initial Timestamp
   echo `TZ=America/Chicago date` > cert-${NUMBER}-hpl.tstamps

   # Launch HPL P=8 x Q=8 = 64 MPI ranks. Output Directed to results file
   mpirun -np 64 --hostfile ./hostfile ./xhpl &> cert-${NUMBER}-hpl.rslts

   # Final Timestamp(note >> to append)
   echo `TZ=America/Chicago date` >> cert-${NUMBER}-hpl.tstamps


**Files to submit**

- HPL output saved to a file ``cert-{N}-hpl.rslts``
- HPL input file (``HPL.dat``) saved to a file ``cert-{N}-hpl.input``
- The script (or commands) used to run HPL saved to a file ``cert-{N}-hpl.run``
- HPL run timestamps saved to a file ``cert-{N}-hpl.tstamps``
- `Run configurations <https://scc26-benchmarking.readthedocs.io/en/latest/benchmarks/general.html#configuration-file-description>`_ saved to a file ``cert-{N}-configuration.txt``

Example submissions for each file are given below

.. tab-set::

   .. tab-item:: HPL output

      .. code-block:: text
         :caption: cert-{N}-hpl.rslts
         :linenos:

         ================================================================================
         HPLinpack 2.3  --  High-Performance Linpack benchmark  --   December 2, 2018
         Written by A. Petitet and R. Clint Whaley,  Innovative Computing Laboratory, UTK
         Modified by Piotr Luszczek, Innovative Computing Laboratory, UTK
         Modified by Julien Langou, University of Colorado Denver
         ================================================================================

         An explanation of the input/output parameters follows:
         T/V    : Wall time / encoded variant.
         N      : The order of the coefficient matrix A.
         NB     : The partitioning blocking factor.
         P      : The number of process rows.
         Q      : The number of process columns.
         Time   : Time in seconds to solve the linear system.
         Gflops : Rate of execution for solving the linear system.

         The following parameter values will be used:

         N      :  102144 
         NB     :     192 
         PMAP   : Row-major process mapping
         P      :       8 
         Q      :       8 
         PFACT  :    Left 
         NBMIN  :       8 
         NDIV   :       2 
         RFACT  :   Crout 
         BCAST  :  1ringM 
         DEPTH  :       0 
         SWAP   : Mix (threshold = 64)
         L1     : transposed form
         U      : transposed form
         EQUIL  : yes
         ALIGN  : 8 double precision words

         --------------------------------------------------------------------------------

         - The matrix A is randomly generated for each test.
         - The following scaled residual check will be computed:
               ||Ax-b||_oo / ( eps * ( || x ||_oo * || A ||_oo + || b ||_oo ) * N )
         - The relative machine precision (eps) is taken to be               1.110223e-16
         - Computational tests pass if scaled residuals are less than                16.0

         ================================================================================
         T/V                N    NB     P     Q               Time                 Gflops
         --------------------------------------------------------------------------------
         WR01C2L8      102144   192     8     8             514.92             1.3798e+03
         HPL_pdgesv() start time Mon Nov 16 09:12:04 2026

         HPL_pdgesv() end time   Mon Nov 16 09:20:39 2026

         --------------------------------------------------------------------------------
         ||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   2.01520000e-03 ...... PASSED
         ================================================================================

         Finished      1 tests with the following results:
                       1 tests completed and passed residual checks,
                       0 tests completed and failed residual checks,
                       0 tests skipped because of illegal input values.
         --------------------------------------------------------------------------------

         End of Tests.
         ================================================================================

   .. tab-item:: HPL input

      .. code-block:: text
         :caption: cert-{N}-hpl.input
         :linenos:

         HPLinpack benchmark input file
         Innovative Computing Laboratory, University of Tennessee
         HPL.out      output file name (if any)
         6            device out (6=stdout,7=stderr,file)
         1            # of problems sizes (N)
         102144       Ns
         1            # of NBs
         192          NBs
         0            PMAP process mapping (0=Row-,1=Column-major)
         1            # of process grids (P x Q)
         8            Ps
         8            Qs
         16.0         threshold
         1            # of panel fact
         0            PFACTs (0=left, 1=Crout, 2=Right)
         1            # of recursive stopping criterium
         8            NBMINs (>= 1)
         1            # of panels in recursion
         2            NDIVs
         1            # of recursive panel fact.
         1            RFACTs (0=left, 1=Crout, 2=Right)
         1            # of broadcast
         1            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
         1            # of lookahead depth
         0            DEPTHs (>=0)
         2            SWAP (0=bin-exch,1=long,2=mix)
         64           swapping threshold
         0            L1 in (0=transposed,1=no-transposed) form
         0            U  in (0=transposed,1=no-transposed) form
         1            Equilibration (0=no,1=yes)
         8            memory alignment in double (> 0)

   .. tab-item:: Script

      .. code-block:: bash
         :caption: cert-{N}-hpl.run
         :linenos:

         #!/bin/bash
         NUMBER=1
         # Save input file
         cp HPL.dat cert-${NUMBER}-hpl.input

         # Initial Timestamp
         echo `TZ=America/Chicago date` > cert-${NUMBER}-hpl.tstamps

         # Launch HPL P=8 x Q=8 = 64 MPI ranks. Output Directed to results file
         mpirun -np 64 --hostfile ./hostfile ./xhpl &> cert-${NUMBER}-hpl.rslts

         # Final Timestamp(note >> to append)
         echo `TZ=America/Chicago date` >> cert-${NUMBER}-hpl.tstamps

   .. tab-item:: Timestamps

      .. code-block:: text
         :caption: cert-{N}-hpl.tstamps
         :linenos:

         Mon Nov 16 09:12:01 CST 2026
         Mon Nov 16 09:20:44 CST 2026

   .. tab-item:: Configuration

      .. code-block:: text
         :caption: cert-{N}-configuration.txt
         :linenos:

         1) Cluster Name: Example-Cluster
         2) Model Name of the System: Custom-built 2U rack servers
         3) Vendor: Supermicro
         4) Number of compute nodes in the cluster: 2 nodes
         5) Compute node processor name and model: AMD EPYC 9124 16-Core Processor
         6) Sockets per compute node: 2
         7) Cores per socket: 16
         8) Processor speed: 3000 MHz
         9) Accelerator/Co-Processor: None (CPU-only HPL run)
         10) Number and type of accelerator(s)/co-processor(s) per node HPL was executed on: 0
         11) FP64 Cores Per Accelerator/Co-Processor that HPL was executed on: N/A
         12) System peak power used in watts, including networking equipment: 1450W
         13) Number of compute node cores that HPL was executed on: 32 cores per node (64 total)
         14) Primary Operating System and version: Rocky Linux 9.4
         15) Primary high speed network interconnect: NVIDIA InfiniBand NDR200
         16) Memory per Compute node: 256 GB
         17) Origin of HPL binary: Netlib HPL 2.3 source built by the team with Open MPI 5.0.3 and AMD AOCL-BLIS 4.2
