# Logan

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/4c64fbca-68d2-420f-b0a8-13db4e73c750)
Photo source: https://www.karmactive.com/mount-logan-the-crown-jewel-of-canadas-peaks/

## Summary

Logan is a dataset of DNA and RNA sequences. It has been constructed by performing genome assembly over a December 2023 freeze of the entire NCBI Sequence Read Archive, which at the time contained 50 petabases of public raw data. Two related sets of assembled sequences are released: unitigs and contigs. Unitigs preserve nearly all the information present in the original sample, whereas contigs get rid of sequencing errors and biological variation for the benefit of increased sequence length. Both sets are hosted on a public S3 bucket provided by the Registry of Open Data at AWS, in compressed form. By downloading either unitigs or contigs, users can access the wealth of information contained in the SRA 10x (respectively 40x for contigs) more efficiently in time and disk space compared to raw reads, with minor loss of sensitivity and higher contiguity.

## SRA size

In December 2023, the public part of SRA consisted of saw sequencing data totalling 50 petabases across 27 million accessions. Each accession corresponds to a DNA sequencing experiment performed by a biology lab somewhere around the world. As you can see below, the majority of accessions and bases are DNA, followed by Covid samples and RNA, then metagenomes and single-cell.

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/3b76ced7-ed01-4842-83f0-d897c0cf7d55)

Note, the y-axis of the second panel is in megabases, as this is the preferred unit for SRA accession sizes. 20 G megabases is 20 petabases.

## v1 release

All sequencing experiments from a December 2023 freeze of the SRA have been reconstructed and made available as unitigs and contigs as a v1 release of Logan. See the [Stats v1](Stats-v1.md) page for more details on this data.

## Data access

See [Unitigs](Unitigs.md) and [Contigs](Contigs.md) pages.

## Tutorials

[Downloading unitigs of several accessions](Accessions.md)

[Search for a k-mer of interest inside an unitigs accession](Kmer_search.md)

[Downloading, mapping many contigs to a gene of interest](Chickens.md)

## Team

- Lead: Rayan Chikhi
- Artem Babaian
- Robert C. Edgar
- Anton Korobeynikov
- Brice Raffestin
- Matthieu Falce
- AWS engineering:
  - Greg Autric
  - Maxime Hugues
- AWS management:
  - Dorian Schaal
  - Adrien Lainé
- Institut Pasteur IT:
  - Thomas Menard
  - Stéphane Fournier

## Acknowledgements

- AWS Registry of Open Data
  - Peter Schmiedeskamp
  - Chris Stoner
  - Erin Chu
- NCBI SRA
  - Ryan Connor
  - Yuriy Skripchenko
    
## Funding

- Institut Pasteur G5 Sequence Bioinformatics, ANR INCEPTION 
- PaRis Artificial Intelligence Research Institute (PRAIRIE)
- ERC Consolidator grant number 101088572 (IndexThePlanet)
- Amazon Web Services
- Registry of Open Data on AWS

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/daa6b5d9-78d0-4da9-aa68-27f329e1d3a8)

