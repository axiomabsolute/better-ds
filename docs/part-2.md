# Part Two - Getting started with DVC and data

## Project Steps

1. Add [AtomicCards from MTG JSON](https://mtgjson.com/downloads/all-files/) dataset
    - `pipenv run dvc import-url https://mtgjson.com/api/v5/AtomicCards.json.bz2 data`
    - After running this command, `dvc` will output what to do next: `git add data/.gitignore data/AtomicCards.json.bz2.dvc`
2. Decompress Atomic Cards data
    - We'll use the commandline utility bzip2 to decompress the data
    - We *could* do this as a one-off command, then save the decompressed data, but we want to capture the transformations applied to the raw data to ensure our project is reproducible.
    - DVC defines transformations as *stages*, which can be connected together to form *pipelines*
    - To create a stage to decompress the data, run
        ```
        pipenv run dvc run -n decompress_atomic \
            -d data/AtomicCards.json.bz2 \
            -o data/AtomicCards.json \
            bzip2 --decompress --keep data/AtomicCards.json.bz2
        ```
    - Once again, `dvc` helpfully tells us what to do next: `git add dvc.lock data/.gitignore dvc.yaml`
    - Under the covers, <!-- TODO --> DVC creates a `dvc.yaml` file, describing each stage and how stages are related to one another, forming a DAG. The `dvc.lock` file tracks derived results and allows DVC to detect when data-level changes occur.
