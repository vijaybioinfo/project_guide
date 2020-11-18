# Project: SiEs10
### This is the workflow followed to analyse single-cell data
In this part of the project, Murine data was utilised.

- Author: [Ciro Ramirez Suastegui](https://github.com/cramirezs)
- Date started: 2020, November 16
- Date projected: 2020, December
- Date finished:

---

## Demultiplexing samples with bcl2fastq
You can actually either use mkfastq or bcl2fastq.<sup>1
1. Prepare IEM sample sheet (carefully choose the names).

## Pre-processing data with Cell Ranger
We have a [wrapper](https://github.com/vijaybioinfo/cellranger_wrappeR) for Cell Ranger if you don't want to prepare a script for each of your samples.
1. Prepare feature reference and aggregation files.
2. Check you're taking the right reference genome.
```
cellranger_wrappeR -h
sh /home/ciro/scripts/cellranger_wrappeR/run.sh -y /home/ciro/christian/scripts/cellranger_config_NV038.yaml -v
```
Some jobs will be created and you will need to wait.<sup>1
```
sh /home/ciro/scripts/cellranger_wrappeR/run.sh -y /home/ciro/christian/scripts/cellranger_config_NV038.yaml -s
```

All results can be found [here](https://informaticsdata.liai.org/NGS_analyses/ad_hoc/Groups/vd-vijay/cramirez/christian/raw/NV038).
- Gene expression and antibody capture [results](https://informaticsdata.liai.org/NGS_analyses/ad_hoc/Groups/vd-vijay/cramirez/christian/raw/NV038/count).
- [TCR results](https://informaticsdata.liai.org/NGS_analyses/ad_hoc/Groups/vd-vijay/cramirez/christian/raw/NV038/vdj/).

## Demultiplexing donors/subjects
You will make use of the [ab_capture](https://github.com/vijaybioinfo/ab_capture) scripts.
1. Have the donor metadata if you want to include it in the single-cell metadata. Up yo you if you want it in the object or if you'll make use of it after clustering.
2. Check the hashtag structure is correct (`Rscript /home/ciro/scripts/ab_capture/summary.R -h`)
```
sh /home/ciro/scripts/ab_capture/demux.sh -y /home/ciro/scripts/ab_capture/config.yaml -v
Rscript /home/ciro/scripts/ab_capture/summary.R -c /home/ciro/large/christian/results/ab_demux/SiEs10_100th \
  --tag_str treatment~donor~hashtag_n~hashtag_id
```
Jobs will be created and you'll need to wait a few minutes.

## Quality control
There's a script for this [here](https://github.com/vijaybioinfo/quality_control).
1. Mind how you indicate the metadata.<sup>2</sup>

It takes about 5 minutes with 90K cells with 20gb and 1 node/1 processor.
```
Rscript /home/ciro/scripts/quality_control/single_cell.R -y /home/ciro/christian/scripts/SiEs10_qc.yaml
```

## Clustering analysis
This analysis is still a bit complex so you need to go to the master file to request jobs.
```
sh /home/ciro/christian/scripts/SiEs10_jobConstSeu_setting.sh
# sh /home/ciro/scripts/clustering/run.sh -y /home/ciro/scripts/clustering/config.yaml
```

## Differential gene expression analysis
```
sh /home/ciro/christian/scripts/SiEs10_dgea.sh
```

<sup>1: Critical step; it takes a lot of time and memory.

<sup>2: You can run this step in an interactive job.
