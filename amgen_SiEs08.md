# Project: AMGEN
### This is the workflow followed to analyse single-cell data
In this part of the project, Murine data was utilised.

- Author: [Ciro Ramirez Suastegui](https://github.com/cramirezs)
- Date started: 2020, October 24
- Date projected: 2020, December 10
- Date finished:

---

## Demultiplexing samples with bcl2fastq
You can actually either use mkfastq or bcl2fastq.<sup>1
1. Prepare IEM sample sheet (carefully choose the names).

## Pre-processing data with Cell Ranger
1. Prepare feature reference and aggregation files.
2. Check you're taking the right reference genome.
```
cellranger_wrappeR -h
cellranger_wrappeR -y /home/ciro/amg319/scripts/amgen_SiEs08_cellranger_NV035.yaml -v
```
Some jobs will be created and you will need to wait.<sup>1

## Demultiplexing donors/subjects
1. Have the donor metadata if you want to include it in the single-cell metadata. Up yo you if you want it in the object or if you'll make use of it after clustering.
2. Check the hashtag structure is correct (`Rscript /home/ciro/scripts/ab_capture/summary.R -h`)
```
sh /home/ciro/scripts/ab_capture/demux.sh -y /home/ciro/scripts/ab_capture/config.yaml -v
Rscript /home/ciro/scripts/ab_capture/summary.R -c /home/ciro/large/amg319/results/ab_demux/amgen_SiEs08_100th \
  --selected /home/ciro/large/amg319/raw/NV035/aggr/mo_treg/outs/aggregation.csv \
  --tag_str treatment~donor~hashtag_n~hashtag_id
```
Jobs will be created and you'll need to wait a few minutes.

## Quality control
1. Mind how you indicate the metadata.<sup>2
It takes about 5 minutes with 90K cells with 20gb and 1 node/1 processor.
```
Rscript /home/ciro/scripts/clustering/qc.R -y /home/ciro/amg319/scripts/amgen_SiEs08_qc.yaml
```

## Clustering analysis
This analysis is still a bit complex so you need to go to the master file to request jobs.
```
sh /home/ciro/amg319/scripts/amgen_SiEs08_jobConstSeu_setting.sh
```

## Differential gene expression analysis
## Trajectory analysis

<sup>1: Critical step; it takes a lot of time and memory.

<sup>2: You can run this step in an interactive job.
