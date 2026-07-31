# Logan Proteins

Protein sequences derived from all contigs published in the **Logan project (v1.0 contigs, accessions up to December 2023)**, called using **Prodigal**. Proteins marked as *complete* by Prodigal were then clustered at **50% identity** using **MMSeqs2**. Accessions are split into **human-associated** and **nonhuman-associated** datasets.

| Dataset | Description | Size |
|---------|-------------|------|
| `logan_c1.0_human_proteins.fasta.zst` | All human-associated proteins | 283 GiB |
| `logan_c1.0_nonhuman_proteins.fasta.zst` | All nonhuman-associated proteins | 1.8 TiB |
| `logan50_c1.0_human_complete.fasta.zst` | Representative clustered human-associated proteins (50% identity) | 5.5 GiB |
| `logan50_c1.0_nonhuman_complete.fasta.zst` | Representative clustered nonhuman-associated proteins (50% identity) | 253 GiB |

### Cluster mapping files

| File | Description | Size |
|------|-------------|------|
| `logan50_c1.0_human_complete-protein-to-cluster.tsv.zst` | Mapping of human-associated proteins to their Logan50 clusters | 347 GiB |
| `logan50_c1.0_human_complete-cluster-to-protein.tsv.zst` | Mapping of human-associated Logan50 cluster representatives to all the proteins in the cluster | 235 GiB |
| `logan50_c1.0_nonhuman_complete-protein-to-cluster.tsv.zst` | Mapping of nonhuman-associated proteins to their Logan50 clusters | 1.1 TiB |
| `logan50_c1.0_nonhuman_complete-cluster-to-protein.tsv.zst` | Mapping of nonhuman-associated Logan50 cluster representatives to all the proteins in the cluster | 806 GiB |


### Data download

Files are available in the `s3://logan-pub` bucket under folder `/p/`.

You can use either wget or aws. E.g. to download logan_human_proteins.fasta.zst you can do:
```bash
aws s3 cp s3://logan-pub/p/logan_c1.0_human_proteins.fasta.zst
wget https://s3.amazonaws.com/logan-pub/p/logan_c1.0_human_proteins.fasta.zst
```

Decompress files using `zstd`:
```bash
zstd -d file.zst
```


## 📂 Data format

### Protein files
- **Format:** FASTA (amino acid sequences).
- **Header format:** Includes a `CL:` tag indicating the corresponding Logan50 cluster.
- **Sorting:** FASTA records are sorted by Logan50 cluster.

### Cluster mapping files, protein-to-cluster
- **Format:** TSV (tab-separated values). Column 1 is the protein name, column 2 is the Logan50 cluster name.
- **Sorting:** Lexicographically sorted by protein name.

### Cluster mapping files, cluster-to-protein
- **Format:** TSV (tab-separated values). Column 1 is the Logan50 cluster name, column 2 is the protein name.
- **Sorting:** Lexicographically sorted by Logan50 cluster name.

---


## Fast access (indexed lookup)

The protein and cluster files are indexed, enabling fast key-based queries without decompressing the files. The index file is provided separately. Keys can be the Logan50 clusters (for the fasta files) or the protein name (for the cluster mapping files). Use the scripts from [RolandFaure/zstd_block_compress](https://www.github.com/RolandFaure/zstd_block_compress).

### Query examples
1. Retrieve all proteins in cluster `SRR21362335_1965_2`:

```bash
sh query_zst.sh logan_c1.0_nonhuman_proteins.index.tsv logan_c1.0_human_proteins.fasta.zst SRR21362335_1965_2
```

2. Find the cluster of the protein `SRR2625865_8456_1`:

```bash
sh query_zst.sh logan50_c1.0_nonhuman_complete-protein-to-cluster.index.tsv logan50_c1.0_nonhuman_complete-protein-to-cluster.tsv.zst SRR2625865_8456_1
```

3. Retrieve the sequence of the `SRR2625865_8456_1` protein by combining the two above commands:

```bash
sh query_zst.sh logan50_c1.0_nonhuman_complete-protein-to-cluster.index.tsv  logan50_c1.0_nonhuman_complete-protein-to-cluster.tsv.zst SRR2625865_8456_1 | \
  awk '{print $2}' | \
  xargs sh query_zst.sh logan_c1.0_nonhuman_proteins.index.tsv logan_nonhuman_proteins.fasta.zst | \
  grep -A 1 SRR2625865_8456_1
 ```
