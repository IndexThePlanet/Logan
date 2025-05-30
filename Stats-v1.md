# Logan v1 release

## General statistics

Cutoff date: December 10th, 2023

* 27.3 million accessions assembled into unitigs, totalling 96.00% of the SRA by size (48.2 petabases of raw reads)
* 26.8 million accessions assembled into contigs (around 88% by size)
* 2.18 petabytes of unitigs compressed (s3://logan-pub/u/)
* 385 terabytes of contigs compressed (s3://logan-pub/c/)

## Detailed statistics

Sequence statistics for each accession are available in `parquet` format, see e.g. [here](https://arrow.apache.org/docs/python/parquet.html) for how to parse.

https://s3.amazonaws.com/logan-pub/stats/logan-seqstats.parquet


It contains the following fields:

* `accession`: SRA accession name (e.g. SRRxxxxxx)
* `seqstats_contigs_n50`: N50 of contigs (in nucleotides)
* `seqstats_contigs_nbseq`: number of contigs
* `seqstats_contigs_maxlen`: maximal contig length (in nucleotides)
* `seqstats_contigs_sumlen`:  total size (in nucleotides) of contigs 
* `size_contigs_after_compression` or `contigs_after_compression`: size (in bytes) of compressed contigs file
* `size_contigs_before_compression` or `contigs_before_compression`: size (in bytes) of uncompressed contigs file

Replace `contigs` to `unitigs` for unitigs stats.

## Contiguity

Statistics computed over 27.3 million accessions.

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/1567c75f-29e3-4f08-82d1-7acd751b8598)

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/9cd20913-0083-427e-996f-885b76cf9809)

## Space vs raw reads

Histogram of total sizes of unitigs (uncompressed)

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/bd2b4e6c-9a7f-4a56-8486-614098992639)

Relative space taken by unitigs bases vs raw reads bases: 

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/28a3aa92-7905-4579-979b-295852237e81)

(Note that, compressed unitigs take on average 10x less space than compressed .sra raw reads with quality information)

Relative space taken by contigs bases vs raw reads bases: 

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/c30627c0-8728-4590-bd92-a80045e9d87d)

## Library source breakdown

* metagenomes: 4,791,129 accessions, 43.4 TB compressed Logan contigs
* metatranscriptomes: 129,974 accessions, 2.6 TB compressed Logan contigs
* transcriptomes: 4,929,361 accessions, 80.9 TB compressed Logan contigs

Using a different counting method, metagenomes: 5,866,706 accessions, 3.7 Pbp in reads

(Athena: `consent = 'public' AND avgspotlen >= 31 AND releasedate <= date_parse('2023-12-10','%Y-%m-%d') and organism like )'%metagenom%'`)
