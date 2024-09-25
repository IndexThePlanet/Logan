# Logan Contigs

## Dataset

Contigs for all SRA accessions were constructed directly from the [Logan Unitigs](Unitigs.md). Unitigs were given to [Minia3](https://github.com/GATB/minia), which performed de Bruijn graph simplifications inspired by [SPAdes](https://github.com/ablab/spades) with just k=31. In total 26.7 million accessions were processed. 

## Directory structure

Contigs are stored at the following location:

    s3://logan-pub/c/[accession]/[accession].contigs.fa.zst

## Size

Careful, this S3 bucket is huge. The total size of all unitigs is 385 terabytes compressed. It contains 26.7M files. Just listing the folder will take half an hour.

## Downloading 

To download one accession, type: 

    wget https://s3.amazonaws.com/logan-pub/c/[accession]/[accession].contigs.fa.zst 

e.g. for accession [SRR11905265](https://www.ncbi.nlm.nih.gov/sra/?term=SRR11905265), type:

    wget https://s3.amazonaws.com/logan-pub/c/SRR11905265/SRR11905265.contigs.fa.zst 

Faster downloads are using the [AWS CLI](https://aws.amazon.com/cli/) (you do not need an AWS account), type:
    
    aws s3 cp s3://logan-pub/c/[accession]/[accession].contigs.fa.zst . --no-sign-request


## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].contigs.fa.zst

## Theoretical guarantees

Contigs do not enjoy the same theoretical guarantees as the [unitigs](https://github.com/IndexThePlanet/Logan/blob/main/Unitigs.md#theoretical-guarantees). Except that, any 31-mer present in the contigs is guaranteed to also appear in the reads. Abundances are reported in the same way as in unitigs.

## Assembly graph

To recover the `.gfa` assembly graph, follow the same procedure as in the [Unitigs page](Unitigs.md#assembly-graph).
