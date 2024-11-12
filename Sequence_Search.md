# Searching for sequences inside unitigs or contigs

## Introduction
Given an accession number, suppose you have a query sequence (of size $\geq k$) and want to find contigs / unitigs containing that sequence.

For the sake of the example, we will pick:

* Accession: `DRR000016`
* Sequence to search `AGATGGAGACATACAGAAATAGTCAAACCACATACTACAAAATGCCTACAAAATGCCAGTATCAGGCGGCGGCTTCG`

There are three methods, with  increased sophistication:

1. Logan Search, to search across all accessions
2. `grep` to search for one sequence in one accession
3. `back_to_sequences` to search for many sequences in one accession

## Method 0: Logan Search

You may use the Logan Search service to perform sequence search across Logan data:

https://logan-search.org/

This will return a list of accessions where your query is likely present. The service also uses `back_to_sequences` (presented later in this tutorial) to determine which unitig/contig sequence matches the query.

More details here: https://github.com/IndexThePlanet/LoganSearch

## Method 1: `grep`-like approach

This was formerly the "[Search for a k-mer of interest inside an unitigs accession](Kmer_search.md)" tutorial.

You will need [rcgrep](https://github.com/dib-lab/rcgrep):

```
pip install rcgrep
```

Then download the accession using the AWS CLI (see [Contigs.md](Contigs.md)): 

```
aws s3 cp s3://logan-pub/u/DRR000016/DRR000016.unitigs.fa.zst . --no-sign-request
```

To search for the k-mer within all the unitigs of the accession, including potential occurrences where this k-mer appears in reverse-complement form, type:

```
zstdcat DRR000016.unitigs.fa.zst | rcgrep -q TTTGGGTTCTTGTTTTCCCTCA --grepargs "-B 1 --no-group-separator" -
```

will output:

```
>DRR000016_912088 ka:f:1.7
CACAATGTTTGGGTTCTTGTTTTCCCTCATGACCAG
```

Which is in fact the only location where this k-mer occurs.

Searching for a shorter k-mer TTTGGGTTCTTA yields:

```
zstdcat DRR000016.unitigs.fa.zst | rcgrep -q TTTGGGTTCTTA --grepargs "-B 1 --no-group-separator" -
```

```
>DRR000016_644893 ka:f:1.6
CTTCATTTGGGTTCTTATCAGGATGGTACTTCAAG
>DRR000016_689157 ka:f:1.8
AAACCGTGTATCAATAGTTTGGGTTCTTAGTTTGCT
```

Note that Linux [process substitution](https://www.gnu.org/software/bash/manual/html_node/Process-Substitution.html#Process-Substitution) is not supported by rcgrep, hence the requirement to do `zstdcat [file] | rcgrep [..] - `.

## Method 2: `back_to_sequences` tool

The `back_to_sequences` tool segments the query into k-mers, providing sequence-wide presence and orientation data of each k-mer in matched unitig/contig sequences.

To install [back_to_sequences](https://github.com/pierrepeterlongo/back_to_sequences):

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
git clone https://github.com/pierrepeterlongo/back_to_sequences.git
cd back_to_sequences && cargo install --path .
```

Download the accession using the AWS CLI (see [Unitigs.md](Unitigs.md) and [Contigs.md](Contigs.md)): 

```bash
# for contigs: 
aws s3 cp s3://logan-pub/c/DRR000016/DRR000016.contigs.fa.zst . --no-sign-request
```

* Sequence queried (to be stored in a `query.fa` file):
```
>query
AGATGGAGACATACAGAAATAGTCAAACCACATACTACAAAATGCCTACAAAATGCCAGTATCAGGCGGCGGCTTCG
```

Here it is a single sequence, but multiple query sequences are supported too.

```
back_to_sequences --in-kmers query.fa --in-sequences  DRR000016.contigs.fa.zst --out-sequences selected_sequences_DRR000016.txt
```

This will create the file `selected_sequences_DRR000016.txt` containing  contig containing the query:

```
>DRR000016_0 ka:f:222.632     5 0.31646 
TCATCAATAGATGGAGACATACAGAAATAGTCAAACCACATCTACAAAATGCCAGTATCAGGCGGCGGCTTCGAAGCCAA...
```
There are five 31-mers from the query are found in the sequence. This is 0.31646% of the full contig `DRR000016_0`

**Note:** Adding option `--output-mapping-positions` enables to obtain the location of the k-mers from the queried sequence in the obtained contig:

```
>DRR000016_0 ka:f:222.632     5 0.31646 8 9 10 41 42
TCATCAATAGATGGAGACATACAGAAATAGTCAAACCACATCTACAAAATGCCAGTATCAGGCGGCGGCTTCGAAGCCAA...
```
The five 31-mers are located positions $\{8,9,10,41,\text{and } 42\}$ on the contig `DRR000016_0`.

`back_to_sequences` has other usages and options. Check the [doc](https://b2s-doc.readthedocs.io/en/latest/index.html)
