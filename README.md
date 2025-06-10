# MyGenome
Project for ABT 480-001

To preface, I completed the majority of this project retroactively, so certain methods vary between myself and other member of this project. 

1. Retrieiving relevant data sets
   As opposed to downloading the files onto my personal, UK VM to begin trimming the data, I copied them directly onto the super computer: 
   ssh jwst256@mcc.uky.edu
   scp ngs@10.163.183.71:Desktop/Po9/Po9* .

   Renamed Files:
   mv Po9_CKDL240039127-1A_22HTV5LT4_L2_1.fq.gz UFVPY185_1.fq.gz
   mv Po9_CKDL240039127-1A_22HTV5LT4_L2_2.fq.gz UFVPY185_2.fq.gz

2. Pre-trimming statistics
   INSERT PHOTO OF PRE TRIMMED DATA STATS

3. Use Velvet Optimiser to determine optimal k value to begin assembling and trimming
   The following command was run in order to retrieve the number of reads in the data set, in order to begin trimming and assembling
   grep -c "^@" UFVPY185_1_paired.fastq
   This yielded 6611873 reads.

   This number was entered into velvet advisor, along with the given base pair length of 140, estimated genome size of 45 megabases, and the 20 fold k-mer coverage.

   Given this information, velvet advisor reccomended I start with a k value of 73. 
    
   
4. Trim Data
   To trim the data, I used trim-velvet.sh, with the k value range surrounding the optimal kmer value by 50 on both sides for a total range of 100 surrounding the kmer value of 73.
   The step sized used was 10 steps. 

   The following command was run with the intent to both trim and assemble the genome in the same step; however, the assembly portion of this script did not work as expected, so this      was done in a later step:
   sbatch trim-velvet.sh . UFVPY185 23 123 10 yes

   This command did successfully trim the data, though, yielding in the 2 sets of paired data and 2 sets of unpaired data. 

5. Assembling the genome
   Still on the supercomputer, I copied over the slurm script, velvet-optimiser.sh, in order to assemble the genome:
   cp ../SLURM_SCRIPTS/velvetoptimiser_noclean.sh .

   Then, the following command was run:
   sbatch velvetoptimiser_noclean.sh UFVPY185 23 123 10
   The reason for these arguments can be found in the previous step. 
