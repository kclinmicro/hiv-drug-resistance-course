# Exercise 2: Run sierra-local and check the HIV drug resistance database

[sierra-local](https://github.com/PoonLab/sierra-local) is a local implementation of the Stanford HIVdb algorithm for mutation-based resistance scoring using consensus sequences.

## Clone sierra-local

`sierra-local` uses the [Stanford HIVdb](https://hivdb.stanford.edu/hivdb/by-patterns/) algorithm and supporting data to interpret HIV drug resistance. Clone the repository so that you can explore these files and download test sequences.

1. Start in the folder where you set up the Pixi environment in Exercise 1. If the environment is not active, run `pixi shell` in that folder first. Then create an `ex2` folder and enter it. Keep the Pixi environment active; changing folders does not deactivate it:

    ```bash
    mkdir ex2
    cd ex2
    ```

2. Clone the `sierra-local` repository at release tag `v0.4.4`:

    ```bash
    git clone --branch v0.4.4 https://github.com/PoonLab/sierra-local.git
    ```

## Prepare the test data

1. Enter the cloned repository and use its download script to retrieve reverse transcriptase (RT) test sequences:

    ```bash
    cd sierra-local
    python scripts/retrieve_hivdb_data.py RT RT.fa
    ```

## Update the HIV drug resistance database

1. From inside the `sierra-local` folder, download the latest HIVdb algorithm and supporting data:

    ```bash
    python sierralocal/updater.py
    ```

    The downloaded files are saved in `sierralocal/data` inside the cloned repository.

2. Open the `sierralocal/data` folder in the file explorer on the left side of the window and inspect the downloaded files. These include:

    ```text
    HIVDB_10.2.xml                  - XML file containing the HIVdb algorithm.
    apobec_drms.json                - JSON file containing the APOBEC DRM data.
    apobecs.csv                    - CSV file containing the APOBEC mutations.
    mutation-type-pairs_hiv1.csv    - CSV file containing the mutation type pairs.
    rx-all_subtype-all.csv          - CSV file containing the unusual mutations.
    sdrms_hiv1.csv                  - CSV file containing the SDRM mutations.
    ```

    The XML filename includes the algorithm version. check the filename you downloaded and use it in the analysis command below.

## Run sierra-local

1. View the available command-line options with `sierralocal -h`. You can also read the
   [online README](https://github.com/PoonLab/sierra-local/tree/v0.4.4) or the `README.md` file in the cloned repository. The help output looks like this:

    ```console
    $ sierralocal -h
    usage: sierralocal [-h] [-o OUTFILE] [-xml XML] [-json JSON] [--cleanup] [--forceupdate] [-alignment {post,nuc}] [-apobec_csv APOBEC_CSV] [-unusual_csv UNUSUAL_CSV] [-sdrms_csv SDRMS_CSV] [-mutation_csv MUTATION_CSV]
                       [-updater_outdir UPDATER_OUTDIR]
                       fasta [fasta ...]

    Local execution of Stanford HIVdb algorithm for mutation-based resistance scoring of sequences.

    positional arguments:
      fasta                 List of input files.

    options:
      -h, --help            show this help message and exit
      -o OUTFILE            Output filename.
      -xml XML              <optional> Path to HIVdb ASI2 XML file (default: HIVDB_9.4.xml)
      -json JSON            <optional> Path to JSON HIVdb APOBEC DRM file
      --cleanup             Deletes NucAmino alignment file after processing.
      --forceupdate         Forces update of HIVdb algorithm. Requires network connection.
      -alignment {post,nuc}
                            Alignment program to use, "post" for post align and "nuc" for nucamino
      -apobec_csv APOBEC_CSV
                            <optional> Path to CSV APOBEC csv file (default: apobecs.csv)
      -unusual_csv UNUSUAL_CSV
                            <optional> Path to CSV file to determine if is unusual (default: rx-all_subtype-all.csv)
      -sdrms_csv SDRMS_CSV  <optional> Path to CSV file to determine SDRM mutations (default: sdrms_hiv1.csv)
      -mutation_csv MUTATION_CSV
                            <optional> Path to CSV file to determine mutation type (default: mutation-type-pairs_hiv1.csv)
      -updater_outdir UPDATER_OUTDIR
                            <optional> Path to folder to store updated files from updater (default: sierralocal/data folder)
    ```


2. Run `sierra-local` using **one** of the following options. The bundled database may be older than the latest release, so use the files you downloaded above or update them as part of the analysis.

    **Option 1: Use the files you downloaded above.**

    This follows directly from the previous step. Replace `HIVDB_10.2.xml` with the downloaded XML filename if it differs.

    ```bash
    sierralocal RT.fa -o RT_results.json \
        -xml sierralocal/data/HIVDB_10.2.xml \
        -json sierralocal/data/apobec_drms.json \
        -apobec_csv sierralocal/data/apobecs.csv \
        -unusual_csv sierralocal/data/rx-all_subtype-all.csv \
        -sdrms_csv sierralocal/data/sdrms_hiv1.csv \
        -mutation_csv sierralocal/data/mutation-type-pairs_hiv1.csv
    ```

    **Option 2: Download the latest files during the analysis.**

    As an alternative, use `--forceupdate` to download the latest algorithm and supporting data. The `-updater_outdir` option saves these files in a separate folder called `hivdb_latest`.

    ```bash
    sierralocal --forceupdate -updater_outdir hivdb_latest RT.fa -o RT_results.json
    ```

3. Open `RT_results.json` in the file explorer on the left side of the window, or view it in the terminal with `less`:

    ```bash
    less -S RT_results.json
    ```

    The `-S` option prevents long lines from wrapping. Use the left and right arrow keys to scroll horizontally, and press `q` to quit.

At the end of this exercise, you should still be inside the cloned `sierra-local` folder (`ex2/sierra-local`), with the Pixi environment active.

## Take-home messages

You have learned how to run `sierra-local` on consensus sequences and select the HIVdb algorithm and supporting data used for resistance interpretation.

Use the latest algorithm and supporting data for new analyses. To reproduce a previous analysis, use the same versions as the original analysis. Always record the versions used.
