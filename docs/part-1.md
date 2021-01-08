# Part 1 - Python Setup

This part will focus on setting up the host system and repository for hosting a Python-based data science project. We'll set up a number of tools, which are briefly described below.

- `pyenv` - A Python version manager. Pyenv takes care of installing Python versions and makes it easy to switch between versions for different projects.
- `pipenv` - Pipenv is a Python project manager that builds on the standard Python packaging tool (`pip`) to safely install dependencies using a locking strategy to pin particular version numbers, resulting in more stable environment definitions. It works with `pyenv` to ensure that the proper version of Python is installed for the project.
- `dvc` - DVC is a Data Version Control system. While installed via Python, it is a language agnostic tool for managing, versioning, and defining pipelines for processing that data. DVC can be used to safely include large data files that normally cause problems for version control systems like Git.

The goal of a project using these tools is to minimize how much time contributors spend configuring their environment and to produce reproducible workflows. Once the host system is setup to use these tools, contributors should be able to get started with your project with only two commands:

1. `pipenv install`
2. `dvc repro`

And that's it! DVC takes care of fetching data from remote sources, caching that data locally, running the data through various pipelines, and tracking the results. We'll dig deeper into DVC in the comming parts, but for now let's get everything set up.


## Project Steps

1. Install Pyenv
	- `brew install pyenv` to install via system Python on Mac
	- `pip install pyenv` elsewhere
2. Install Pipenv
	- `pip install pipenv`
3. Create a Pipfile
	- `pipenv --python 3.9.1`
4. Install DVC
	- `pipenv install dvc`
	- Note: this process may take a while. Pipenv is a little slow, but produces very stable environments.
5. Setup DVC. DVC requires a location to cache data added from external sources. This storage location can be a cloud-hosted file service like S3, a shared network drive, or even just a dedicated directory on the local machine. For this demo, we'll set up a `data` directory in the current user's home to act as the default remote. Later we'll demonstrate how to utilize non-local remotes.
	- `dvc init`
	- `mkdir ~/data`
	- `pipenv run dvc remote add -d default ~/data`
6. Create the `data` directory. References to externally fetched data will be stored here.
	- `mkdir data`
