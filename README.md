# tubebend
A real-world dataset of rotary tube bending machine settings, processes, and results for predicting the quality results of rotary draw bending and springback.


# 📁 Tube Bending Process Dataset

This repository contains a dataset stored in a serialized Python pickle file: `experiments_process_and_results.pkl`. It includes sensor readings and experimental results from a tube-bending process, designed for machine learning and signal analysis research.

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

# Load the dictionary from the file
with open('experiments_process_and_results.pkl', 'rb') as f:
    loaded_dict = pickle.load(f)
 ```

---

### Part 2: 📥 Loading Dataset
## 2.1 Load all data from an experiment as a dictionary

```python
experiment_as_dictinary = loaded_dict['Exp_11']
 ```




This dataset is shared for preview/research purposes. The license will be added later.
