# Exercise 3: Run the nf-core/viralrecon pipeline

In Exercise 2, you used `sierra-local` to predict HIV drug resistance from a consensus sequence. In this exercise, you will start with raw sequencing reads and run HIV drug-resistance analysis as part of the `nf-core/viralrecon` Nextflow pipeline.

These instructions were written for GitHub Codespaces and use Singularity. On a local Linux computer or Windows with WSL, you can also use Singularity. On macOS, use Docker. Step 5 provides separate commands for Singularity and Docker; choose the one for your environment.

The HIV drug-resistance workflow used in this exercise is available on the `dev` branch. Karolinska Institutet, Karolinska University Laboratory, and the Bioinformatics Unit of Instituto de Salud Carlos III collaborated on the integration. [Sarai Varona](https://github.com/svarona) and [Lili Andersson-Li](https://github.com/LilyAnderssonLee) carried out the original work.

The HIV drug-resistance workflow in `nf-core/viralrecon` supports Illumina sequencing data only. [nf-core/viralrecon documentation](https://nf-co.re/viralrecon/dev/docs/output/#illumina-hiv-resistance-detection)

## Test data

The exercise uses three publicly available samples that are also included in the [Stanford HIVdb sequencing-read examples](https://hivdb.stanford.edu/hivdb/by-reads/).

| Accession    | Sequencing target                          |
| ---          | ---                                        |
| `DRR030302`  | Whole-genome amplicons                     |
| `SRR4071760` | Protease and reverse transcriptase regions |
| `SRR6937100` | Integrase region                           |

The pipeline's test dataset retains **0.02%** of the original reads from each sample. [Test data sampling procedure](https://github.com/nf-core/test-datasets/tree/viralrecon#sampling-procedure)

## Run the analysis with the `test_hiv` profile

The `test_hiv` profile provides the sample sheet and parameters for this exercise. Step 5 provides commands for both Singularity and Docker. Use Singularity for GitHub Codespaces, Linux, or Windows with WSL, or Docker for macOS.

1. Return to the `ex1` folder containing `pixi.toml`. If you are continuing directly from Exercise 2, your current directory is `ex2/sierra-local`, so run:

   ```bash
   cd ../../ex1
   ```

   If you are starting in a new terminal, navigate to `ex1` from your chosen working directory instead.

2. Activate the Pixi environment if it is not already active. If your prompt already shows `(ex1)`, skip this command.

   ```bash
   pixi shell
   ```

   Check the Singularity installation from Exercise 1:

   ```bash
   singularity --version
   ```

   If you are using Docker locally, skip only the Singularity check and continue with the steps below. Make sure Docker Desktop is running.

   After activating the environment, move up one directory so you can create `ex3` alongside `ex1`. Keep the Pixi environment active for the rest of the exercise.

   ```bash
   cd ..
   ```

3. Create a working directory and clone the `dev` branch of the pipeline. Run the remaining commands from `ex3`, outside the cloned `viralrecon` directory.

   ```bash
   mkdir -p ex3
   cd ex3
   git clone --branch dev https://github.com/nf-core/viralrecon.git
   ```

   The profile's settings are in [`conf/test_hiv.config`](https://github.com/nf-core/viralrecon/blob/dev/conf/test_hiv.config).

4. If you are using GitHub Codespaces, create `custom.config` to limit the CPU, memory, and time available to each task.

   ```bash
   cat > custom.config <<'CONFIG'
   process {
       resourceLimits = [
           cpus: 2,
           memory: '6.GB',
           time: '1.h'
       ]
   }
   CONFIG
   ```

   If your environment has sufficient resources, you can skip this step and delete `-c custom.config` from the command you choose below.

5. Choose one of the following commands. Both use the `test_hiv` profile and write results to `results_test/`.

   Run the local checkout you cloned in step 3:

   **GitHub Codespaces, Linux, or Windows with WSL — Singularity**

   ```bash
   nextflow run viralrecon/main.nf -profile test_hiv,singularity --outdir results_test -c custom.config
   ```

   **macOS — Docker**

   Make sure Docker Desktop is running.

   ```bash
   nextflow run viralrecon/main.nf -profile test_hiv,docker --outdir results_test -c custom.config
   ```

   If you skipped creating `custom.config`, delete `-c custom.config` from the command you choose.

   Alternatively, you can run the pipeline directly from GitHub by replacing `viralrecon/main.nf` with `nf-core/viralrecon -r dev` in either command above. For example, with Singularity:

   ```bash
   nextflow run nf-core/viralrecon -r dev -profile test_hiv,singularity --outdir results_test -c custom.config
   ```

   Using `custom.config`, the analysis took approximately 10 minutes on a local Mac and 20 minutes in GitHub Codespaces. Runtime may vary depending on the computer and available resources.

   **Use the updated HIVdb files**

   The commands above use the default HIVdb files bundled with `sierra-local`. To use the files you updated in Exercise 2, provide their paths as pipeline parameters:

   **GitHub Codespaces, Linux, or Windows with WSL — Singularity**

   ```bash
   nextflow run viralrecon/main.nf -profile test_hiv,singularity \
       --outdir results_test -c custom.config \
       --hivdb_xml ../ex2/sierra-local/sierralocal/data/HIVDB_10.2.xml \
       --apobec_drm ../ex2/sierra-local/sierralocal/data/apobec_drms.json \
       --apobec_csv ../ex2/sierra-local/sierralocal/data/apobecs.csv \
       --unusual_csv ../ex2/sierra-local/sierralocal/data/rx-all_subtype-all.csv \
       --sdrms_csv ../ex2/sierra-local/sierralocal/data/sdrms_hiv1.csv \
       --mutation_csv ../ex2/sierra-local/sierralocal/data/mutation-type-pairs_hiv1.csv
   ```

   **macOS — Docker**

   ```bash
   nextflow run viralrecon/main.nf -profile test_hiv,docker \
       --outdir results_test -c custom.config \
       --hivdb_xml ../ex2/sierra-local/sierralocal/data/HIVDB_10.2.xml \
       --apobec_drm ../ex2/sierra-local/sierralocal/data/apobec_drms.json \
       --apobec_csv ../ex2/sierra-local/sierralocal/data/apobecs.csv \
       --unusual_csv ../ex2/sierra-local/sierralocal/data/rx-all_subtype-all.csv \
       --sdrms_csv ../ex2/sierra-local/sierralocal/data/sdrms_hiv1.csv \
       --mutation_csv ../ex2/sierra-local/sierralocal/data/mutation-type-pairs_hiv1.csv
   ```

   If you skipped creating `custom.config`, delete `-c custom.config` from the command you choose.

   These paths assume you saved the updated HIVdb files in `ex2/sierra-local/sierralocal/data`, as described in Exercise 2. Adjust the paths and XML filename if you used a different location or downloaded a different HIVdb version.

6. To resume a run, repeat the same command from the same directory and add `-resume`. Nextflow reuses results from completed tasks when their cache entries and outputs are still available. [How Nextflow resume works](https://seqera.io/blog/demystifying-nextflow-resume/)

## Inspect the results

After the pipeline finishes successfully, explore the results. The analysis generates many folders and files, which can feel overwhelming at first.

A good place to start is the HIV drug-resistance HTML reports saved under `results_test/variants/ivar/consensus/ivar/hiv_resistance/resistance_reports/*html`

If you are using GitHub Codespaces, download the report and open it in a web browser (opening it in the VS Code editor shows the HTML source code).

Feel free to explore the result files, but don’t worry if they seem confusing. We will go through the outputs together in Session 2 on 8 December.

In VS Code, expand `results_test` in the Explorer panel on the left to browse the files.

Alternatively, display a brief overview in the terminal:

```bash
tree -L 2 results_test
```

The `-L 2` option limits the display to two directory levels.

Congratulations on completing the exercise! Give yourself a round of applause!

## Take-home messages

You have learned how to run the `nf-core/viralrecon` pipeline for HIV drug-resistance testing and provide updated HIVdb files on the command line.

## Bonus

The bonus tips below can help you use `nf-core/viralrecon` in future analyses.

### Run the analysis on your own samples

The `hiv` profile loads the recommended settings for HIV drug-resistance analysis from `conf/hiv.config`. Replace `your_samplesheet.csv` with the path to your sample sheet. The example writes results to `results_hiv/`.

```bash
nextflow run nf-core/viralrecon -r dev -profile hiv,singularity \
    --input your_samplesheet.csv \
    --outdir results_hiv
```

To adjust CPU, memory, or time limits for your dataset, put the settings in a custom configuration file and pass its path with `-c custom.config`. For details, please check [nf-core configuration options](https://nf-co.re/docs/running/configuration/configuration-options)

For help preparing a samplesheet and choosing HIV-specific parameters, see the pipeline documentation [nf-core/viralrecon HIV usage guide](https://github.com/nf-core/viralrecon/blob/dev/docs/usage.md#hiv)

### Run the analysis with a parameter file

You can save pipeline parameters in a YAML file and pass it to Nextflow with `-params-file`. Create a file named `example_params.yaml` in `ex3` and copy the contents of the [example parameter file](https://github.com/kclinmicro/hiv-drug-resistance-course/blob/main/exercises/03/example_params.yaml) into it. Replace the placeholder paths with paths to your own files, then run the command below from `ex3`.

```bash
nextflow run nf-core/viralrecon -r dev -profile hiv,singularity -params-file example_params.yaml
```

## Get help

For questions about the pipeline, use the `#viralrecon` channel on the nf-core Slack workspace. [Join nf-core Slack](https://nf-co.re/join/slack) · [Open #viralrecon](https://nfcore.slack.com/channels/viralrecon)
