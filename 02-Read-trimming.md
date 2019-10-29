# Day 2: Read trimming

## Connecting to Taito

See the instructions [here](01-UNIX-and-CSC.md#connecting-to-taito).

## PART 1

### Downloading the raw genome data

First let's create a folder for the course in our work directory. Your work directory is located in **/wrk/yourusername**, but it can also be accessed using the variable **$WRKDIR**:

```bash
mkdir $WRKDIR/MMB114
cd $WRKDIR/MMB114
```

Now let's copy the raw genome data from my CSC account **(remember: don't just copy the code, but don't write everything either; use the tabulator!)**:

```bash
cp /wrk/stelmach/MMB114/Maunula_Hultman_microbial_genomes_MiSeq-20191018.tar.gz .
```

The raw genome data was given to us by the sequencing facility as a tar archive (**.tar.gz**). So the first thing we need to do is extract the archive:

```bash
tar -zxf Maunula_Hultman_microbial_genomes_MiSeq-20191018.tar.gz
```

Investigate the contents of the folder. We can see that four GZIP files (**.gz**) were extracted from the tar archive (one for the R1 and one for the R2 reads of each genome). GZIP is a compressed file much like a ZIP file, so to continue we have to decompress them first. We will do this with the command **gunzip**:

```bash
gunzip A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq.gz
gunzip A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R2_001.fastq.gz
gunzip A023-E3-CTTGCCTC-CTGCAGTA-Maunula-Hultman-run20191018R_S23_L001_R1_001.fastq.gz
gunzip A023-E3-CTTGCCTC-CTGCAGTA-Maunula-Hultman-run20191018R_S23_L001_R2_001.fastq.gz
```

Investigate the contents of the folder again. Do you see that the FASTQ files are now uncompressed?  

Now let's take a look at the FASTQ files to see how they look like. Here we will use three useful commands for visualizing text files (**head**, **tail** and **less**):

```bash
head A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq
tail A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq
less A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq

# In less, scroll down by hitting the space bar
# To quit, hit "q"
```

### Performing a quality assessment of the raw data

Now we will run a program called FASTQC for quality assessment of the raw genome data. The tasks we are performing from now on require more memory than simple commands. Running them on the login node would make the system slow, and if a task takes more than 30 minutes to complete, it is cancelled by the system. Instead, we have to connect to another node, called the Taito-shell:

```bash
sinteractive
```

The next step is to load the **biokit** environment, which contains several programs widely used in bioinformatics:

```bash
module load biokit
```

Then we go to the folder where we have put the data:

```bash
cd $WRKDIR/MMB114
```

And now we run FASTQC:

```bash
fastqc A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq
fastqc A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R2_001.fastq
fastqc A023-E3-CTTGCCTC-CTGCAGTA-Maunula-Hultman-run20191018R_S23_L001_R1_001.fastq
fastqc A023-E3-CTTGCCTC-CTGCAGTA-Maunula-Hultman-run20191018R_S23_L001_R2_001.fastq
```

After all tasks are completed, we exit the Taito-shell by typing:

```bash
exit
```

One of the outputs of FASTQC is a HTML document. Let's look at it by moving it to your laptop with the file transfer program FileZilla. Remember to download the files for the R1 and the R2 reads of both genomes. In your computer, double-click to open them in your favourite web browser.  

Take a look at the FASTQC reports. How does the data look like?

* How many sequences are in R1? And in R2?
* The "Per base sequence quality" module shows a problem. Why?
  * Which part of the reads have the best quality? Beginning, middle, end?
* What is the mean sequence quality for the majority of the reads?
* The "Per base sequence content" module shows a problem. Why?
* The "Adapter content" module also shows a problem. Why?
  * Are there adapters in our reads?
  * In which part of the reads?
  * What are adapters and why should we remove them?
* Do you see differences between the R1 and R2 reads?

To help you answering the questions above, you can read more about the FASTQC report [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/).

## PART 2

### Trimming adapters and low-quality regions

Now we will run a program called CUTADAPT to trim the reads of adapters and low-quality regions. From now on we will work only with the first genome (A022-GA001).

First, let's connect to the Taito-shell, load **biokit** and go to the course folder:

```bash
sinteractive
module load biokit
cd $WRKDIR/MMB114
```

And now we run CUTADAPT. But first, take a moment to familiarize yourself the tool. Look at the command below and see which flags (**-LETTER**) we are passing to CUTADAPT (**DO NOT RUN**):

```bash
cutadapt -a CTGTCTCTTATACACATCTCCGAGCCCACGAGAC
         -A CTGTCTCTTATACACATCTGACGCTGCCGACGA
         -o GA001_R1_trimmed.fastq
         -p GA001_R2_trimmed.fastq
         -q 20
         -m 60
         A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq
         A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R2_001.fastq
```

Now let's take a look at the help page for CUTADAPT to understand what each of these flags are doing:

```bash
cutadapt -h | less

# You can scroll down by hitting the space bar
# To quit, hit "q"
```

You can also read more about CUTADAPT [here](https://cutadapt.readthedocs.io/en/stable/guide.html).  

Now that we understand well what we are doing, let's run CUTADAPT. Pay attention as it is a long command **(you have to scroll to the right to see the full command)**:

```bash
cutadapt -a CTGTCTCTTATACACATCTCCGAGCCCACGAGAC -A CTGTCTCTTATACACATCTGACGCTGCCGACGA -o GA001_R1_trimmed.fastq -p GA001_R2_trimmed.fastq -q 20 -m 60 A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R1_001.fastq A022-GA001-TTGCATGT-ATTCCATG-Maunula-Hultman-run20191018R_S22_L001_R2_001.fastq > log_cutadapt.txt
```

When CUTADAPT has finished, list the contents of the directory. Why do the names look so different from before?

Now take a look at the file "log_cutadapt.txt" with the command **less**:

* How many times adapters were trimmed in R1 and R2?
* How many reads were removed because they were too short?
* How many low-quality bases were trimmed?

Now let's run FASTQC again, this time using the trimmed genome data:

```bash
fastqc GA001_R1_trimmed.fastq
fastqc GA001_R2_trimmed.fastq
```

After both tasks are completed, we exit the Taito-shell:

```bash
exit
```

Move the new HTML documents to your laptop with FileZilla and open them in a web browser. Compare these with the FASTQC reports for the raw genome data. What kind of differences you see?

* Do the "Per base sequence quality" plots look different now? Why?
* What is the mean sequence quality for the majority of the reads now?
* The "Sequence length distribution" module shows a warning now. Have the read lengths changed? Why?
* Are there still adapter sequences in our reads?
