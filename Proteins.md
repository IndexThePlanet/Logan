# Logan protein clusters

## Dataset

Protein sequences extracted from Logan contigs using Prodigal, then clustered at 50% identity using MMseqs2. This is "Logan50" as reported in the manuscript.

## Data format

Proteins are stored in FASTA format, in sequences of amino acids.
The FASTA headers are the outputs of Prodigal: https://github.com/hyattpd/prodigal/wiki/understanding-the-prodigal-output#protein-translations

## Data access

Protein clusters are available via AWS S3 at the `s3://logan-pub` bucket in folder `/p/`. Two files:

   human-complete.fa.zst
   nonhuman-complete.fa.zst

To download:

    wget https://s3.amazonaws.com/logan-pub/p/human-complete.fa.zst
    wget https://s3.amazonaws.com/logan-pub/p/nonhuman-complete.fa.zst

## Decompression

To decompress:

    zstd -d *human-complete.fa.zst
