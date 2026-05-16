# Source code related to Logan

All repositories cited in the Methods section of the Logan paper are listed
below, grouped by whether the code was written for Logan or is existing
third-party software used by Logan.

## Logan-specific code

Code written for the Logan project, hosted across the contributing labs.

### Core infrastructure
* **Cloud infrastructure used to construct Logan** : https://gitlab.pasteur.fr/rchikhi_pasteur/erc-unitigs-prod/ (solely intended for internal usage due to its high execution costs)
* **f2sz**, the block variant of Zstandard developed for FASTA-aligned random access : https://github.com/asl/f2sz
* **Logan documentation and data access tutorial** : https://github.com/IndexThePlanet/Logan

### Search and indexing
* **Logan-DIAMOND and Logan-minimap2** shallow/deep homology search pipeline : https://gitlab.pasteur.fr/rchikhi_pasteur/logan-analysis

### Downstream analyses
* **Logan50 protein clustering** infrastructure : https://github.com/leejoey0921/cluster-logan/
* **Plasmid identification** via circular contig detection : https://gitlab.pasteur.fr/rchikhi_pasteur/logan-circles
* **reorient-circular-seq**, used in the plasmid pipeline to shift sequence breakpoints to intergenic regions : https://github.com/apcamargo/reorient-circular-seq
* **PETadex** plastic-active enzyme search : https://github.com/ababaian/petadex
* **P4 phage satellite** expansion : https://github.com/kdcurry/P4-logan
* **AMR gene discovery** : https://github.com/mmontonerin/logan_AMR
* **FracMinHash sketching of Logan metagenomes** : https://github.com/KoslickiLab/ingest_logan_yacht_data
* **GenBank WGS vs Logan metagenome comparison** : https://github.com/KoslickiLab/GenBank_WGS_analysis
* **SRA geographical metadata** extraction and enrichment : https://github.com/serratus-bio/logan-backend

## Third-party tools used in Logan

### Forks
* **Cuttlefish2 fork** with per-unitig abundance tracking, used for unitig assembly : https://github.com/rchikhi/cuttlefish (commit `9401ef5`)
* **kmtricks** `kmtricks-logan` tag, used to build the Logan-Search index : https://github.com/tlemane/kmtricks

### Used unmodified
* **Minia3** contig assembler : https://github.com/GATB/minia (commit `71484e8`)
* **kmviz** query result visualization, on which the Logan-Search front-end is based : https://github.com/tlemane/kmviz
