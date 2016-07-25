# Introduction

This project contains examples on how to work with [conda](http://conda.pydata.org/docs/), an open source package and environment management system.  The examples provided go over:

1. Installing conda
2. Package and environment management
3. Channels and Bioconda
4. Building packages, specifically for bioconda

# Install Conda

From http://conda.pydata.org/miniconda.html

```bash
wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
bash Miniconda2-latest-Linux-x86_64.sh

# Install client for anaconda cloud
# Removes warning message "Warning: somethings is wrong with binstar_client"
conda install anaconda-client # client for anaconda cloud
```

# Package Management

```bash
conda search scipy
conda install scipy
python -c "import scipy; print scipy.__version__" # print scipy module version
conda list scipy
conda remove scipy
conda list
```

# Environment Management

```bash
# Create new environment installing specific package and version
conda create --name oldmodules scipy=0.11.0

# Activate environment and check module version
source activate oldmodules
python -c "import scipy; print scipy.__version__"

# Deactivate environment
source deactivate
python -c "import scipy; print scipy.__version__"

# Display environments and locations of files
conda info --envs
ls $HOME/miniconda2/envs/oldmodules

# Display modules in environment
conda list --name oldmodules

# Save/re-load environments
conda list --name oldmodules --export > saved-environment.txt

# Re-construct same environemnt
conda create --name oldmodules2 --file saved-environment.txt

# copy environments
conda create --name oldmodulescopy --clone oldmodules
```

# Conda Channels

```bash
# Search and install from r channel
conda search -c https://conda.anaconda.org/r r-yaml
conda install -c https://conda.anaconda.org/r r-yaml

# Add r channel to defaults
conda config --add channels r
cat ~/.condarc
```

# Bioconda

Available at <https://bioconda.github.io/>.

```bash
conda config --add channels r
conda config --add channels bioconda

# Install art read simulator
conda search art
conda install art
art_illumina

# Install neptune
conda install neptune
neptune

# Install rgi (resistence gene identifier)
conda install rgi
rgi --help
```

# Building conda packages

Example fastqc recipie taken from <https://github.com/bioconda/bioconda-recipes/tree/master/recipes/fastqc>

```bash
# Install conda-build needed for building packages
conda install conda-build

# Builds package
conda build recipes/fastqc

# Install local package
conda install ~/miniconda2/conda-bld/linux-64/fastqc-0.11.5-1.tar.bz2

# Optionally upload package to anaconda cloud for sharing (requires account)
anaconda upload ~/miniconda2/conda-bld/linux-64/fastqc-0.11.5-1.tar.bz2
```

# Building bioconda packages

Instructions at <https://github.com/bioconda/bioconda-recipes>.  Bioconda automates process of building, testing, and uploading package to bioconda with github and travis-ci.

```bash
# Clone local copy of bioconda recipes
git clone https://github.com/bioconda/bioconda-recipes.git
cd bioconda-recipes/

# Create package in recipes/

# Build package (if dependencies in bioconda channel will need to add this channel)
conda build recipes/neptune

# Install docker for testing build in docker (can skip if docker installed)
curl -sSL https://get.docker.com/ | sh
sudo service docker start
sudo usermod -a -G docker `whoami`
# Close/re-open terminal to refresh groups

# Run build and test process in docker container
docker run -v `pwd`:/bioconda-recipes bioconda/bioconda-builder --packages neptune --env-matrix /bioconda-recipes/scripts/env_matrix.yml
```

Now push to github and create pull request.  See <https://github.com/bioconda/bioconda-recipes/pulls>.
