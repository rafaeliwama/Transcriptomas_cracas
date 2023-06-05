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


