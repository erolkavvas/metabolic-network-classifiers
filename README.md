[![MIT][image-1]][1]

# Metabolic Allele Classifiers

Metabolic Allele Classifiers (MACs) are flux balance analysis-based genome classifiers that achieve prediction accuracy on par with state-of-the-art machine learning approaches while enabling a mechanistic interpretation of the genotype-phenotype map. MACs thus provide a FBA framework for microbial genome-wide association studies (GWAS). Inference and computation of MACs utilize cobrascape (https://github.com/erolkavvas/cobrascape), which wraps around COBRApy package (https://github.com/opencobra/cobrapy) to model a population of strain-specific genome-scale models (GEMs) at allelic resolution.

![cobrafig][image-2]

## Estimating a Metabolic Allele Classifier

1. Run `$ python 01_sample_macs.py -f INPUT_DIR -s NUM_SAMPLES -o MAC_DIR [optional parameters...]`
	- INPUT_DIR: 
	- NUM_SAMPLES: Recommend \>1000 samples
2. Run `$ python 02_eval_macs.py -f MAC_DIR [--testset --bicthresh]`
	- Optimize the MAC for the training set or testset.

`01_sample_macs.py` generates MAC samples, which requires performing the computationally expensive flux variability analysis (popFVA). `02_eval_macs.py` solves the MAC for the training set or testset and saves the MAC solutions. An ensemble of sampled MACs can be generated by running steps 2 and 3 multiple times for the same output directory. The input parameters must be consistent for the same output directory. This is helpful because generating samples takes a long time so the simulations are likely to be interrupted at some point.

## Example run

`$ cd metabolic-allele-classifiers/`  

`$ python 01_sample_macs.py -f input_data -s 2 -o mnc_ensemble_0 --testsize 0.9`  

`$ python 02_eval_macs.py -f mnc_ensemble_0`  

## Analyzing Metabolic Allele Classifiers
Once a large number of MACs has been generated (\~1000+), those with $\deltaBIC\<10$ can be analyzed to study the genetic and metabolic basis for the GWAS dataset.
1. See `analyze_macs.ipynb`

## Input Data
- The following files are to be placed in INPUT\_DIR. The files should be named exactly as described below.
	- `MODEL_FILE.json` (**REQUIRED**). A genome-scale model in json format
	- `X_ALLELES_FILE.csv` (**REQUIRED**). A strain allele matrix with shape (strains, alleles). Alleles (columns) should have the same name as the gene ids in the genome-scale model (with \_num appended to specify the allele id) 
	- `Y_PHENOTYPES_FILE.csv` (**REQUIRED**). A strain phenotypes matrix with shape (strains, phenotypes)
	- `GENE_LIST_FILE.csv` (_OPTIONAL_). A pandas series or dataframe with index specifying the list of genes to limit the modeled alleles to. Highly recommended \<200 genes. otherwise sample deeply  
-  `cobra_model/gene_to_pathways.json`. JSON object describing mapping of genes to pathways for the organism. Required for pathway enrichments.
- `gene_to_name.json`. JSON object describing mapping of gene ids to names. OPTIONAL.

Strain allele matrix and strain phenotypes matrix from (https://rdcu.be/9rHj) and cobra model from (https://rdcu.be/bG6JO).

## Installation
	$ git clone https://github.com/erolkavvas/metabolic-allele-classifiers.git


[1]:	https://github.com/erolkavvas/escher/blob/master/LICENSE

[image-1]:	https://img.shields.io/pypi/l/Escher.svg
[image-2]:	/MAC_overview.png?raw=true