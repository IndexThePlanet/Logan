# Get a list of SRA accessions

## Introduction

This is a short tutorial to explain how to get a list of SRA accessions from a particular species. (This is not specific to Logan, it is just some advanced SRA browsing.)

## Simple method (Logan-provided CSV)

Suppose you want a list of all SRA accessions in Logan where the annotated species is Homo sapiens. First, find out the `tax_id` using https://www.ncbi.nlm.nih.gov/taxonomy/ (for human it is 9606).

Then, type this command:

    aws s3 cp s3://logan-pub/stats/sra_taxid.csv.zst - --no-sign-request | zstdcat | \
    awk -F',' '$4 == "\"9606\"" { print }' \
    > list_accessions_human.txt

What this does, is stream a file from the Logan bucket that contains a list of all SRA accessions in Logan, along with some metadata extracted from the SRA (library type, organism name, organism tax ID, taxon rank, I forgot the last field). For other species, change 9606 in the command to the correct `tax_id`.

This gives you a list of SRA accessions that are annotated as belonging to that species. But of course, not all Logan contigs of those accession will be from that species, e.g. think of the contaminating viruses, bacteria, etc..

## Advanced method (STAT)

This one doesn't simply use the SRA metadata provided by submitters, but uses inferred taxonomy in sequences. An example usage is shown [in this other tutorial](https://github.com/IndexThePlanet/Logan/blob/main/Chickens.md#getting-a-list-of-accessions) as well.

Basically, NCBI has annotated the entire SRA with a rough taxonomy of each accession. Check out their paper: https://pmc.ncbi.nlm.nih.gov/articles/PMC8450716/

This data can be accessed via the cloud using Amazon Athena or Google BigQuery. Detailed instructions are provided here: https://www.ncbi.nlm.nih.gov/sra/docs/sra-cloud/

Then, same principle as the previous method: if you have a species name of interest, you can get a list of accessions. Here, it is with a SQL query:

    SELECT *
    FROM "sra"."tax_analysis"
    WHERE name = 'my species name, e.g. homo sapiens' AND total_count > 10000

It will grab the list of accessions where there are sufficiently many reads from that species, and for each accession you have the `total_count` field, which very roughly approximates the number of reads from that accession corresponding to the species (but think of it as a subsampled number).

As an alternative to Amazon Athena or Google BigQuery, you can use [DuckDB](https://duckdb.org/) to query SRA metadata stored as Parquet files in the cloud. This approach is simpler, as it does not require an account with a cloud provider, but it provides access to a more limited set of metadata compared to Athena and BigQuery.

  duckdb -c "
  INSTALL httpfs;
  LOAD httpfs;
  INSTALL parquet;
  LOAD parquet;
  COPY (
    SELECT *
    FROM read_parquet('s3://sra-pub-metadata-us-east-1/sra/metadata/*')
    WHERE releasedate > '2021-09-07' AND geo_loc_name_country_calc = 'Brazil'
  ) TO STDOUT WITH (FORMAT CSV, DELIMITER E'\t', HEADER);"

The query above retrieves accessions released after 7 September 2021, where the associated samples were collected in Brazil.
