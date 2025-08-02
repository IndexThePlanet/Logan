# Logan v1.1 release (29 Jan 2025)

## Changelog

* Only contigs were updated. Unitigs remain the same as v1.
* v1 contigs are temporarily moved to `s3://logan-pub/c1.0/`, will be deleted in ~1 year.
* Why the recompute? we fixed two bugs in Minia and recomputed all contigs from the v1 ones.
  1. In v1 some k-mers were repeated within a contig (looping 2-3 times), bug discovered by [@apcamargo](https://github.com/apcamargo): https://github.com/IndexThePlanet/Logan/issues/3
  2. In v1 some tips/bubbles were not properly deleted, bug discovered by [@syueqiao](https://github.com/syueqiao) and [@asl](https://github.com/asl)
     
## General statistics

Same as v1, except total contigs size. Cutoff date: December 10th, 2023

* 27.3 million accessions assembled into unitigs, totalling 96.00% of the SRA by size (48.2 petabases of raw reads)
* 26.8 million accessions assembled into contigs (specifically 26,788,835, around 88% of SRA by size)
* 2.18 petabytes of unitigs compressed (s3://logan-pub/u/)
* 315 terabytes of contigs compressed (s3://logan-pub/c/)


## Detailed statistics

Sequence statistics for each accession are available in `parquet` format, see e.g. [here](https://arrow.apache.org/docs/python/parquet.html) for how to parse.

For v1.1 contigs:

https://s3.amazonaws.com/logan-pub/stats/logan-seqstats-contigs-v1.1.parquet

It contains the following fields:

* `accession`: SRA accession name (e.g. SRRxxxxxx)
* `seqstats_contigs_n50`: N50 of contigs (in nucleotides)
* `seqstats_contigs_nbseq`: number of contigs
* `seqstats_contigs_maxlen`: maximal contig length (in nucleotides)
* `seqstats_contigs_sumlen`:  total size (in nucleotides) of contigs

For unitigs and v1 contigs, see [v1 stats](Stats-v1.md).

Example parser that prints `accession` and `seqstats_contigs_sumlen`:

```python
    import sys, pyarrow.dataset as ds
    d = ds.dataset(sys.argv[1], format="parquet").scanner(columns=["accession","seqstats_contigs_sumlen"])
    print("accession\tseqstats_contigs_sumlen")
    for b in d.to_batches():
        a = b.column("accession"); s = b.column("seqstats_contigs_sumlen")
        for i in range(b.num_rows):
            print(f"{(a[i].as_py() or '')}\t{(s[i].as_py() or '')}")
```

Example usage:

    python parquet_parse.py  logan-seqstats-contigs-v1.1.parquet > logan-seqstats-contigs-v1.1.sumlen.txt



## Various stats

![image](https://github.com/user-attachments/assets/f2ca285f-dea6-4fc7-ba00-d9bb52ac4195)

