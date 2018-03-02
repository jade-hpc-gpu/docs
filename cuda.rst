.. _cuda:

CUDA
====

.. sidebar:: CUDA

   :URL: http://www.nvidia.co.uk/object/cuda-parallel-computing-uk.html

CUDA is a parallel computing platform and API model created and developed by Nvidia, which enables dramatic increases in computing performance by harnessing the power of GPUs


Versions
--------
Multiple CUDA versions are available through the module system. Version 9.0 is the current usable version following upgrade to DGX-1 Server 3.1.4. Upgrades to CUDA will be released as they become Generally Available.


Environment
-----------
The CUDA environment is managed through the modules, which set all the environment variables needed.  The availability of different versions can be checked with ::

  module avail cuda

The environment set by a particular module can be inspected, *e.g.* ::

  module show cuda/8.0


Learn more
----------
To learn more about CUDA programming, either talk to your local RSE
support, or visit Mike Giles' CUDA Programming course page at

http://people.maths.ox.ac.uk/gilesm/cuda/

This one-week course is taught in Oxford at the end of July each year,
but all of the lecture notes and practicals are provided online for
self-study at other times.

Official CUDA documentation
---------------------------

NVIDIA provides lots of documentation, both online and in downloadable form:

* `Online CUDA documentation <http://docs.nvidia.com/cuda/index.html>`_
* `CUDA homepage <http://www.nvidia.com/object/cuda_home.html>`_
* `CUDA Runtime API <http://docs.nvidia.com/cuda/pdf/CUDA_Runtime_API.pdf>`_
* `CUDA C Best Practices Guide <http://docs.nvidia.com/cuda/pdf/CUDA_C_Best_Practices_Guide.pdf>`_
* `CUDA Compiler Driver NVCC <http://docs.nvidia.com/cuda/pdf/CUDA_Compiler_Driver_NVCC.pdf>`_
* `CUDA Visual Profiler <http://docs.nvidia.com/cuda/pdf/CUDA_Profiler_Users_Guide.pdf>`_
* `CUDA-gdb debugger <http://docs.nvidia.com/cuda/pdf/CUDA_GDB.pdf>`_
* `CUDA-memcheck memory checker <http://docs.nvidia.com/cuda/pdf/CUDA_Memcheck.pdf>`_
* `CUDA maths library <http://docs.nvidia.com/pdf/CUDA_Math_API.pdf>`_
* `CUBLAS library <http://docs.nvidia.com/cuda/pdf/CUDA_CUBLAS_Users_Guide.pdf>`_
* `CUFFT library <http://docs.nvidia.com/cuda/pdf/CUDA_CUFFT_Users_Guide.pdf>`_
* `CUSPARSE library <http://docs.nvidia.com/cuda/pdf/CUDA_CUSPARSE_Users_Guide.pdf>`_
* `CURAND library <http://docs.nvidia.com/cuda/pdf/CURAND_Library.pdf>`_
* `NCCL multi-GPU communications library <https://developer.nvidia.com/nccl>`_
* `NVIDIA blog article <https://devblogs.nvidia.com/parallelforall/fast-multi-gpu-collectives-nccl/>`_
* `GTC 2015 presentation on NCCL <http://images.nvidia.com/events/sc15/pdfs/NCCL-Woolley.pdf>`_
* `PTX (low-level instructions) <http://docs.nvidia.com/cuda/pdf/ptx_isa_4.1.pdf>`_


Nsight is NVIDIA's integrated development environment:

* `Nsight Visual Studio <https://developer.nvidia.com/nvidia-nsight-visual-studio-edition>`_
* `Nsight Eclipse <https://developer.nvidia.com/nsight-eclipse-edition>`_
* `Nsight Eclipse -- Getting Started <http://docs.nvidia.com/cuda/nsight-eclipse-edition-getting-started-guide/index.html>`_


NVIDIA also provide helpful guides on the Pascal architecture:

* `Pascal Tuning Guide <http://docs.nvidia.com/cuda/pascal-tuning-guide/>`_
* `Pascal P100 White Paper <https://images.nvidia.com/content/pdf/tesla/whitepaper/pascal-architecture-whitepaper.pdf>`_


Useful presentations at NVIDIA's 2017 GTC conference contain:

* `Cooperative Groups <http://on-demand.gputechconf.com/gtc/2017/presentation/s7622-Kyrylo-perelygin-robust-and-scalable-cuda.pdf>`_
* `NCCL 2.0 <http://on-demand.gputechconf.com/gtc/2017/presentation/s7155-jeaugey-nccl.pdf>`_
* `Multi-GPU Programming <http://on-demand.gputechconf.com/gtc/2017/presentation/s7142-jiri-kraus-multi-gpu-programming-models.pdf>`_
* `The Making of Saturn-V <http://on-demand.gputechconf.com/gtc/2017/presentation/s7750-louis-capps-making-of-dgx-saturnv.pdf>`_
