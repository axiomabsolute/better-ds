# Part 1 - Python Setup

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
5. Setup DVC
	- `dvc init`
	- `mkdir ~/data`
	- `pipenv run dvc remote add -d default ~/data`
6. Create the `data` directory
	- `mkdir data`
