# eQTL-AbO-Conditional-Analysis

<img src="https://github.com/maggiebr0wn/eQTL-AbO-Conditional-Analysis/blob/main/lzplot_visual.png" width="1000" height="400">

Expression quantitative trait locus (eQTL) detection has become increasingly important for understanding how non-coding variants contribute to disease susceptibility and complex traits. The major challenge in eQTL fine-mapping and causal variant discovery relate to the impact of linkage disequilibrium on signals due to one or multiple functional variants that lie within a credible interval set. We contrast eQTL fine-mapping using the all-but-one approach, which conditions each signal on all others detected in a set, with results from forward stepwise conditional analysis. The objective of this pipeline is to perform all-but-one conditional analysis by isolating sequential eQTL peaks and reducing effects of linkage disequilibirum (LD).

**Please see our publication:** 

Margaret Brown, Emily Greenwood, Biao Zeng, Joseph E Powell, Greg Gibson, Effect of all-but-one conditional analysis for eQTL isolation in peripheral blood, Genetics, Volume 223, Issue 1, January 2023, iyac162, https://doi.org/10.1093/genetics/iyac162

----------
OBJECTIVE:
----------
The objective of this pipeline is to perform "all-but-one" conditional analysis by isolating sequential eQTL peaks and reducing effects of linkage disequilibirum (LD).

<img src="https://github.com/mbr0wn1995/eQTL-AbO-Conditional-Analysis/blob/main/fsw_vs_abo_visual.jpg" width="800" height="450">

-------------------
STEPS & HOW TO RUN:
-------------------

I.) Input:

	Genotype data for each gene of interest in the form of PLINK files located in a "genotype" directory. Any phenotype data must be called "phenotype_for_chosen_genes" and structured as:
	
	chr
	gene
	probe/misc
	quantitative phenotypic value
	
	These scripts are required to run the pipeline, along with PLINK. PLINKv1.9 is incorporated in this repository or can also be accessed and downloaded here: https://www.cog-genomics.org/plink/
	
	i.) plink_wrapper.sh
	ii.) extract_phenotypes.sh
	iii.) transpose.sh
	iv.) run_plink.sh
	v.) csv_plots.sh
	vi.) LZ_overlay_plots.R --> requires R package libraries "readr" and "stringi"
	vii.) csv_plots_No_Cond_Analysis.sh

II.) Automated Steps (Pseudocode):

	For each gene listed in the "phenotype_for_chosen_genes" file:
		Generate a new directory
		copy the genotype data into this directory
		
		For each probe associated with this gene
			create a PLINK compatible phenotype file by subsetting the "phenotype_for_chosen_genes" file
			perform stepwise linear association analysis with PLINK, noting each eSNP meeting significance threshold
			
			For each eQTL peak discovered
				use the forward stepwise "eSNP list" to perform "All but One" conditional analysis, noting any potential eSNP disagreements
				compute LD R^2 between the eSNP versus all other SNPs in this region
				generate custom locuszoom-like plots

III.) Running the script:

	i.) version 1: run automated pipeline to all genes in "phenotype_for_chosen_genes" file:

		./plink_wrapper.sh -c
	
	ii.) version 2: run pipeline for ONE gene in "phenotype_for_chosen_genes" file (all probes for this gene run automatically):
		
		Example for CISD1:
			./extract_phenotypes.sh -a CISD1
			./run_plink.sh -a CISD1
			./csv_plots.sh -a CISD1 -c  

IV.) Plotting Forward Stepwise locuszoom-like plots (NOT AbO conditional analysis locuszoom-like plots)

	Example for CISD1:
		./csv_plots_No_Cond_Analysis.sh -a CISD1

----------------------
GEORGIA TECH EQTL HUB: 
----------------------

Results for 165 IBD genes that had one or more signficant eQTL after AbO isolation are avaliable at the Georgia Tech eQTL Hub. Raw data and summary statistics for each gene and credible set are also available. Please note, the raw data includes all individuals within the CAGE cohort, while the analysis was conducted after removing related individuals.
Please refer to this link to access the Georgia Tech eQTL Hub: https://eqtlhub-gt.shinyapps.io/shiny/
