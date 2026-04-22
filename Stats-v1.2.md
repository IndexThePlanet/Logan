# Logan v1.2 release (21 Apr 2026)

## Changelog

* Added Logan unitigs and contigs for nearly all SRA accessions from 2024 and 2025. 
     
## General statistics

Cutoff date: December 31th, 2025

* 38.1 million accessions assembled into unitigs, totalling 99.5% of the SRA by size (86.6 petabases of raw reads assembled into unitigs)
* 37.3 million accessions assembled into contigs (specifically 37,377,661 which is around 92.3% of the SRA by size)
* 4.19 petabytes of unitigs compressed (s3://logan-pub/u/)
* 623 terabytes of contigs compressed (s3://logan-pub/c/)

## Detailed statistics

Sequence statistics for each accession are available in `parquet` format, see e.g. [here](https://arrow.apache.org/docs/python/parquet.html) for how to parse.

https://s3.amazonaws.com/logan-pub/stats/logan-seqstats-contigs-v1.2.parquet

It contains the following fields:

* `accession`: SRA accession name (e.g. SRRxxxxxx)
* `seqstats_contigs_n50`: N50 of contigs (in nucleotides)
* `seqstats_contigs_nbseq`: number of contigs
* `seqstats_contigs_maxlen`: maximal contig length (in nucleotides)
* `seqstats_contigs_sumlen`:  total size (in nucleotides) of contigs

Replace contigs to unitigs for unitigs stats.

Example parser that prints `accession` and `seqstats_contigs_sumlen`:

```python
import sys,pyarrow.dataset as ds
c=["accession","seqstats_contigs_sumlen"]
sc=ds.dataset(sys.argv[1],format="parquet").scanner(columns=c)
print(*c,sep="\t")
for b in sc.to_batches():
    for a,s in ((x.as_py() or "", y.as_py() or "") for x,y in zip(*(b.column(n) for n in c))):
        print(a,s,sep="\t")
```

Example usage:

    python parquet_parse.py  logan-seqstats-contigs-v1.2.parquet > logan-seqstats-contigs-v1.2.sumlen.txt

