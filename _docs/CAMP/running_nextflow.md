---
title: How to run Nextflow on CAMP?
category: CAMP
order: 3
---

You need CAMP to run Nextflow pipelines. It is convenient to use a bash wrapper like the following one to start up a Nextflow pipeline:

```
## LOAD REQUIRED MODULES
ml purge
ml Nextflow/20.07.1
ml Singularity/3.4.2

## RUN PIPELINE
nextflow run path/to/pipeline_main.nf \
    --design input.csv \
    -resume
```

In this example, we first remove all pre-installed modules that could have been loaded so far (`ml purge`) and load the two modules that we need: Nextflow and [Singularity](https://en.wikipedia.org/wiki/Singularity_(software)). Singularity is a containerisation system used on computational clusters instead of Docker, because it is more secure than Docker. Docker images are automatically converted into the Singularity format, so you do not need to think about this. 

**Make sure that you use the latest version of Nextflow available on CAMP! It should be higher or equal to 20.07.1.** You can do it by first checking which versions of Nextflow are pre-installed on CAMP as modules (`ml spider Nextflow`) and then choosing the latest one. It also makes sense to check for the latest version of Singularity available on CAMP and use it.

Next, we execute the command `nextflow run path/to/pipeline_main.nf` which starts the pipeline described in the `path/to/pipeline_main.nf` Nextflow script and provide necessary parameters to Nextflow (one dash) and to the pipeline itself (double dash).

You also need to make sure that the Crick-specific Nextflow config is located in the same directory as the pipeline script (`` in our example above).

To run an nf-core pipeline on CAMP, you need the same kind of bash wrapper, but instead of a path to a Nextflow script, you need to provide the name of the pipeline (it will be downloaded by Nextflow automatically):

```
## LOAD REQUIRED MODULES
ml purge
ml Nextflow/20.07.1
ml Singularity/3.4.2

## RUN nf-core PIPELINE
nextflow run nf-core/chipseq \
    --design design_table.csv \
    --genome GRCm38 \
    --skipSpp \
    --skipDiffAnalysis \
    --pvalue 1e-3 \
    --email your.email@crick.ac.uk \
    -r 1.0.0
    -profile crick \
    -resume
```

In this example, the name of the pipeline is `nf-core/chipseq`, and we run its version `1.0.0`. To find out about possible Nextflow and pipeline-specific options that you can use when running an nf-core pipeline, please, see the [nf-core documentation](https://nf-co.re/usage/introduction) and the pipeline-specific documentation.