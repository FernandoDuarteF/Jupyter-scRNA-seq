# Single Cell analysis using Scanpy and jupyter

Datasets from [this](https://www.nature.com/articles/s41586-021-03569-1) study.

GEO accession:
* [GSE171524](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE171524)

To obtain dataset use NCBI ftp site:

```
wget https://ftp.ncbi.nlm.nih.gov/geo/series/GSE171nnn/GSE171524/suppl/GSE171524_RAW.tar
tar -xvf GSE171524_RAW.tar
gunzip *.gz
```

**WE WILL NOT USE THE DATASETS FROM ABOVE**

It seems they only contain filtered counts. We need raw counts to remove empty droplets and estimate the ambient RNA in the droplets. We will use [this paper](https://www.cell.com/cancer-cell/fulltext/S1535-6108%2823%2900364-1#secsectitle0095) instead, which contains both filtered and unfiltered counts.

GEO accession:
* [GSE235063](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE235063)

And download the data from the **ftp ncbi site**:

```
wget https://ftp.ncbi.nlm.nih.gov/geo/series/GSE235nnn/GSE235063/suppl/GSE235063_RAW.tar
```


Let's create a conda environment were we will install outilities as well as the jupyter notebook:

```
conda create --name scanpy
conda activate scanpy
```

Now we will install the necessary utilities using pip:

```
# For R (we will take advantage of interoperability between Python and R inside the jupyter notebook):
conda install pip r-base
# In order to install rpy2
conda install gcc
# For our analysis platform and tools
pip install notebook scanpy
# For interoperability to work
pip install anndata2ri rpy2
# We will need these two packages to use the leiden algorithm from ``scannpy``
pip install igraph leidenalg
# And apparently this too (for leiden algorithm)
conda install -c conda-forge ipywidgets
```

To install ``DropletUtils``, for certain dependecies we will need ``ranlib``, we can obtain it with the following command:

```
conda install binutils
```

We will also use some R dependencies for estimating empty droplets and for infering the ambient RNA:

```
Rscript -e "install.packages(c('SoupX', 'BiocManager'), repos = 'http://cran.us.r-project.org')"
Rscript -e "BiocManager::install('DropletUtils')"

```

View below for the main packages versions used for this framework. Also check the list of pip packages and the R session.



Doublet detection: elbow plot of cell barcodes

Main difference of feature matrix between Bioconductor/Seurat and scanpy is that in the later it is transposed. ``obs`` are cell barcodes and ``var`` are gene identifiers.

Looks like DoubletFinder outperforms other methods (10.1016/j.cels.2020.11.008)

To install **SoupX** the packages below need to be installed:

```
sudo apt install libssl-dev libcurl4-openssl-dev
```

Now:

```
sudo apt install r-base-core
pip install anndata2ri rpy2
sudo Rscript -e "install.packages('SoupX')"
Rscript -e "BiocManager::install('DropletUtils')"
```

Conda packages:

* pip:24.0
* python:3.12.3
* r-base:4.3.3
* binutils:2.40
* gcc:13.2.0
* ipywidgets:8.1.2

pip packages (as listed by ``pip list``):

```
pip list

Package                   Version
------------------------- --------------
anndata                   0.10.7
anndata2ri                1.3.1
anyio                     4.3.0
argon2-cffi               23.1.0
argon2-cffi-bindings      21.2.0
array_api_compat          1.6
arrow                     1.3.0
asttokens                 2.4.1
async-lru                 2.0.4
attrs                     23.2.0
Babel                     2.14.0
beautifulsoup4            4.12.3
bleach                    6.1.0
certifi                   2024.2.2
cffi                      1.16.0
charset-normalizer        3.3.2
comm                      0.2.2
contourpy                 1.2.1
cycler                    0.12.1
debugpy                   1.8.1
decorator                 5.1.1
defusedxml                0.7.1
executing                 2.0.1
fastjsonschema            2.19.1
fonttools                 4.51.0
fqdn                      1.5.1
h11                       0.14.0
h5py                      3.11.0
httpcore                  1.0.5
httpx                     0.27.0
idna                      3.7
igraph                    0.11.4
ipykernel                 6.29.4
ipython                   8.23.0
isoduration               20.11.0
jedi                      0.19.1
Jinja2                    3.1.3
joblib                    1.4.0
json5                     0.9.25
jsonpointer               2.4
jsonschema                4.21.1
jsonschema-specifications 2023.12.1
jupyter_client            8.6.1
jupyter_core              5.7.2
jupyter-events            0.10.0
jupyter-lsp               2.2.5
jupyter_server            2.14.0
jupyter_server_terminals  0.5.3
jupyterlab                4.1.6
jupyterlab_pygments       0.3.0
jupyterlab_server         2.27.1
kiwisolver                1.4.5
legacy-api-wrap           1.4
leidenalg                 0.10.2
llvmlite                  0.42.0
MarkupSafe                2.1.5
matplotlib                3.8.4
matplotlib-inline         0.1.7
mistune                   3.0.2
natsort                   8.4.0
nbclient                  0.10.0
nbconvert                 7.16.3
nbformat                  5.10.4
nest-asyncio              1.6.0
networkx                  3.3
notebook                  7.1.3
notebook_shim             0.2.4
numba                     0.59.1
numpy                     1.26.4
overrides                 7.7.0
packaging                 24.0
pandas                    2.2.2
pandocfilters             1.5.1
parso                     0.8.4
patsy                     0.5.6
pexpect                   4.9.0
pillow                    10.3.0
pip                       24.0
platformdirs              4.2.1
prometheus_client         0.20.0
prompt-toolkit            3.0.43
psutil                    5.9.8
ptyprocess                0.7.0
pure-eval                 0.2.2
pycparser                 2.22
Pygments                  2.17.2
pynndescent               0.5.12
pyparsing                 3.1.2
python-dateutil           2.9.0.post0
python-json-logger        2.0.7
pytz                      2024.1
PyYAML                    6.0.1
pyzmq                     26.0.2
referencing               0.35.0
requests                  2.31.0
rfc3339-validator         0.1.4
rfc3986-validator         0.1.1
rpds-py                   0.18.0
rpy2                      3.5.16
scanpy                    1.10.1
scikit-learn              1.4.2
scipy                     1.13.0
seaborn                   0.13.2
Send2Trash                1.8.3
session_info              1.0.0
setuptools                69.5.1
six                       1.16.0
sniffio                   1.3.1
soupsieve                 2.5
stack-data                0.6.3
statsmodels               0.14.2
stdlib-list               0.10.0
terminado                 0.18.1
texttable                 1.7.0
threadpoolctl             3.4.0
tinycss2                  1.3.0
tornado                   6.4
tqdm                      4.66.2
traitlets                 5.14.3
types-python-dateutil     2.9.0.20240316
tzdata                    2024.1
tzlocal                   5.2
umap-learn                0.5.6
uri-template              1.3.0
urllib3                   2.2.1
wcwidth                   0.2.13
webcolors                 1.13
webencodings              0.5.1
websocket-client          1.8.0
wheel                     0.43.0
```
