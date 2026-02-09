# Viral_genomics_project
# 1- upload the reference and viral data using wget
wget -nc ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR574/003/ERR5743893/ERR5743893_1.fastq.gz
wget -nc ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR574/003/ERR5743893/ERR5743893_2.fastq.gz
wget https://www.ebi.ac.uk/ena/browser/api/fasta/MN908947.3 -O MN908947.fasta
# 2- update the system and download tool for viral_genomics_project
sudo apt update 
sudo apt install fastqc
sud apt install bwa 
sudo apt install samtools
conda install -c bioconda freebayes
# 3- Gunzip the file
gunzip ERR5743893_1.fastq.gz
gunzip ERR5743893_2.fastq.gz
# 4- Run the fastqc command
fastqc ERR5743893_1.fastq ERR5743893_2.fastq
fastqc ERR5743893_1.fastq ERR5743893_2.fastq --outdir QC_Rep
# 5- Alignment with reference using bwa tool
bwa index MN908947.fasta 
bwa mem MN908947.fasta ERR5743893_1.fastq ERR5743893_2.fastq > ERR5743893.sam
# 6- Running samtools for converting sam data to bam data for further analysis
samtools view -@ 20 -Sb  ERR5743893.sam > ERR5743893.bam
samtools sort -@ 32 -o ERR5743893.sorted.bam ERR5743893.bam
# 7- variant calling using freebayes
freebayes -f MN908947.fasta ERR5743893.sorted.bam  > ERR5743893.vcf



