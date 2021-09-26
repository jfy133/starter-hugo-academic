---
title: nf-core/eager
summary: nf-core/eager is a scalable and reproducible bioinformatics best-practise processing pipeline for genomic NGS sequencing data, with a focus on ancient DNA (aDNA) data, written in Nextflow.
tags:
- ancient DNA
- ancient metagenomics
- Nextflow
- nf-core
- eager
- genomics
- metagenomics
- pipeline
- bioinformatics
#date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: "https://nf-co.re/eager"

image:
  caption: Logo by the EAGER development team
  focal_point: Smart

links:
#- icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
url_code: "https://github.com/nf-core/eager"
#url_pdf: ""
#url_slides: ""
#url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
#slides: example
---

nf-core/eager is a scalable and reproducible bioinformatics best-practise processing pipeline for genomic NGS sequencing data, with a focus on ancient DNA (aDNA) data. It is ideal for the (palaeo)genomic analysis of humans, animals, plants, microbes and even microbiomes.

The pipeline is built using Nextflow, a workflow tool to run tasks across multiple compute infrastructures in a very portable manner. It comes with docker containers making installation trivial and results highly reproducible. The pipeline pre-processes raw data from FASTQ inputs, or preprocessed BAM inputs. It can align reads and performs extensive general NGS and aDNA specific quality-control on the results. It comes with docker, singularity or conda containers making installation trivial and results highly reproducible.

I co-lead development, and the current maintainer of the pipeline.

The pipeline was published in [PeerJ](https://doi.org/10.7717/peerj.10947) (open access).
