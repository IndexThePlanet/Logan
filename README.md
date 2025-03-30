# Logan

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/4c64fbca-68d2-420f-b0a8-13db4e73c750)
Photo source: https://www.karmactive.com/mount-logan-the-crown-jewel-of-canadas-peaks/

## Summary

Logan is a dataset of DNA and RNA sequences. It has been constructed by performing genome assembly over a December 2023 freeze of the entire NCBI Sequence Read Archive, which at the time contained 50 petabases of public raw data. Two related sets of assembled sequences are released: unitigs and contigs. Unitigs preserve nearly all the information present in the original sample, whereas contigs get rid of sequencing errors and biological variation for the benefit of increased sequence length. Both sets are hosted on a public S3 bucket provided by the Registry of Open Data at AWS, in compressed form. By downloading either unitigs or contigs, users can access the wealth of information contained in the SRA 10x (respectively 40x for contigs) more efficiently in time and disk space compared to raw reads, with minor loss of sensitivity and higher contiguity.

## Preprint

Read more about Logan here: https://www.biorxiv.org/content/10.1101/2024.07.30.605881v1

## SRA size

In December 2023, the public part of SRA consisted of raw sequencing data totalling 50 petabases across 27 million accessions. Each accession corresponds to a DNA sequencing experiment performed by a biology lab somewhere around the world. As you can see below, the majority of accessions and bases are DNA, followed by Covid samples and RNA, then metagenomes and single-cell.

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/3b76ced7-ed01-4842-83f0-d897c0cf7d55)

Note, the y-axis of the second panel is in megabases, as this is the preferred unit for SRA accession sizes. 20 G megabases is 20 petabases.

## v1 release

All sequencing experiments from a December 2023 freeze of the SRA have been reconstructed and made available as unitigs and contigs as a v1 release of Logan. See the [Stats v1](Stats-v1.md) page for more details on this data.

## v1.1 release

All v1 contigs were recomputed to increase contiguity and fix spurious repetitions. See [Stats v1.1](Stats-v1.1.md) page for more details on this data.

## Data access

See [Unitigs](Unitigs.md) and [Contigs](Contigs.md) pages.

## Tutorials

[Get a list of SRA accessions](SRA_list.md)

[Downloading unitigs of several accessions](Accessions.md)

[Search for sequences inside unitigs or contigs](Sequence_Search.md)

[Downloading, mapping many contigs to a gene of interest for a species of interest](Chickens.md)

## Related resources

[Logan Search](https://logan-search.org/)

[Logan talk at CGSI 2024](https://www.youtube.com/watch?v=H-8L_hFWXCQ)

## How to cite

Official Logan dataset URL: https://registry.opendata.aws/pasteur-logan/

BibTeX:

```
@article {logan,
	author = {Chikhi, Rayan and Raffestin, Brice and Korobeynikov, Anton and Edgar, Robert and Babaian, Artem},
	title = {Logan: Planetary-Scale Genome Assembly Surveys Life{\textquoteright}s Diversity},
	elocation-id = {2024.07.30.605881},
	year = {2024},
	doi = {10.1101/2024.07.30.605881},
	publisher = {Cold Spring Harbor Laboratory},
	eprint = {https://www.biorxiv.org/content/early/2024/07/31/2024.07.30.605881.full.pdf},
	journal = {bioRxiv}
}
```


## Team

- Lead: Rayan Chikhi
- co-Lead: Artem Babaian
- Robert C. Edgar
- Anton Korobeynikov
- Brice Raffestin
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
- Matthieu Falce
- Institut Pasteur admin:
  - Melanie Ridel
  - Loïc Orellou
  - Florence Percie du Sert
    
## Funding

- Institut Pasteur G5 Sequence Bioinformatics, ANR INCEPTION
- ERC Consolidator grant number 101088572 (IndexThePlanet)
- ANR-19-CE45-0008 (SeqDigger), ANR-22-CE45-0007 (Full-RNA)
- EU H2020 Marie Sklodowska-Curie grants agreements No 956229 (Alpaca) and 872539 (Pangaia)
- PaRis Artificial Intelligence Research Institute (PRAIRIE)
- Amazon Web Services
- Registry of Open Data on AWS

![image](https://github.com/IndexThePlanet/Logan/assets/1218301/daa6b5d9-78d0-4da9-aa68-27f329e1d3a8)

