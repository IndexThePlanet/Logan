# Logan v1 release

## General statistics

* 27.3 million accessions assembled into unitigs, totalling 96.00% of the SRA by size (48.2 petabases of raw reads)
* 26.8 million accessions assembled into contigs (around 88% by size)
* 2.18 petabytes of unitigs compressed (s3://logan-pub/u/)
* 385 terabytes of contigs compressed (s3://logan-pub/c/)

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

* metagenomes: 4,791,129 accessions, 43.4 TB compressed Logan contig contigs
* metatranscriptomes: 129,974 accessions, 2.6 TB compressed Logan contigs
* transcriptomes: 4,929,361 accessions, 80.9 TB compressed Logan contigs
* 
