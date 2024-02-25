# Logan

## Summary

Logan is a dataset of DNA and RNA sequences. It has been constructed by assembling a December 2023 freeze of the entire NCBI Sequence Read Archive, using Cuttlefish2 to create unitigs, then using Minia3 to create contigs. Both unitigs and contigs are hosted on a bucket in this registry, in compressed form. By downloading either unitigs or contigs, users can access the wealth of information contained in the SRA using respectively 20x or 80x less space compared to raw reads, with minor loss of sensitivity and higher contiguity.

## SRA size

In December 2023, the public access part of SRA is about 60 petabases, in 23 million accessions. As you can see below, the majority of accessions and bases are DNA, followed by Covid samples and RNA-seq, then metagenomes and single-cell data.

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/3b76ced7-ed01-4842-83f0-d897c0cf7d55)


## Data access

See [Unitigs](Unitigs.md) and [Contigs](Contigs.md) pages.

## Tutorials

[Downloading unitigs of several accessions](Accessions.md)

[Search for a k-mer of interest inside an unitigs accession](Kmer_search.md)

[Downloading, mapping many contigs to a gene of interest](Chickens.md)
