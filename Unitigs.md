# Logan Unitigs

## Dataset

Unitigs for all SRA accessions (as of December 2023). 

[Unitigs](https://github.com/GATB/bcalm/blob/master/bidirected-graphs-in-bcalm2/bidirected-graphs-in-bcalm2.md#unitigs-and-compaction) (non-branching paths of the de Bruijn graph, here k=31) are provided in FASTA format, with BCALM2-style links allowing for quick conversion to GFA. Unitigs were constructed using a modified version of [Cuttlefish2](https://github.com/rchikhi/cuttlefish/) which records approximate mean k-mer abundance per unitig. Small abundances are accurate within 5% error, and abundances are capped at 50,000. In total [XXX] accessions were processed.

## Directory structure

Unitigs are stored at the following location:

    s3://[bucket-name]/[accession].unitigs.fa.zstd

## Downloading unitigs

To download unitigs for one accession, using the [AWS CLI](https://aws.amazon.com/cli/), type:
    
    aws s3 cp s3://[bucket-name]/[accession].unitigs.fa.zstd . --no-sign-request

e.g. for accession [SRR14407446](https://www.ncbi.nlm.nih.gov/sra/?term=SRR14407446), type:

    aws s3 cp s3://[bucket-name]/SRR14407446.unitigs.fa.zstd . --no-sign-request
    
## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].unitigs.fa.zstd
