# Install Conda

From http://conda.pydata.org/miniconda.html

```bash
wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
bash Miniconda2-latest-Linux-x86_64.sh
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

# Save/re-load environments
conda create --name oldmodules scipy=0.11.0

# Re-construct same environemnt
conda create --name oldmodules2 --file saved-environment.txt

# List all environments
conda info --envs
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
```

# Building conda packages

Example fastqc recipie taken from <https://github.com/bioconda/bioconda-recipes/tree/master/recipes/fastqc>

```bash
# Install conda-build needed for building packages
conda install conda-build

ls recipes/fastqc
conda build recipes/fastqc
```

# Building bioconda packages

Instructions at <https://github.com/bioconda/bioconda-recipes>.

```bash
# Clone local copy of bioconda recipes
git clone https://github.com/bioconda/bioconda-recipes.git
cd bioconda-recipes/

# Create package in recipes/

# Build package
conda build recipes/fastqc --channel bioconda

# Install docker for testing build in docker (can skip if docker installed)
curl -sSL https://get.docker.com/ | sh
sudo service docker start
sudo usermod -a -G docker `whoami`
# Close/re-open terminal to refresh groups

# Run build and test process in docker container
docker run -v `pwd`:/bioconda-recipes bioconda/bioconda-builder --packages fastqc --env-matrix /bioconda-recipes/scripts/env_matrix.yml
```

Now push to github and create pull request.
