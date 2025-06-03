# 📁 Tube Bending Process Dataset

This repository contains a real-world dataset stored in a serialized Python pickle file: `experiments_process_and_results.pkl`. It includes sensor readings and experimental results from a rotary tube-bending process, designed for machine learning and signal analysis research.


The file is tracked using [Git Large File Storage (Git LFS)](https://git-lfs.com/) due to its size (~660 MB).

---

## 🧪 How to Use This Dataset

Follow these steps to load, explore, and visualize the data in Python.

---

### Part 1: 🧰 Import Required Libraries and Load the Dataset

```python
import pickle
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import pandas as pd
import re

import warnings
warnings.filterwarnings('ignore')

with open('experiments_process_and_results.pkl', 'rb') as f:
    loaded_dict = pickle.load(f)
 ```

---

### Part 2: 📥 Loading Dataset
## 2.1 Load all data from an experiment as a dictionary

```python
experiment_as_dictinary = loaded_dict['Exp_11']
 ```
## 2.2- Load a specific data as a Pandas Dataframe from the experiment:
```python
features_as_pandas_dataframe = experiment_as_dictinary['load_sensor_process']

# To see all options:
print(list(experiment_as_dictinary.keys()))
```

This dataset is shared for preview/research purposes. The license will be added later.
