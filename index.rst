JADE-HPC Facility User guide
############################

.. image:: images/Nvidia-DGX.jpg
   :width: 40%
   :align: right
   :alt: A picture showing some of the Jade hardware.

This is the documentation for the Joint Academic Data science Endeavour
(JADE) facility.

JADE is a UK Tier-2 resource, funded by EPSRC, owned by the University
of Oxford and hosted at the Hartree Centre. The hardware was supplied
and integrated by ATOS Bull.

A consortium of eight UK universities, led by the University of Oxford,
has been awarded £3 million by the Engineering and Physical Sciences
Research Council (EPSRC) to establish a new computing facility known as
the Joint Academic Data science Endeavour (JADE). This forms part of a
combined investment of £20m by EPSRC in the UK’s regional Tier 2
high-performance computing facilities, which aim to bridge the gap
between institutional and national resources.

JADE is unique amongst the Tier 2 centres in being designed for the
needs of machine learning and related data science applications. There
has been huge growth in machine learning in the last 5 years, and this
is the first national facility to support this rapid development, with
the university partners including the world-leading machine learning
groups in Oxford, Edinburgh, KCL, QMUL, Sheffield and UCL.

The system design exploits the capabilities of NVIDIA's DGX-1 Deep
Learning System which has eight of its newest Tesla V100 GPUs tightly
coupled by its high-speed NVlink interconnect. NVIDIA has clearly
established itself as the leader in massively-parallel computing for
deep neural networks, and the DGX-1 runs optimized versions of many
standard machine learning software packages such as Caffe, TensorFlow,
Theano and Torch.

This system design is also ideal for a large number of molecular
dynamics applications and so JADE will also provide a powerful resource
for molecular dynamics researchers at Bristol, Edinburgh, Oxford and
Southampton.

JADE Hardware
=============

JADE hardware consists of:
  * 22 DGX-1 Nodes, each with 8 Nvidia V100 GPUs
  * 2 Head nodes
  * Mellanox EDR networking
  * Over 1PB of Seagate ClusterStor storage

.. toctree::
   :maxdepth: -1
   :hidden:

   jade/index
   software/index
   cuda
   more_info
   troubleshooting
