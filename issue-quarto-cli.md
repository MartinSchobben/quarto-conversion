# Bug: conversion from ipynb to qmd removes first text cell(s)


I noticed that text disappeared when converting a Jupyter Notebook to a
Quarto Notebook. It appears to only happen to text cells that occur
before the first code cell.

``` python
!quarto --version
```

    1.6.40

``` python
!quarto check
```

    Quarto 1.6.40
    [✓] Checking environment information...
          Quarto cache location: /home/nicola/.cache/quarto
    [✓] Checking versions of quarto binary dependencies...
          Pandoc version 3.4.0: OK
          Dart Sass version 1.70.0: OK
          Deno version 1.46.3: OK
          Typst version 0.11.0: OK
    [✓] Checking versions of quarto dependencies......OK
    [✓] Checking Quarto installation......OK
          Version: 1.6.40
          Path: /opt/quarto/bin

    [✓] Checking tools....................OK
          TinyTeX: (external install)
          Chromium: (not installed)

    (|) Checking LaTeX....................(/) Checking LaTeX....................[✓] Checking LaTeX....................OK
          Using: TinyTex
          Path: /home/nicola/.TinyTeX/bin/x86_64-linux
          Version: 2024

    (|) Checking basic markdown render....(/) Checking basic markdown render....(-) Checking basic markdown render....(\) Checking basic markdown render....(|) Checking basic markdown render....(/) Checking basic markdown render....[✓] Checking basic markdown render....OK

    [✓] Checking Python 3 installation....OK
          Version: 3.12.9 (Conda)
          Path: /home/nicola/miniconda3/envs/quarto-cli/bin/python
          Jupyter: 5.7.2
          Kernels: python3, 02_floodmapping, pangeo-workflow-examples, floodmapping, 04_l-band-sar, mrs-env, tuw_education_notebooks, 01_classification, classification, microwave-remote-sensing

    (|) Checking Jupyter engine render....(/) Checking Jupyter engine render....(-) Checking Jupyter engine render....(\) Checking Jupyter engine render....(|) Checking Jupyter engine render....(/) Checking Jupyter engine render....(-) Checking Jupyter engine render....(\) Checking Jupyter engine render....(|) Checking Jupyter engine render....(/) Checking Jupyter engine render....(-) Checking Jupyter engine render....(\) Checking Jupyter engine render....(|) Checking Jupyter engine render....(/) Checking Jupyter engine render....(-) Checking Jupyter engine render....[✓] Checking Jupyter engine render....OK

    (|) Checking R installation...........(/) Checking R installation...........[✓] Checking R installation...........OK
          Version: 4.4.2
          Path: /usr/lib/R
          LibPaths:
            - /home/nicola/R/x86_64-pc-linux-gnu-library/4.4
            - /usr/local/lib/R/site-library
            - /usr/lib/R/site-library
            - /usr/lib/R/library
          knitr: 1.45
          rmarkdown: 2.25

    (|) Checking Knitr engine render......(/) Checking Knitr engine render......(-) Checking Knitr engine render......(\) Checking Knitr engine render......(|) Checking Knitr engine render......(/) Checking Knitr engine render......(-) Checking Knitr engine render......[✓] Checking Knitr engine render......OK

To showcase this I will convert an `qmd` file to an `ipynb` file and
back again. Check source files in this [GitHub
repo](https://github.com/MartinSchobben/quarto-conversion/tree/main).

This is the starting point:

    ---
    title: "My Document"
    format: html
    jupyter: python3
    keep-ipynb: true
    ---

    ## Introduction

    Here I place some text.

    ## Results

    ::: {.cell execution_count=3}
    ``` {.python .cell-code}
    import os
    ```
    :::


    Some more text.

    ::: {.cell execution_count=4}
    ``` {.python .cell-code}
    1 + 1
    ```

    ::: {.cell-output .cell-output-display execution_count=4}
    ```
    2
    ```
    :::
    :::

``` python
!quarto convert input-document.qmd
```

    Converted to input-document.ipynb

The output `ipynb` is as expected, see
[here](https://github.com/MartinSchobben/quarto-conversion/blob/main/input-document.ipynb).

Here one can see that some text is missing when I now `convert` this
back to a `qmd` file.

``` python
!quarto convert input-document.ipynb --output output-document.qmd
```

    Converted to output-document.qmd

    ---
    title: My Document
    format: html
    jupyter: python3
    keep-ipynb: true
    ---

    ::: {.cell execution_count=7}
    ``` {.python .cell-code}
    import os
    ```
    :::


    Some more text.

    ::: {.cell execution_count=8}
    ``` {.python .cell-code}
    1 + 1
    ```

    ::: {.cell-output .cell-output-display execution_count=8}
    ```
    2
    ```
    :::
    :::
