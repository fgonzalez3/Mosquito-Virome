# Make adapter.fasta file 

```
with open('adapters.txt', 'r') as file:
    lines = file.readlines()

fasta_lines = []
for line in lines[1:]:  # Skip the header row
    name, sequence = line.strip().split('\t')
    fasta_lines.append(f'>{name}\n{sequence}\n')

with open('adapters.fasta', 'w') as file:
    file.writelines(fasta_lines)
```

# Trim adapters with adapter.fasta file 

```
#!/bin/bash
#SBATCH --job-name=trimmomatic
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=06:00:00
#SBATCH --output=trimmomatic.out
#SBATCH --error=trimmomatic.err

module load miniconda 
conda activate beluga 

trimmomatic PE -threads 4 \
   EAal-9_362_215_S40_L001_R1_001.fastq.gz EAal-9_362_215_S40_L001_R2_001.fastq.gz \
   EAal-9_362_215_S40_L001_R1_001.trimmed.fastq EAal-9_362_215_S40_L001_R1_001.untrimmed.fastq \
   EAal-9_362_215_S40_L001_R2_001.trimmed.fastq EAal-9_362_215_S40_L001_R2_001.untrimmed.fastq \
   ILLUMINACLIP:adapters.fasta:2:30:10 SLIDINGWINDOW:4:20
```
