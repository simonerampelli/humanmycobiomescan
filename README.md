# humanmycobiomescan
HumanMycobiomeScan allows the characterization of the fungal fraction of complex microbial ecosystems directly from reads of shotgun metagenomics.

Please see [SourceForge web page](https://sourceforge.net/projects/hmscan) for downloading ViromeScan.

Please refer to [this publication](https://doi.org/10.1186/s12864-019-5883-y) for further information.


## README

SPACE REQUIREMENTS : HMS requires about 65 GB of free space to work on your device

OTHER  REQUIREMENTS: HMS can be run on a regular desktop computer, but minimum 16 GB of RAM memory is required. We strongly suggest to run the tool on a cluster. To use the tool proficiently a basic knowledge of command-line usage is suggested.

### FIRST STEPS

After downloading the tool, in order to perform the analysis, you need to unzip the downloaded file and compile the database. 

1) UNTAR HMS
```
tar -zxvf HMS.tar.gz
```
### 2) MOVE THE DATABASE DIRECTORY IN THE HMS FOLDER
```
cd $HMS_path/HMS/database
```
### 3) UNZIP THE ZIPPED FILES
```
gzip -d Bacteria_custom/*
gzip -d  bowtie2/*
mv final_database bowtie2
gzip -d hg19/*
```
### 4) BUILD THE HUMAN DATABASE FOR THE FILTERING PROCEDURE
```
cd hg19/
```
Now you need to create the indexes database for running bmtagger. bmtagger and other bmtools necessary to run HMS are already present in the $PWD/HMS/tools folder. You can eventually download a compatible version of bmtools for your operating system at ftp://ftp.ncbi.nlm.nih.gov/pub/agarwala/bmtagger.

*N.B. Bmtagger scripts (bmfilter, srprism) require about 8.5Gb RAM memory and three times as much hard-disk space for index data.*
  
If you downloaded Bmtools, you will have to move bmtagger.sh, bmfilter, bmtool, extract_fullseq and srprism scripts in the directory $PWD/HMS/tools inside the HMS folder.*

Now you need to:
-  Make indexes for bmfilter. 
```
bmtool -d hg19reference.fa -o hg19reference.bitmask -A 0 -w 18
```
- Make index for srprism
```
srprism mkindex -i hg19reference.fa -o hg19reference.srprism -M 7168
```
- Make blastdb for blast
```
makeblastdb -in hg19reference.fa -dbtype nucl
```

### 5) CREATE A VIRTUAL LINK OF THE HMS.sh SCRIPT

On your command line type:
```
ln -s $HMS_path/HMS/hms.sh  $your_binary_folder/HMS
```

### 6) USAGE AND HELP 

For usage and help information please digit "HMS" without any option on your command line


### 7) EXPECTED OUTPUTS

The tool provides a series of files contained within the output folder. The tabular outputs report the raw and relativized values of the various fungal species detected. 
The same tables are also reported in a normalized form, based on the size of 10x millions of bases composing each reference genomes present in the database.


### 8) EXAMPLES OF USAGE

A) Computing the analysis on a single-end .fastq file for the eukaryotic fungi
```
hms.sh -p 3 -m /user/your-user-name/ -d fungi_LITE -1 /user/your-user-name/seqs/sample_A.fastq -o HMS_sample_A 
```
B) Computing the analysis on paired-end .fastq files for the eukaryotic fungi
```
hms.sh -p 3 -m /user/your-user-name/ -d fungi_LITE -1 /user/your-user-name/seqs/sample_A_r1.fastq -2 /user/your-user-name/seqs/sample_A_r2.fastq -o HMS_sample_A
```


