# MyGenome

**Project for ABT 480-001**

> **Note:** The majority of this project was completed retroactively, so certain methods may differ from those used by other group members.

---

## 1. Retrieving Relevant Data Sets

Instead of downloading the files onto my personal UK VM to begin trimming, I copied them directly onto the supercomputer:

```bash
ssh jwst256@mcc.uky.edu
scp ngs@10.163.183.71:Desktop/Po9/Po9* .
```

Renamed files:

```bash
mv Po9_CKDL240039127-1A_22HTV5LT4_L2_1.fq.gz UFVPY185_1.fq.gz
mv Po9_CKDL240039127-1A_22HTV5LT4_L2_2.fq.gz UFVPY185_2.fq.gz
```

---

## 2. Pre-Trimming Statistics

*Talk about the command used to get the pics and steps taken*

*Insert photo of pre-trimmed data stats here*

---

## 3. Using Velvet Optimiser to Determine Optimal K Value

To retrieve the number of reads in the dataset:

```bash
grep -c "^@" UFVPY185_1_paired.fastq
```

Output:

```
6611873 reads
```

Inputs entered into **Velvet Advisor**:

- Read count: 6,611,873
- Base pair length: 140 (given)
- Estimated genome size: 45 megabases (given)
- K-mer coverage: 20-fold (given)

Based on this, Velvet Advisor recommended starting with a **k value of 73**.

---

## 4. Trimming the Data

Used the `trim-velvet.sh` script with a k-mer value range of 50 on either side of the optimal value (73), for a total range of 100. The step size was set to 10.

Command used:

```bash
sbatch trim-velvet.sh . UFVPY185 23 123 10 yes
```

> Note: While this command trimmed the data successfully, the assembly portion of the script did not work as expected and was performed in a later step.

Result: Two sets of paired and two sets of unpaired trimmed data.

---

## 5. Assembling the Genome

Still working on the supercomputer, I copied the SLURM script to begin the genome assembly:

```bash
cp ../SLURM_SCRIPTS/velvetoptimiser_noclean.sh .
```

Then ran:

```bash
sbatch velvetoptimiser_noclean.sh UFVPY185 23 123 10
```

> These arguments match those used in the trimming step.

---

## 📌 Notes

- Make sure to monitor the SLURM job outputs for successful completion.
```bash
cat slurm-jobID.out
```
---
