MLPerf rules and validation
---------------------------

The instructions to run the MLPerf Inference LLAMA2-70B benchmark and submit your results 
can be found in `this tutorial <https://docs.mlcommons.org/inference/benchmarks/language/scc25_guide/scc25/>`_. 

**IMPORTANT**
**Download the models ahead of time**

- Teams are **strongly encouraged** to download the quantized models before arriving at the competition. The modles can take up to 500GB stoarge and can take a long time to download.
- Teams using AMD GPUs can download the optimized models from Huggingface as described `here <https://github.com/mlcommons/inference_results_v5.1/tree/main/closed/AMD/measurements/8xMI300X_2xEPYC_9575F/llama2-70b-99.9/Offline>`_. 
- Teams using Nvidia GPUs can download the optimized models from the MLCommons server using a service account as described in the `tutorial <https://docs.mlcommons.org/inference/benchmarks/language/scc25_guide/scc25/>`_. 
- **Each team should submit the email address of two team members along with the team name to the SCC committee to receive the service account credentials.** Please submit the email addresses via this `Google Form <https://forms.gle/nQBgeeX8B7wpSExn6>`_.

**Results Submission**

MLPerf results must be uploaded to the MLPerf submission server as described in the `tutorial <https://docs.mlcommons.org/inference/benchmarks/language/scc25_guide/scc25/>`_.

**Things to remember**

  - Teams are encouraged to download the latest copy of the MLPerf Inference LLAMA2-70B codes. This will make sure that your runs incorporate latest features and bug fixes in the benchmark.
  - All improvements to the MLPerf codes must be made publicly available under the Apache 2.0 license and submitted as pull requests by November 10, 2025 (Mon) and only the code which is merge ready will be considered for evaluation.
  - Teams that make significant community contributions may be awarded bonus points at the discretion of the SCC25 committee. To receive bonus points, teams must 1) publicly share their code changes with the community by creating a pull request to one or more of the following repositories: ``mlcommons/mlperf-automations``, ``mlcommons/inference``, ``mlcommons/inference_results_v5.1``, at least 7 days before the start of the competition (Nov 10, 2025, 9:00AM CDT), 2) have their code changes reviewed by the MLCommons reviewer(s), 3) accept the Contributor License Agreement (CLA) to merge the PR into the repository.
