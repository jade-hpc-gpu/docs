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

A consortium of UK universities, led by the University of Oxford,
has been awarded £5 million by the Engineering and Physical Sciences
Research Council (EPSRC) to  continue the world leading research enabled 
by the Joint Academic Data science Endeavour (JADE) service. This forms part of a
much larger investment by EPSRC in the UK’s regional Tier 2
high-performance computing facilities, which aim to bridge the gap
between institutional and national resources.

JADE is unique amongst the Tier 2 centres in being designed for the
needs of machine learning and related data science applications. There
has been huge growth in machine learning in the last 5 years, and JADE
was the first national facility to support this rapid development.
JADE will accelerate the research of world-leading machine learning groups
in universities across the UK and national centres such as The Alan Turing
Institute and Hartree Centre.

The system design exploits the capabilities of NVIDIA's DGX-MAX-Q Deep
Learning System which has eight of its Tesla V100 GPUs tightly
coupled by its high-speed NVlink interconnect. NVIDIA has clearly
established itself as the leader in massively-parallel computing for
deep neural networks, and the MAXQ runs optimized versions of many
standard machine learning software packages such as Caffe, TensorFlow,
Theano and Torch.

This system design is also ideal for a large number of molecular
dynamics applications and as such JADE will also provide a powerful resource
for molecular dynamics researchers within the UK [HECBioSim](https://www.hecbiosim.ac.uk) community.

JADE Hardware
=============

JADE hardware consists of:
  * 63 DGX-MAX-Q Nodes, each with 8 Nvidia V100 GPUs
  * 4 Login nodes
  * Mellanox EDR networking
  * 4TB of local SSD storage per node
  * Over 1PB of DDN AI400X storage (70TB of NVMe)

.. toctree::
   :maxdepth: -1
   :hidden:

   jade/index
   software/index
   cuda
   more_info
   troubleshooting
