# Downloading several accessions

## Introduction

This is a short tutorial to explain how to download several accessions. This is valid for both contigs or unitigs. In this example, we will download contigs.

## Setup

Prepare a text file of the list of accessions you wish to download. Say, `accessions.txt` containing:

```
DRR000273
DRR000280
DRR000294
DRR000300
DRR000305
DRR000339
```

## Download

To download the unitigs for all these accessions, a simple Bash command will do:

```
cat accessions.txt | xargs -I{} aws s3 cp s3://logan-staging/u/{}.unitigs.fa.zst .
```
