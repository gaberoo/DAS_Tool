# DAS Tool version 1.0

Database files are available here: http://banfieldlab.berkeley.edu/~csieber/db.zip

See installation instructions for more details.

## 1. Usage

``` 
DAS_Tool.sh -i methodA.scaffolds2bin,...,methodN.scaffolds2bin
            -l methodA,...,methodN -c contigs.fa -o myOutput

   -i, --bins                 Comma separated list of tab separated scaffolds to bin tables.
   -c, --contigs              Contigs in fasta format.
   -o, --outputbasename       Basename of output files.
   -l, --labels               Comma separated list of binning prediction names. (optional)
   --search_engine            Engine used for single copy gene identification [blast/diamond/usearch].
                              (default: usearch)
   --write_bin_evals          Write evaluation for each input bin set [0/1]. (default: 1)
   --create_plots             Create binning performance plots [0/1]. (default: 1)
   --write_bins               Export bins as fasta files  [0/1]. (default: 0)
   --proteins                 Predicted proteins in prodigal fasta format (>scaffoldID_geneNo).
                              Gene prediction step will be skipped if given. (optional)
   --score_threshold          Score threshold until selection algorithm will keep selecting bins [0..1].
                              (default: 0.1)
   -t, --threads              Number of threads to use. (default: 1)
   -v, --version              Print version number and exit.
   -h, --help                 Show this message.

``` 


### 1.1 Input file format
- Bins [\--bins, -i]: Tab separated files of scaffold-IDs and bin-IDs.
Scaffold to bin file example:
``` 
Scaffold_1	bin.01
Scaffold_8	bin.01
Scaffold_42	bin.02
Scaffold_49	bin.03
``` 
- Contigs [\--contigs, -c]: Assembled contigs in fasta format:
``` 
>Scaffold_1
ATCATCGTCCGCATCGACGAATTCGGCGAACGAGTACCCCTGACCATCTCCGATTA...
>Scaffold_2
GATCGTCACGCAGGCTATCGGAGCCTCGACCCGCAAGCTCTGCGCCTTGGAGCAGG...
``` 

- Proteins (optional) [\--proteins]: Predicted proteins in prodigal fasta format. Header contains scaffold-ID and gene number:
``` 
>Scaffold_1_1
MPRKNKKLPRHLLVIRTSAMGDVAMLPHALRALKEAYPEVKVTVATKSLFHPFFEG...
>Scaffold_1_2
MANKIPRVPVREQDPKVRATNFEEVCYGYNVEEATLEASRCLNCKNPRCVAACPVN...
```

### 1.2 Output files
- Summary of output bins including quality and completeness estimates (DASTool_summary.txt).
- Scaffold to bin file of output bins (DASTool_scaffolds2bin.txt).
- Quality and completeness estimates of input bin sets, if ```--write_bin_evals 1```  is set ([method].eval).
- Plots showing the amount of high quality bins and score distribution of bins per method, if ```--create_plots 1``` is set (DASTool_hqBins.pdf, DASTool_scores.pdf).
- Bins in fasta format if ```--write_bins 1``` is set (DASTool_bins).



### 1.3 Examples: Running DAS Tool on sample data.

**Example 1:**  Run DAS Tool on binning predictions of MetaBAT, MaxBin, CONCOCT and tetraESOMs. Output files will start with the prefix *DASToolRun1*:
``` 
$ ./DAS_Tool.sh -i sample_data/sample.human.gut_concoct_scaffolds2bin.tsv,
                   sample_data/sample.human.gut_maxbin2_scaffolds2bin.tsv,
                   sample_data/sample.human.gut_metabat_scaffolds2bin.tsv,
                   sample_data/sample.human.gut_tetraESOM_scaffolds2bin.tsv 
                -l concoct,maxbin,metabat,tetraESOM 
                -c sample_data/sample.human.gut_contigs.fa 
                -o sample_output/DASToolRun1
``` 

**Example 2:** Run DAS Tool again with different parameters. Use the proteins predicted in Example 1 to skip the gene prediction step, disable writing of bin evaluations, set the number of threads to 2 and score threshold to 0.6. Output files will start with the prefix *DASToolRun2*:
```
$ ./DAS_Tool.sh -i sample_data/sample.human.gut_concoct_scaffolds2bin.tsv,
                   sample_data/sample.human.gut_maxbin2_scaffolds2bin.tsv,
                   sample_data/sample.human.gut_metabat_scaffolds2bin.tsv,
                   sample_data/sample.human.gut_tetraESOM_scaffolds2bin.tsv 
                -l concoct,maxbin,metabat,tetraESOM 
                -c sample_data/sample.human.gut_contigs.fa 
                -o sample_output/DASToolRun2 
                --proteins sample_output/DASToolRun1_proteins.faa 
                --write_bin_evals 0 
                --threads 2 
                --score_threshold 0.6
```


# 2. Installation
## 2.1 Dependencies

- R (>= 3.2.3): https://www.r-project.org
- R-packages: data.table (>= 1.9.6), doMC (>= 1.3.4), ggplot2 (>= 2.1.0)
- ruby (>= v2.3.1): https://www.ruby-lang.org
- Pullseq (>= 1.0.2): https://github.com/bcthomas/pullseq
- Prodigal (>= 2.6.3): https://github.com/hyattpd/Prodigal
- One of the following search engines:
	- USEARCH (>= 8.1): http://www.drive5.com/usearch/download.html
	- DIAMOND (>= 0.8.24): https://github.com/bbuchfink/diamond
	- BLAST+ (>= 2.5.0): https://blast.ncbi.nlm.nih.gov/Blast.cgi


## 2.2 Installation

``` 
# Download and extract DASTool.zip archive:
unzip DAS_Tool.v1.0.zip
cd ./DAS_Tool.v1.0

# Install R-packages:
R CMD INSTALL ./package/DASTool_1.0.0.tar.gz

# Download latest SCG database:
wget http://banfieldlab.berkeley.edu/~csieber/db.zip
unzip db.zip

# Run DAS Tool:
./DAS_Tool.sh -h
``` 

For detailed instructions please read the documentation.
