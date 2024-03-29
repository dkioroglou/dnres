# dnres: data n results

[![Documentation Status](https://readthedocs.org/projects/pip/badge/?version=stable)](https://pip.pypa.io/en/stable/?badge=stable)

`dnres` is a python package for managing and sharing data and results generated from any type of data analysis. It is a tagging system that facilitates modular type of analysis by allowing easy tagging, storing and loading of python objects or files across different python scripts.

## Usage

### Configuration file

`dnres` requires a configuration file. In this file, two sections should be specified: **PATHS**, and **INFO**. In the section **PATHS** the keys **structure** and **database** are mandatory. In the section **INFO** the key **description** is mandatory.

Example of configuration file with filename `config.ini`:
```python
[PATHS]
structure = foo/bar
database = foo/foo/bar

[INFO]
description = "This is the description of the analysis related to the data and results."
```

The configuration file is passed as argument upon instantiation of the `DnRes`
```python
from dnres import DnRes

res = DnRes('config.ini')
```

### Storing and loading of python objects

Example of storing a list in analytical script `script_01.py` 

```python
from dnres import DnRes

res = DnRes('config.ini')

# Create some data
x = [1,2,3]

# Store data to use in another analytical script
res.store(data=x,
          tag='variousLists',
          path='dir1/mylist.pickle',
          description='List with three numbers.',
          source='script_01.py'
         )
```
Valid serializations are `json` or `pickle`. Serialization if taken from the extension of the path (`.json` or `.pickle`).


Example of loading stored data from `script_01.py` in `script_02.py`:

```python
from dnres import DnRes

res = DnRes('config.ini')

# Show available tagged data
print(res)

# Load tagged data
x = res.load('dir1/mylist.pickle')
```

### Tagging of files or directories

Tagging is only available for files or directories that are inside the `structure`.
Example of tagging a `.csv` file in analytical script `script_01.py`

```python
from dnres import DnRes

res = DnRes('config.ini')

filepath = 'dir1/foo.csv'
res.tag('someTag', filepath)
```

Load in `script_02.py` the `.csv` filepath that was tagged in `script_01.py`.

```python
from dnres import DnRes
import pandas as pd

res = DnRes('config.ini')

# load() method returns absolute filepath when stored data is not python object.
filepath = res.load('dir1/foo.csv')
df = pd.read_csv(filepath, sep='\t')
```

## Documentation

For complete package reference visit [dnres.readthedocs](https://dnres.readthedocs.io/en/latest/source/dnres.html).

## Installation

```bash
pip install dnres
```

## CLI utility

`dnres-cli` can be found [here](https://github.com/DKioroglou/dnres-cli).

## License

BSD 3

