# Logan Proteins

## 🧬 Dataset

This repository contains protein sequences derived from all contigs published in the **Logan project (v1.0, datasets up to December 2023)**, called using **Prodigal**. Proteins were extracted from Logan contigs, then clustered at **50% identity** using **MMSeqs2**. Accessions are split into **human-associated** and **nonhuman-associated** datasets.

### Available Datasets

| Dataset | Description | Size |
|---------|-------------|------|
| `logan_human_proteins.fasta.zst` | All human-associated proteins | 284 GiB |
| `logan50_human_proteins.fasta.zst` | Representative clustered human-associated proteins (50% identity) | 5.5 GiB |
| `logan_nonhuman_proteins.fasta.zst` | All nonhuman-associated proteins | 1.9 TiB |
| `logan50_nonhuman_proteins.fasta.zst` | Representative clustered nonhuman-associated proteins (50% identity) | 253 GiB |

> **RAYAN TODO:** Update dataset names from `(non)human-complete.fa.zst` to `logan50_(non)human_proteins.fasta.zst` to disambiguate from non-clustered versions.

### Cluster Mapping Files

| File | Description | Size |
|------|-------------|------|
| `human-complete-proteins-to-clusters.tsv.zst` | Mapping of human-associated proteins to their Logan50 clusters | 347 GiB |
| `nonhuman-complete-clusters.tsv.zst` | Mapping of nonhuman-associated proteins to their Logan50 clusters | 1.1 TiB |

---

## 📂 Data Format

### Protein Files
- **Format:** FASTA (amino acid sequences).
- **Header Format:** Includes a `CL:` tag indicating the corresponding Logan50 cluster.
- **Sorting:** FASTA records are sorted by Logan50 cluster.

### Cluster Mapping Files
- **Format:** TSV (tab-separated values). One column for the protein name, one column for the Logan50 cluster name.
- **Sorting:** Lexicographically sorted by protein name.

---

## 🔓 Decompression

Decompress files using `zstd`:
```bash
zstd -d file.zst
```

## ⚡ Fast Access (Indexed Lookup)

The protein and cluster files are zstd-indexed, enabling fast key-based queries without decompressing the files. Keys can be the Logan50 clusters (for the fasta files) or the protein name (for the cluster mapping files). Use the scripts from [RolandFaure/zstd_block_compress](https://www.github.com/RolandFaure/zstd_block_compress).

### Query Examples
1. Retrieve all proteins in cluster `SRR21362335_1965_2`:

```bash
sh query_zst.sh logan_nonhuman_proteins.index.tsv logan_nonhuman_proteins.fasta.zst SRR21362335_1965_2
```

2. Find the cluster of the protein `SRR2625865_8456_1`:

```bash
sh query_zst.sh nonhuman-complete-proteins-to-clusters.index.tsv nonhuman-complete-proteins-to-clusters.tsv.zst SRR2625865_8456_1
```

3. Retrieve the sequence of the `SRR2625865_8456_1` protein by combining the two above commands:

```bash
sh query_zst.sh nonhuman-complete-proteins-to-clusters.index.tsv nonhuman-complete-proteins-to-clusters.tsv.zst SRR2625865_8456_1 | \
  awk '{print \$2}' | \
  xargs sh query_zst.sh logan_nonhuman_proteins.index.tsv logan_nonhuman_proteins.fasta.zst | \
  grep -A 1 SRR2625865_8456_1
 ```

> **RAYAN NOTE**: The (non)human-complete.tsv.zst files present on this page before may be redundant since they can be reconstructed from logan_(non)human_proteins.fasta.zst. If you want to keep them, I zstd-block indexed them at: /pasteur/helix/projects/seqbio/rfaure/logan_tokeep/nonhuman-complete_tsv_sorted/nonhuman-complete-clusters.tsv.zst (with a corresponding index file).

## 📥 Data Download

> **RAYAN TODO:** Upload datasets to AWS S3. They are in :
   - /pasteur/helix/scratch/rfaure/all_prots/all_prots_single_file/logan_human_proteins.fasta.zst
   - /pasteur/helix/scratch/rfaure/all_prots/all_prots_single_file/logan_human_proteins.index.tsv
   - /pasteur/helix/scratch/rfaure/all_prots/all_prots_single_file/logan_nonhuman_proteins.fasta.zst
   - /pasteur/helix/scratch/rfaure/all_prots/all_prots_single_file/logan_nonhuman_proteins.index.tsv
   - logan50_(non)human_proteins.fasta.zst: already there, but named (non)human-complete.fa.zst -> to rename

   - /pasteur/helix/projects/seqbio/rfaure/logan_tokeep/human-complete_tsv_sorted/human-complete-proteins-to-clusters.tsv.zst
   - /pasteur/helix/projects/seqbio/rfaure/logan_tokeep/human-complete_tsv_sorted/human-complete-proteins-to-clusters.index.tsv
   - /pasteur/helix/projects/seqbio/rfaure/logan_tokeep/nonhuman-complete_tsv_sorted/nonhuman-complete-proteins-to-clusters.tsv.zst
   - /pasteur/helix/projects/seqbio/rfaure/logan_tokeep/nonhuman-complete_tsv_sorted/nonhuman-complete-proteins-to-clusters.index.tsv


### AWS S3 Access
Files are available in the `s3://logan-pub` bucket under `/p/`.

#### Protein Files
| File | Size |
|------|------|
| `logan_human_proteins.fasta.zst` | 284 GiB |
| `logan_human_proteins.index.tsv` | 447 KiB |
| `logan_nonhuman_proteins.fasta.zst` | 1.9 TiB |
| `logan_nonhuman_proteins.index.tsv` | 2.1 MiB |
| `logan50_human_proteins.fasta.zst` | 5.5 GiB |
| `logan50_nonhuman_proteins.fasta.zst` | 253 GiB |

#### Cluster Mapping Files
| File | Size |
|------|------|
| `human-complete-proteins-to-clusters.tsv.zst` | 347 GiB |
| `human-complete-proteins-to-clusters.index.tsv` | 132 KiB |
| `nonhuman-complete-proteins-to-clusters.tsv.zst` | 1.1 TiB |
| `nonhuman-complete-proteins-to-clusters.index.tsv` | 452 KiB |

### Download Command
You can use either wget or aws. E.g. to download logan_human_proteins.fasta.zst you can do:
```bash
aws s3 cp s3://logan-pub/p/logan_human_proteins.fasta.zst
wget https://s3.amazonaws.com/logan-pub/p/logan_human_proteins.fasta.zst
```
