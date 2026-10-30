# nf-core/configs: CBE Configuration

All nf-core pipelines have been successfully configured for use on CBE.next, the CLIP Batch Environment cluster at the Vienna BioCenter (VBC).

To use it, run the pipeline with `-profile cbe`. This downloads and loads [`cbe.config`](../conf/cbe.config), which submits each task as a Slurm job and runs it in a Singularity (Apptainer) container:

- Tasks run on partition `c`. Tasks that need more than 740 GB of memory run on partition `m`.
- The QOS follows the task's time limit: `rapid` up to 1 hour, `short` up to 8 hours, `medium` up to 2 days, and `long` up to 14 days.
- Processes with the label `process_gpu` get one NVIDIA B200 MIG instance (23 GB) on partition `g`.
- `--genome` uses the local copy of [AWS iGenomes](https://registry.opendata.aws/aws-igenomes/) in `/resources/references/igenomes`.

Load Nextflow from the environment modules, and keep container images in your group directory:

```bash
module load Nextflow/26.04.0
export NXF_SINGULARITY_CACHEDIR=/groups/<group>/nextflow/singularity
mkdir -p "$NXF_SINGULARITY_CACHEDIR"
```

Run Nextflow itself in a Slurm batch job, not on the login node, and put the work directory on scratch:

```bash
nextflow run nf-core/<pipeline> -r <version> -profile cbe \
    -work-dir "$SCRATCHDIR/nextflow/work" --outdir /groups/<group>/results
```

See the [CBE.next Nextflow documentation](https://docs.vbc.ac.at/research-computing/cbe/nextflow/) for a complete job script, resuming runs, and debugging.

> [!NOTE]
> You need an account on CBE.next to run the pipeline. If in doubt, contact the VBC IT Service Desk.

> [!NOTE]
> This profile is for CBE.next. The legacy CBE cluster shuts down on 2026-12-18. Until then, legacy CBE users can use the previous version of this profile with `--custom_config_version 32a6072f`.
