# Logan protein clusters

## Dataset

Protein sequences extracted from Logan contigs using Prodigal, then clustered at 50% identity using MMseqs2. This is "Logan50" as reported in the manuscript.

## Data format

Proteins are stored in FASTA format, in sequences of amino acids.
The FASTA headers are the outputs of Prodigal: https://github.com/hyattpd/prodigal/wiki/understanding-the-prodigal-output#protein-translations

The TSV files provide the mapping between the original SRA contigs and the centroids.

## Data access

Protein clusters are available via AWS S3 at the `s3://logan-pub` bucket in folder `/p/`. Files:

    5.5 GiB human-complete.fa.zst
    235.4 GiB human-complete.tsv.zst
    252.7 GiB nonhuman-complete.fa.zst
    806.6 GiB nonhuman-complete.tsv.zst

To download:

    wget https://s3.amazonaws.com/logan-pub/p/human-complete.fa.zst
    wget https://s3.amazonaws.com/logan-pub/p/nonhuman-complete.fa.zst

## Decompression

To decompress:

    zstd -d *human-complete.fa.zst
