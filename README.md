# TERM-ddG
method for computing stability changes in proteins upon mutations, published in https://doi.org/10.1371/journal.pone.0178272

Author: Fan Zheng, Gevorg Grigoryan

This package is the source codes for a method predicting the effects of amino-acid point mutations on protein thermostability, reported in details in our paper:

**Sequence statistics of tertiary structural motifs reflect protein stability**, F. Zheng, G. Grigoryan, PLoS ONE, 12(5): e0178272, 2017

## Prerequisites

This package is written in both Python and MATLAB and works under UNIX/Linux environments. We tested our codes with Python 2.7 and MATLAB 2015b.

SciPy (https://www.scipy.org/), ProDy (http://prody.csb.pitt.edu/downloads/), and USEARCH8.0 (http://www.drive5.com/usearch/download.html) need to be installed to run the codes properly. Set path to USEARCH executable `PATH_usearch` in `modules/General.py` after obtaining it. Other tools from our previous work have been compiled and included in this package.

For the current version, we assume the users are familiar with, and have access to Sun Grid Engine (SGE) computing clusters. The commands for submitting computing jobs may vary from cluster to cluster. To adapt the commands to your specific cluster environment, go to `modules/Cluster.py` and make change under the function `qsub`.

## Get started

An example of program output can be found in `data_demo`. The following instructions will show how these results can be generated from scratch.

1. Prepare a PDB file for the protein to be predicted. For example, `1EY0.pdb`.

2. Prepare a list of point mutations. For example, `1EY0.s350.tab`. The file is a tab-delimited file, and it should conform the following format:  
  `PDB_id Chain_id Wild_type_aa Position Mutant_aa`  
  and the first column should agree with the name of the PDB file. For example, if the elements in the first column are `1EY0` then the PDB file should be named `1EY0.pdb`.

3. Run the following command:  
 `python mutationListIteration.py --l 1EY0.s350.tab --homof S2648.homo`
> We recommend to have a file to keep track of the PDB structures that have homologous relationships to the query protein. These PDB structures will be excluded from statistics of structure motif search. Otherwise, we believe the results will be biased. Please see the details in our paper. That is the purpose of the `--homof` flag. In `1EY0.homo` the PDB IDs following `1EY0_A` are excluded. Such results can be created from NCBI `blastpgb` program. The list here includes all PDB chains that are similar to `1EY0_A` at a cutoff e=1.0, which is consistent to our paper.

4. Upon finishing the previous command, run:  
 `python submitMatlab.py --l 1EY0.s350.tab --o 1ey0.dat`  
  and `1ey0.dat` will contain the results. A positive score indicates that the mutation is predicted as destabilizing.

5. We also provide the option to score all amino acid at a position. For that, simply run:  
 `python submitMatlab.py --l 1EY0.s350.tab --o 1ey0.dat.scan --scan`  
 and `1ey0.dat.scan` with show the score of 20 amino acids, with the wild-type amino acid specified in column 3 assigned as zeroes, and the other amino acids are relative to the wild-type amino acid.
> The step 5 is independent of step 4. However, if step 4 has been carried out, adding a flag `--oo` to the step 5 command will ultilize the existing results and give output much faster.

## Citations

If you use this tool in your research, please cite the following publications:

* Zheng, Fan and Gevorg Grigoryan. "Sequence statistics of tertiary structural motifs reflect protein stability", F. Zheng, G. Grigoryan, PLoS ONE, 12.5 (2017): e0178272.

* Zheng, Fan, Jian Zhang, and Gevorg Grigoryan. "Tertiary structural propensities reveal fundamental sequence/structure relationships." Structure 23.5 (2015): 961-971.

We also appreciate the authors of the following works that are utilized in this tool:

* Zhou, Jianfu, and Gevorg Grigoryan. "Rapid search for tertiary fragments reveals protein sequence–structure relationships." Protein Science 24.4 (2015): 508-524.

* Bakan, Ahmet, et al. "Evol and ProDy for bridging protein sequence evolution and structural dynamics." Bioinformatics 30.18 (2014): 2681-2683.

* Edgar, Robert C. "Search and clustering orders of magnitude faster than BLAST." Bioinformatics 26.19 (2010): 2460-2461.

