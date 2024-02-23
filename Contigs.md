# Logan Contigs

## Dataset

Contigs for all SRA accessions (as of December 2023) were constructed from the [Logan Unitigs](Unitigs.md). Unitigs were given to [Minia3](https://github.com/GATB/minia), which performed de Bruijn graph simplifications inspired by [SPAdes](https://github.com/ablab/spades). In total [XXX] accessions were processed. 

## Directory structure

Contigs are stored at the following location:

    s3://[bucket-name]/c/[accession].contigs.fa.zst

## Downloading 

To download one accession, using the [AWS CLI](https://aws.amazon.com/cli/), type:
    
    aws s3 cp s3://[bucket-name]/c/[accession].contigs.fa.zst . --no-sign-request

## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].contigs.fa.zst

## Theoretical guarantees

Contigs do not enjoy the same theoretical guarantees as the [unitigs](https://github.com/IndexThePlanet/Logan/blob/main/Unitigs.md#theoretical-guarantees). Except that, any 31-mer present in the contigs is guaranteed to also appear in the reads. Abundances are reported in the same way as in unitigs.
