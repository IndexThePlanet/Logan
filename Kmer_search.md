# Search for a k-mer of interest inside an unitigs accession

## Introduction

Suppose you want to search for a k-mer of interest inside the contigs of a particular accession.

For the sake of the example, we will pick:

* Accession: `DRR000016`
* k-mer: `TTTGGGTTCTTGTTTTCCCTCA`

## Setting up

You will need [rcgrep](https://github.com/dib-lab/rcgrep):

```
pip install rcgrep
```

Then download the accession using the AWS CLI (see [Contigs.md](Contigs.md)): 

```
aws s3 cp s3://logan-pub/u/DRR000016/DRR000016.unitigs.fa.zst . --no-sign-request
```

## Running

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
