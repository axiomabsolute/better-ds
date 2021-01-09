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
3. Saving the data in the cache
    - Both raw data and intermediate pipeline steps can be stored in a cache. For now, we've set up a cache on the local machine in the `~/data` directory. In practice, this would usually be either a network attached storage device or a cloud hosted storage service like S3.
    - To save the files, run `pipenv run dvc push`
4. Demonstrating reproducibility
    - Now we're going to do something interesting: erase our data and reproduce it.
    - First, run `git clean -xdf`. **This is reset your local repository to how it was when you cloned it**, including removing any un-tracked files. Note that `AtomicCards.json` and `AtomicCards.json.bz2` should both be removed.
    - Now run `pipenv run dvc pull`. This should pull both files from the local cache.
    - Run `rm data/AtomicCards.json` to remove the version of the file from the cache
    - Run `pipenv run dvc repro` - this should re-generate the file from the source raw data.
    - Why jump through all of these hoops with caching and defining pipelines? Why not just re-fetch from URL we originally imported from? Why not just use Make to define how data is derived? DVC takes care to ensure that our pipelines are reproducible, including tracking not just where data comes from, but whether that data changed. Imagine if the authors of `mtgjson.com` decided to suddenly change their data format, or just remove their site from the internet. Suddenly our pipeline is no longer reproducible! Using a remote cache for external data dependencies isn't just about speeding up the development experience through caching, but about insulating our project from external data changes.
