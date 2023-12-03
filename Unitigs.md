# Logan Unitigs

## Dataset

Unitigs for all SRA accessions (as of December 2023). 

[Unitigs](https://github.com/GATB/bcalm/blob/master/bidirected-graphs-in-bcalm2/bidirected-graphs-in-bcalm2.md#unitigs-and-compaction) (non-branching paths of the de Bruijn graph, here k=31) are provided in FASTA format, with BCALM2-style links allowing for quick conversion to GFA. Unitigs were constructed using a modified version of [Cuttlefish2](https://github.com/rchikhi/cuttlefish/) which records approximate mean k-mer abundance per unitig. In total [XXX] accessions were processed.

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

## Theoretical guarantees

Any 31-mer that occurs more than twice in the original SRA reads of an accession will appear in the unitigs. Conversely, any 31-mer in the unitigs is also present somewhere in the SRA reads of the accession. 

The reported mean abundance `ka:i:xxx` in the header of the unitigs is approximate. It is the product of two approximations: 1) Cuttlefish2 records abundances for k+1-mers and not k-mers, hence the abundance of a k-mer was obtained by summing all the abundances of the k+1-mers it appears in, then divided by two. 2) To save memory, small abundances are accurate within 5% error, and large abundances are capped at 50,000. This means that all abundances larger than 50,000 will appear as abundance 50,000. More information on the discretization scheme: https://github.com/GATB/gatb-core/blob/b1a27642f873904838bef1b7d9224acdfb0c78fa/gatb-core/src/gatb/tools/collections/impl/MapMPHF.hpp#L84

## Directory structure

On S3, all unitigs are stored at the following locations:

    s3://[bucket-name]/[accession].unitigs.fa.zstd

## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].unitigs.fa.zstd
