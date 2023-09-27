.. _alphafold:

Alphafold
=========

.. sidebar:: Alphafold

   :URL: https://github.com/google-deepmind/alphafold

`Alphafold <https://github.com/google-deepmind/alphafold>`__ is a Deep Learning model that can be used to predict the 3D structure of proteins. It is developed by `DeepMind <https://www.deepmind.com>`__ which is a subsidiary of `Alphabet <https://abc.xyz/>`__.





The Alphafold Module
--------------------

Alphafold is available to use as a module. Use the following command to load the latest version of Alphafold: ::

  module load cuda
  module load alphafold

This will load the Alphafold module and also CUDA driver to ensure GPU is used for accelerating the prediction.

.. note::

  This package is still undergoing testing. Please help report issues and slow predictions to the `service-now portal <https://stfc.service-now.com/hcssp>`__.


Genetic Database
----------------

AlphaFold needs multiple genetic (sequence) databases to run:

- `BFD <https://bfd.mmseqs.com/>`__
- `MGnify <https://www.ebi.ac.uk/metagenomics/>`__
- `PDB70 <http://wwwuser.gwdg.de/~compbiol/data/hhsuite/databases/hhsuite_dbs/>`__
- `PDB <https://www.rcsb.org/>`__ structures in the mmCIF format
- `PDB seqres <https://www.rcsb.org/>`__ – only for AlphaFold-Multimer
- `UniRef30 FKA UniClust30 <https://uniclust.mmseqs.com/>`__
- `UniProt <https://www.uniprot.org/uniprot/>`__ – only for AlphaFold-Multimer
- `UniRef90 <https://www.uniprot.org/help/uniref>`__

Which are stored on the flash drive and accessible to all users at: :: 

  /jmain02/flash/share/datasets/GeneticDB


Using Alphafold
---------------

Ensure the protein sequence (.fasta) you'd like to perform prediction on has been downloaded to your JADE home directory.

Once the Alphafold module has been loaded, the script ``run_alphafold.sh`` can be used to run the prediction. For example, if trying to run a sequence located at ``~/my_protein.fasta``:  ::

  run_alphafold.sh -d /jmain02/flash/share/datasets/GeneticDB -o predictions/ -f ~/my_protein.fasta  -t 2023-02-03 

Explanation of the parameters for the above command:

- **-d** - Location of the genetic databases, on JADE it is stored at ``/jmain02/flash/share/datasets/GeneticDB``.
- **-o** - Path to a directory that will store the results.
- **-f** - Path to a FASTA file containing sequence. If a FASTA file contains multiple sequences, then it will be folded as a multimer.
- **-t** - Maximum template release date to consider (ISO-8601 format - i.e. YYYY-MM-DD). Important if folding historical test sets.


Run ``run_alphafold.sh --help`` for explanations on all the parameters that can be used.

A folder of the protein file name is created inside the output directory e.g. ``predictions/my_protein/`` for this example. Explanations of outputs can be found at on the `official repository <https://github.com/google-deepmind/alphafold#alphafold-output>`__.






