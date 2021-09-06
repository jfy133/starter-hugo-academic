---
title: "All Possible NCBI Taxonomy Ranks"
Summary: "How to to get all possible ranks in the NCBI taxonomy"
date: 2020-03-17
tags:
  - bioinformatics
  - ncbi
  - taxonomy
---

It was bothering me that nowhere on the NCBI website are all possible taxonomic
'ranks' actually described.

I eventually decided to go back to basics and literally download the raw data
the NCBI taxonomy website is based on - the infamous `taxonomy.dmp`, find the
column that gives a rank for *every single tax. ID*, and collapse to all unique
values.

To do this I downloaded and extracted the taxonomy dump as follows:

```bash
wget ftp://ftp.ncbi.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.zip
unzip new_taxdump.zip
```

Then after hunting through all the files, the one I wanted was `nodes.dmp`


The rank is recorded in the third column,

```text
$ head nodes.dmp
1 | 1 | no rank |   | 8 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |   |   ||  0 | 0 | 0 |
2 | 131567  | superkingdom  |   | 0 | 0 | 11  | 0 | 0 | 0 | 0 | 0 |   | ||  0 | 0 | 1 |
6 | 335928  | genus |   | 0 | 1 | 11  | 1 | 0 | 1 | 0 | 0 |   |   ||  0 | 0 | 1 |
7 | 6 | species | AC  | 0 | 1 | 11  | 1 | 0 | 1 | 1 | 0 |   |   ||  1 | 0 | 1 |
9 | 32199 | species | BA  | 0 | 1 | 11  | 1 | 0 | 1 | 1 | 0 |   |   ||  1 | 0 | 1 |
10  | 1706371 | genus |   | 0 | 1 | 11  | 1 | 0 | 1 | 0 | 0 |   |   ||  0 | 0 | 1 |
11  | 1707  | species | CG  | 0 | 1 | 11  | 1 | 0 | 1 | 1 | 0 | effective current name; |   |   | 1 | 0 | 1 |
13  | 203488  | genus |   | 0 | 1 | 11  | 1 | 0 | 1 | 0 | 0 |   |   ||  0 | 0 | 1 |
14  | 13  | species | DT  | 0 | 1 | 11  | 1 | 0 | 1 | 1 | 0 |   |   ||  1 | 0 | 1 |
16  | 32011 | genus |   | 0 | 1 | 11  | 1 | 0 | 1 | 0 | 0 |   |   ||  0 | 0 | 1 |

```

So to extract the column using and collapse to single values, run the following
bash-fu

```bash
cat nodes.dmp | cut -d '|' -f 3  | sort | uniq
```

You get the following result

```text
biotype
clade
class
cohort
family
forma
forma specialis
genotype
genus
infraclass
infraorder
isolate
kingdom
morph
no rank
order
parvorder
pathogroup
phylum
section
series
serogroup
serotype
species
species group
species subgroup
strain
subclass
subcohort
subfamily
subgenus
subkingdom
suborder
subphylum
subsection
subspecies
subtribe
subvariety
superclass
superfamily
superkingdom
superorder
superphylum
tribe
varietas
```

Note that this doesn't actually give you the hierarchy of all the wierd and
wonderful 'custom' groups taxonomists have come up with. However I found by
googling 'NCBI taxonomy <random_rank>' I could find enough examples that told me
approximately where they fell compared to the standard ranks (even if they are
technically analogous e.g. strain vs varietas)

The ones I did find:

* parvorder - comes after order
* section - comes after genus
* 'strain', 'morph', 'forma specialis' - comes after species
