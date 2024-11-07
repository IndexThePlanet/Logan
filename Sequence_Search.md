# Search for a sequence of interest (of size $\geq k$) inside an unitigs/contig accession

## Introduction

Suppose you want to search for a sequence of interest inside the contigs of a particular accession.

For the sake of the example, we will pick:

* Accession: `DRR000016`
* Sequence to find (in a `query.fa` file):
```
>query
AGATGGAGACATACAGAAATAGTCAAACCACATCTACAAAATGC
```

## Setting up

You will need [back_to_sequences](https://github.com/pierrepeterlongo/back_to_sequences):

```bash
# install rust if not already installed:
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# install back to sequences:
git clone https://github.com/pierrepeterlongo/back_to_sequences.git
cd back_to_sequences
cargo install --path .
```
You may remove the created `back_to_sequences` directory once installed.

Then download the accession using the AWS CLI (see [Contigs.md](Contigs.md)): 

```bash
# for unitigs: 
aws s3 cp s3://logan-pub/u/DRR000016/DRR000016.unitigs.fa.zst . --no-sign-request
# for contigs: 
aws s3 cp s3://logan-pub/c/DRR000016/DRR000016.contigs.fa.zst . --no-sign-request
```

For the remaining of this example, we focus in contigs: 

## Running back_to_sequences

```
 back_to_sequences --in-kmers query.fa --in-sequences  DRR000016.contigs.fa.zst --out-sequences selected_sequences_DRR000016.txt
```

This will create the file `selected_sequences_DRR000016.txt` containing  contig containing the query:

```
>DRR000016_0 ka:f:222.632     14 0.88608 
TCATCAATAGATGGAGACATACAGAAATAGTCAAACCACATCTACAAAATGCCAGTATCAGGCGGCGGCTTCGAAGCCAA...
```

**Note:** Adding option `--output-mapping-positions` enables to obtain the location of the k-mers of the queried sequence in the obtained contig:

```
>DRR000016_0 ka:f:222.632     14 0.88608 8 9 10 11 12 13 14 15 16 17 18 19 20 21
TCATCAATAGATGGAGACATACAGAAATAGTCAAACCACATCTACAAAATGCCAGTATCAGGCGGCGGCTTCGAAGCCAA...
```

back_to_sequences has other ther usages and options. Check the [doc](https://b2s-doc.readthedocs.io/en/latest/index.html)
