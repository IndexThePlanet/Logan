# Logan Unitigs

## Dataset

Unitigs for all SRA accessions (as of December 2023) were constructed using a modified version of [Cuttlefish2](https://github.com/rchikhi/cuttlefish/). In total [XXX] accessions were processed.

## Directory structure

Unitigs are stored at the following location:

    s3://[bucket-name]/[accession].unitigs.fa.zstd

## Downloading 

To download one accession, using the [AWS CLI](https://aws.amazon.com/cli/), type:
    
    aws s3 cp s3://[bucket-name]/[accession].unitigs.fa.zstd . --no-sign-request

## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].unitigs.fa.zstd
