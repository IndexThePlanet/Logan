# Download then map contigs to a gene of interest

## Introduction

In this tutorial, we will explain how to download all contigs from a pre-defined list of accessions, then map these contigs on the fly to a gene of interest. For the sake of the example, we will use accessions that contain chickens genomes.

## Getting a list of accessions

This part is outside of the scope of Logan, but we will describe it for completeness. Assume you are interested in obtaining all accessions in the SRA that contain a chicken genome. Here I am skipping a lot of steps, but the high level description is that you may use [AWS Athena](https://www.ncbi.nlm.nih.gov/sra/docs/sra-athena-examples/) for that, with the query:

```
   SELECT *
   FROM "sra"."tax_analysis"
   WHERE name = 'Gallus gallus' AND total_count > 100
```
The outcome of this step is a list of accessions, i.e. a `.txt` file containing:

```
   SRRxxxxx
   SRRyyyyy
   ...
```

## Preparing the analysis

Assume you have a sequence of interest to which you wish to map all Logan contigs from these accessions. That sequence may be a gene, e.g. the [MC1R](https://www.ncbi.nlm.nih.gov/gene/427562) gene from the chicken reference genome, responsible for feather color. 
Let us assume for the rest of the document that the gene sequence is given in the file `mc1r.fa`, and that we want to search for all homologs of this gene inside the SRA.

## Downloading then mapping accessions, without Logan

Now, if you were to download reads from the SRA and map them to this gene of interest, you could do something like:

```
aws s3 cp s3://sra-pub-run-odp/sra/$accession/$accession \
	  $accession.sra \
          --no-sign-request
minimap2 -t20 -x sr mc1r.fa \
         <(fasterq-dump --fasta-unsorted $accession.sra) \
         -o mapping/$accession.minimap2_output
```

for each accession. This download reads from SRA as hosted on AWS Open Data, then maps using `minimap2` with on the fly conversion of SRA format to fasta using NCBI's `fasterq-dump`. 
Despite the usage of Linux pipes, this is a time-consuming step due to the size of the raw reads. Then you also need to later either perform genome assembly or variant calling to reconstruct consensuses per accession.

## Downloading then mapping accessions, with Logan

Instead, consider using Logan to directly map contigs to the gene of interest:

```
minimap2 -t 8 -a mc1r.fa \
          <(aws s3 cp s3://logan-pub/c/$accession/$accession.contigs.fa.zst - | zstdcat) \
    | samtools view -hF4 - \
    > mapping-logan/$accession.minimap2_output
```

The `zstdcat` command decompresses contigs on the fly, and `samtools view` command gets rid of unmapped contigs.

This sidesteps the assembly or variant calling steps, and you immediately get contigs that contain the gene of interest.
