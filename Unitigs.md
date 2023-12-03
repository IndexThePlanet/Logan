# Logan Unitigs

## Dataset

Unitigs for all SRA accessions (as of December 2023). 

[Unitigs](https://github.com/GATB/bcalm/blob/master/bidirected-graphs-in-bcalm2/bidirected-graphs-in-bcalm2.md#unitigs-and-compaction) (non-branching paths of the de Bruijn graph, here k=31) are provided in FASTA format, with BCALM2-style links allowing for quick conversion to GFA. Unitigs were constructed using a modified version of [Cuttlefish2](https://github.com/rchikhi/cuttlefish/) which records approximate mean k-mer abundance per unitig. Small abundances are accurate within 5% error, and abundances are capped at 50,000. In total [XXX] accessions were processed.

## Data format

Unitigs are stored in FASTA format. The FASTA header is as follows:

    >[accession]_[counter] ka:i:[abundance]

Where `accession` is the accession name (e.g. SRR14407446), `counter` is a 0-based integer counter, `abundance` is the mean approximate k-mer abundance over the unitig.

## Data access

Unitigs are available via AWS S3 at the s3://[bucket-name] bucket.

To download unitigs for one accession, using the [AWS CLI](https://aws.amazon.com/cli/), type:
    
    aws s3 cp s3://[bucket-name]/[accession].unitigs.fa.zstd . --no-sign-request

e.g. for accession [SRR14407446](https://www.ncbi.nlm.nih.gov/sra/?term=SRR14407446), type:

    aws s3 cp s3://[bucket-name]/SRR14407446.unitigs.fa.zstd . --no-sign-request

## Directory structure

On S3, all unitigs are stored at the following locations:

    s3://[bucket-name]/[accession].unitigs.fa.zstd

## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].unitigs.fa.zstd
