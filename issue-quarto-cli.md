# Bug: conversion from ipynb to qmd removes first text cell(s)


I noticed that text disappeared when converting a Jupyter Notebook to a
Quarto Notebook. It appears to only happen to text cells that occur
before the first code cell.

``` python
!quarto --version
```

    1.6.40

To showcase this I will convert an `qmd` file to an `ipynb` file and
back again.

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

    ::: {.cell execution_count=2}
    ``` {.python .cell-code}
    import os
    ```
    :::


    Some more text.

    ::: {.cell execution_count=3}
    ``` {.python .cell-code}
    1 + 1
    ```

    ::: {.cell-output .cell-output-display execution_count=3}
    ```
    2
    ```
    :::
    :::

``` python
!quarto convert input-document.qmd
```

    Converted to input-document.ipynb

The output `ipynb` is as expected, see [here]().

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

    ::: {.cell execution_count=6}
    ``` {.python .cell-code}
    import os
    ```
    :::


    Some more text.

    ::: {.cell execution_count=7}
    ``` {.python .cell-code}
    1 + 1
    ```

    ::: {.cell-output .cell-output-display execution_count=7}
    ```
    2
    ```
    :::
    :::
