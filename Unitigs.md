# Logan Unitigs

## Dataset

Unitigs for all SRA accessions. 

[Unitigs](https://github.com/GATB/bcalm/blob/master/bidirected-graphs-in-bcalm2/bidirected-graphs-in-bcalm2.md#unitigs-and-compaction) (non-branching paths of the de Bruijn graph, here k=31) are provided in FASTA format. Unitigs were constructed using a modified version of [Cuttlefish2](https://github.com/rchikhi/cuttlefish/) which records approximate mean k-mer abundance per unitig. In total 27.3 million accessions were processed.

## Data format

Unitigs are stored in FASTA format. The FASTA header is as follows:

    >[accession]_[counter] ka:f:[abundance]

Where `accession` is the accession name (e.g. SRR11905265), `counter` is a 0-based integer counter, `abundance` is the mean approximate k-mer abundance over the unitig.

Additional FASTA header in the form `L:i:x` are BCALM2-style links allowing for quick conversion to GFA. Note: in some large accessions, the `ka:i:[xx]` field is replaced by `km:i:[xx]`, this is a bug, consider that they are the same information.

## Data access

Unitigs are available via AWS S3 at the `s3://logan-pub` bucket in folder `/u/`.

To download unitigs for one accession, using the [AWS CLI](https://aws.amazon.com/cli/) (you do not need an AWS account), type:
    
    aws s3 cp s3://logan-pub/u/[accession]/[accession].unitigs.fa.zst . --no-sign-request

e.g. for accession [SRR11905265](https://www.ncbi.nlm.nih.gov/sra/?term=SRR11905265), type:

    aws s3 cp s3://logan-pub/u/SRR11905265/SRR11905265.unitigs.fa.zst . --no-sign-request

## Directory structure

On S3, all unitigs are stored at the following locations:

    s3://logan-pub/u/[accession]/[accession].unitigs.fa.zst

## Size

Careful, this S3 bucket is huge. As of the v1 release, the total size of all unitigs is 2.18 petabytes. It contains 27.3 million files. Just listing the folder will take half an hour using `s5cmd ls`.

## Decompression

To decompress a single unitigs file, type:

    zstd -d [accession].unitigs.fa.zst

Note: unitigs (and contigs) were compressed using [f2sz](https://github.com/asl/f2sz), which is a FASTA-aware block compressed zstd format. In principle one can decompress in parallel.

## Theoretical guarantees

Any 31-mer that occurs more than twice in the original SRA reads of an accession will appear in the unitigs. Conversely, any 31-mer in the unitigs is also present somewhere in the SRA reads of the accession. 

The reported mean abundance `ka:i:xxx` in the header of the unitigs is an approximation of how many reads each k-mer of that sequence occurs, on average. It is the product of two approximations: 1) Cuttlefish2 records abundances for k+1-mers and not k-mers, hence the abundance of a k-mer was obtained by summing all the abundances of the k+1-mers it appears in, then divided by two. 2) To save memory during Cuttlefish2, small abundances of k-mers were accurate within 5% error, and large abundances were capped at 50,000. This means that all abundances larger than 50,000 will be reported to be equal to 50,000. More information on the discretization scheme are [here](https://github.com/GATB/gatb-core/blob/b1a27642f873904838bef1b7d9224acdfb0c78fa/gatb-core/src/gatb/tools/collections/impl/MapMPHF.hpp#L84).
