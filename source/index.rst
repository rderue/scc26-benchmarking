.. You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. .. |project_caps| replace:: '%s' % project

2025 Student Cluster Competition (SCC25) benchmark submission instructions
==========================================================================

During the 2025 Student Cluster Competition (SCC25), each team is required to run three core benchmarks (HPL, HPL-MXP, and MLPerf Inference LLAMA2-70B). The benchmarks must be run during the benchmarking window of 9:00am--6:00pm CDT on Nov 17 (Mon), 2025, and must be run with the same hardware configuration. All benchmark results must be submitted together following the instructions given below. The benchmark results submission process is also referred to as "hardware certification" in the rest of the document.

Each team is allowed up to 5 hardware certification submission attempts. The SCC committee may at their discretion increase the allowable number of hardware certification submission attempts. Only one outstanding hardware certification submission attempt will be considered at a time.

Once a hardware certification has been submitted and verified, a team cannot modify their hardware configuration (for example, power on additional nodes or shutdown existing nodes) without invalidating their certification results. Such modification would require the team to run the three core benchmarks (HPL, HPL-MXP, and MLPerf Inference LLAMA2-70B) again in succession (any order) and recertify the results before the benchmarking deadline in order to have a valid result. Only the last valid hardware certification result will be used in benchmark scoring. This last certified hardware configuration will also be the hardware configuration used for the rest of the competition.


.. toctree::
   :maxdepth: 1
   :caption: Table of Contents:
   :numbered:
   :glob:

   benchmarks/general.rst
   benchmarks/hpl.rst
   benchmarks/mxphpl.rst
   benchmarks/mlperf.rst
   benchmarks/grading.rst
   benchmarks/faq.rst
   benchmarks/help.rst


.. Indices and tables
.. ==================

.. * :ref:`genindex`


