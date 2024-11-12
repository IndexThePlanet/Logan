# Search for unitigs or contigs similar to a sequence of interest

## Introduction
Given an accession number, suppose you want to find either unitigs or contigs similar to a query sequence (of size $\geq k$).

For the sake of the example, we will pick:

* Accession: `DRR000016`
* Sequence queried (to be stored in a `query.fa` file):
```
>query
AGATGGAGACATACAGAAATAGTCAAACCACATACTACAAAATGCCTACAAAATGCCAGTATCAGGCGGCGGCTTCG
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

## Get the data and run back_to_sequences
### Get the unitig or contig file:

Download the accession using the AWS CLI (see [Unitigs.md](Unitigs.md) and [Contigs.md](Contigs.md)): 

```bash
# for unitigs: 
aws s3 cp s3://logan-pub/u/DRR000016/DRR000016.unitigs.fa.zst . --no-sign-request
# for contigs: 
aws s3 cp s3://logan-pub/c/DRR000016/DRR000016.contigs.fa.zst . --no-sign-request
```

For the rest of this example, we focus on contigs.

### Run back_to_sequences

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


## Read the doc

back_to_sequences has other usages and options. Check the [doc](https://b2s-doc.readthedocs.io/en/latest/index.html)
