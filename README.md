# Transcriptomas_cracas

## quality trimming

```
trim_galore --paired -q 30 --fastqc -j 20 *_1.fastq *_2.fastq
```

## mitogenome assembly

I needed to run MITGARD from the bin directory individually for each transcriptome. For each transcriptome I run the following code.

```

./MITGARD.py -1 /home/riwama/Cracas_transcriptomes/2nd_files/trimmed/DRR169038_1_val_1.fq -2 /home/riwama/Cracas_transcriptomes/2nd_files/trimmed/DRR169038_2_val_2.fq -R ~/Cracas_transcriptomes/mitogenomes/refence.fa -c 20 -s DRR169038

mv *_mitogenome.fa ~/Cracas_transcriptomes/mitogenomes/
rm -rf *.fasta

rm DRR* -rf

rm -rf align.sam

rm mapped/ -rf

rm -rf bowtie_index/

echo done DRR169038
```

## Transcriptome assembly

To facilitate tracking of error I run individually the following code in a script named trinity_1.sh

```
Trinity --seqType fq --left ~/Cracas_transcriptomes/fq_files/trimmed/XXXX_1_val_1.fq --right ~/Cracas_transcriptomes/fq_files/trimmed/XXXX_2_val_2.fq --CPU 20 --max_memory 50G --output XXXX_trinity
```
## ORFs prediction with transdecoder

```
cd ~/Cracas_transcriptomes/Trinity/assemblies

for fasta in *.fa; do TransDecoder.LongOrfs -t $fasta; TransDecoder.Predict -t $fasta; done
```
# functional annotation
## diamond against swiss-prot

```
cd ~/Cracas_transcriptomes/Trinity/assemblies/pep


for file in *.pep;do diamond blastp --log --threads 20 --db ~/databases/swissprot/uniprot_sprot.diamond --query $file --ultra-sensitive --outfmt 6 --max-target-seqs 1 --evalue 1e-10 --out $file.blastp.txt; done
```

## functional annotation with interproscan

```
for file in *.pep;do sed 's/*//g' $file > $file.new; done

for file in *.new; do ~/Softwares/interproscan-5.61-93.0/./interproscan.sh -i $file -o /home/riwama/Cracas_transcriptomes/interpro_dir/$file.interpro.txt -goterm -cpu 5 -f gff3; done
```

## building specific databases

```
cd ~/databases/immunity/

for file in *.fasta; do diamond makedb --in $file -d $file.ddb; done
```


