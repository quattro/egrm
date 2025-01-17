Expected Genetic Relationship Matrix
========

Expected Genetic Relationship Matrix (EGRM) is the expected value of the Genetic Relationship Matrix (GRM) on unknown SNPs 
given the complete genealogical tree of a sample of individuals, derived under the coalescent theory framework.


Installation
------------

Install from PyPI (not available yet):

    pip install egrm

Or download the package and install from local:

    git clone https://github.com/Ephraim-usc/egrm.git
    
    pip install ./egrm


Command Line Tools
------------------
The software to compute the eGRM from tskit tree sequence output is handled by `trees2egrm`. Its usage is given by,

    usage: trees2egrm [-h] [--output OUTPUT] [--c_extension] [--skip_first_tree] [--run_var] [--genetic_map GENETIC_MAP] [--left LEFT] [--right RIGHT] [--rlim RLIM]
                      [--alim ALIM] [--output-format {gcta,numpy}]
                      input
    
    Construct eGRM matrix from tree sequence data
    
    positional arguments:
      input                 Path to ts-kit tree sequence file
    
    optional arguments:
      -h, --help            show this help message and exit
      --output OUTPUT, --o OUTPUT
                            output file prefix
      --c_extension, --c    acceleration by C extension
      --skip_first_tree, --sft
                            discard the first tree in the tree sequence
      --run_var, --var      compute varGRM instead of eGRM
      --genetic_map GENETIC_MAP, --map GENETIC_MAP
                            map file fullname
      --left LEFT, --l LEFT
                            leftmost genomic position to be included
      --right RIGHT, --r RIGHT
                            rightmost genomic position to be included
      --rlim RLIM           most recent time limit
      --alim ALIM           most ancient time limit
      --output-format {gcta,numpy}
                            Output format of eGRM
        trees2egrm [--input INPUT] [--output OUTPUT]
        
        trees2mtmrca [--input INPUT] [--output OUTPUT]

Where `input` is the tree sequence file prefix (so that the full name should be "INPUT.trees"), and `OUTPUT` is the output file prefix.

Optional parameters:

    [--c_extension] or [--c]

This specifies whether to use the C exntension model to accelerate the algorithm.
Usually this makes it ~10 times faster.
Recommended whenever the C environment is available.

    [--skip_first_tree] or [--sft]

This option skips the first tree in the tree sequence.
This is often useful because RELATE and some other tools always output tree sequences from 0bp, even when the genotype data starts from within the chromosome.

    [--run_var] or [--var]

This option is only for trees2egrm, not trees2mtmrca.
With this option turned on, the algorithm will output the varGRM in addition to eGRM, while roughly doubling the compuation time.

    [--genetic_map] or [--map]

A (comma/space/tab separated) three-column file with first column specifying the physical position in bp and the third column specifying the genetic position in cM. The second column is not used. The first line will always be ignored as the header.

    [--left LEFT] [--right RIGHT]

The leftmost and rightmost positions (in bp) between which the eGRM or mTMRCA is computed.

    [--rlim RLIM] [--alim ALIM]

This option is only for trees2egrm, not trees2mtmrca.
RLIM and ALIM are the most recent and most ancient times (in generations) between which the eGRM is computed.

The output of trees2egrm will be two (or three, if with --var option) files in numpy NPY format: 

-   OUTPUT.npy, which contains the eGRM matrix;

-   OUTPUT_mu.npy, which contains a single number of the measure of the tree sequence (i.e., the expected number of mutations on this tree sequence);

-   OUTPUT_var.npy, which contains the varGRM matrix, if the --var option is selected.

The output of trees2mtrmca will be two files in numpy NPY format: 

-   OUTPUT.npy, which contains the mTMRCA matrix;

-   OUTPUT_l.npy, which contains a single number of the number of base pairs of the tree sequence;


Python Functions
-----------------

    varGRM_C(trees)
    
    varGRM(trees)

The C and non-C versions of the eGRM algorithm. The input is a tskit TreeSequence object.
See the source code for a complete explanation of its parameters.

    mTMRCA_C(trees)
    
    mTMRCA(trees)

The C and non-C versions of the mTMRCA algorithm. The input is a tskit TreeSequence object.
See the source code for a complete explanation of its parameters.


Reproducing Results in the paper
-----------------

There is an additional commandline tool

    simulate 

which is included in the package, but not installed by default. You may manually run this script.

A complete explanation of its parameters and output files can be found at [reproduce.rst].


Support
-------

If you are having issues, please let us know.
Email the author: caoqifan@usc.edu

