# Exercise 1: Installation

Install tools needed for the exercises in the course:

- [nextflow](https://github.com/nextflow-io/nextflow)
- [sierra-local](https://github.com/PoonLab/sierra-local)
- Singularity (for running pipeline containers on Linux, WSL, or GitHub Codespaces)

*(Note that you can view an outline of this document by clicking the icon that
looks like a bullet point list, to the far right just above the header above)*

## Dependencies

This guide assumes you have access to a Linux environment, as highlighted in
the [preparations information](https://learning.ecdc.europa.eu/course/view.php?id=1258#section-2).

HIV drug-resistance testing will be performed using `sierra-local`, which is integrated into the `nf-core/viralrecon` Nextflow pipeline.
To run `sierra-local` and `nf-core/viralrecon`, you also need to set up a Conda or Pixi environment.

In this guide, we will go with Pixi, as it is, in our experience, faster and more
robust than Conda.

## Setting up nextflow and sierra-local from Bioconda with Pixi

Below is a guide on how to set up tools through the bioconda repository, but
using Pixi instead of Conda, for greater speed and flexibility.

1. Install Pixi
  - The easiest way is to run the official install script from the [webpage](https://prefix.dev):

    ```bash
    curl -fsSL https://pixi.sh/install.sh | bash
    ```

    But there are [alternative installation methods](https://pixi.prefix.dev/latest/installation/#alternative-installation-methods) too.

2. Reload your shell, for example by executing bash again:

    ```bash
    bash
    ```

3. From your chosen working directory, create a folder for Exercise 1. The later exercises will create `ex2` and `ex3` alongside `ex1`:

    ```bash
    mkdir -p ex1
    cd ex1
    ```

4. Create a new pixi environment in this folder:

    ```bash
    pixi init -c conda-forge -c bioconda
    ```

   This will create a file called `pixi.toml` inside the current folder.

5. You can now run `pixi shell`,
   so that all installed software is available in your `$PATH` variable:

   ```bash
   pixi shell
   ```

   You can see that you have the pixi environment loaded by the `(ex1)` part
   added to the left in your terminal.

6. Now you can install Nextflow, sierra-local, and some other required
   tools via Pixi:

    ```bash
    pixi add nextflow=25.10.4 sierra-local=0.4.4 python=3.10 tree singularity
    ```

    This command includes Singularity for Linux, WSL, and GitHub Codespaces. If you are running locally on a Mac, omit `singularity` and use Docker for Exercise 3.

    (As you can see, you can specify the version by appending
    `=<version>` to a tool name).

7. Done! Now you should be able to check that `nextflow` and `sierra-local` are installed with:

    ```bash
    nextflow -h
    sierralocal -h
    ```

    On Linux, WSL, or GitHub Codespaces, also check Singularity:

    ```bash
    singularity --version
    ```

8. If you are taking a break, exit the Pixi environment with the command below. You can exit from any directory. If you are continuing directly to Exercise 2, you can keep it active. Exercise 2 starts from the `ex1` folder.

    ```bash
    exit
    ```

## Take home message

You have now learned how to use `pixi` to install and manage software
for the exercises.
