# ⚙️ Tube Bending Process Dataset

This repository contains a real-world dataset stored in a serialized Python pickle file: `experiments_process_and_results.pkl`. It includes sensor readings and experimental results from a rotary tube-bending process, designed for machine learning and signal analysis research.


The file is tracked using [Git Large File Storage (Git LFS)](https://git-lfs.com/) due to its size (~660 MB).


To load, explore, and visualize this dataset, please follow the steps below using Python. 

---

### Part 1: 📥 Import Required Libraries and Load the Dataset

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

### Part 2: 🔍 Choosing A Specific Experiment
#### 2.1 Load all data from an experiment as a dictionary

```python
# Experiment numbers range from 1 to 318, random selection:
experiment_number = 11

experiment_as_dictinary = loaded_dict[f'Exp_{experiment_number}']
 ```
#### 2.2- Load a specific data as a Pandas Dataframe from the experiment:
```python

# Options: ['geometry_features', 'geometry_result', 'load_machine_process', 'load_sensor_process', 'movement', 'machine_setting']
features_as_pandas_dataframe = experiment_as_dictinary['load_sensor_process']
```
features_as_pandas_dataframe:

|    |   Time_[s] |   SENSOR_MANDREL_AXIAL_Load_[kN] |   SENSOR_PRESSURE-DIE_LATERAL_1_Load_[kN] |   SENSOR_PRESSURE-DIE_LATERAL_2_Load_[kN] |
|---:|-----------:|---------------------------------:|------------------------------------------:|------------------------------------------:|
|  0 |       0    |                        0.103435  |                                 0.028831  |                              -0.000158929 |
|  1 |       0.01 |                        0.0984101 |                                 0.0281868 |                               0.00112951  |
|  2 |       0.02 |                        0.100085  |                                 0.0384943 |                               0.00177373  |
|  3 |       0.03 |                        0.0984101 |                                 0.0281868 |                              -0.000158929 |
|  4 |       0.04 |                        0.10511   |                                 0.0391385 |                               0.000485291 |


---
### 3- 📈 Plotting Experiment:
#### 3.1- Helper Functions For Plots:

```python
def extract_y_label(col_name: str) -> str:
    """
    Extracts signal type and unit for y-axis label.
    E.g., "MANDREL_AXIAL_Load_[kN]" -> "Load [kN]"
    """
    match = re.search(r'([A-Za-z0-9\- ]+_\[[^]]+\])$', col_name)
    return match.group(1).replace('_', ' ') if match else col_name.replace('_', ' ')

def extract_trace_name(col_name: str) -> str:
    """
    Extracts the part before the measurement and unit.
    E.g., "SENSOR_MANDREL_AXIAL_Load_[kN]" -> "SENSOR MANDREL AXIAL"
    """
    parts = col_name.split('_')
    for i in range(len(parts)):
        if re.match(r'^[A-Za-z0-9\-]+\_\[[^]]+\]$', '_'.join(parts[i:])):
            return ' '.join(parts[:i]).replace('_', ' ')
    return ' '.join(parts[:-2]).replace('_', ' ')



def multi_sensor_subplots(df: pd.DataFrame, 
                          save_fig=False, 
                          output_path="machine_process_flow.html") -> go.Figure:  
    
    time = df.iloc[:, 0]
    sensor_cols = df.columns[1:]

    numeric_cols = []
    for col in sensor_cols:
        try:
            df[col] = pd.to_numeric(df[col], errors='coerce')
            if df[col].notna().any():
                numeric_cols.append(col)
        except Exception:
            continue

    num_sensors = len(numeric_cols)

    # Subplot titles = descriptive sensor names (not units)
    subplot_titles = [extract_trace_name(col) for col in numeric_cols]

    fig = make_subplots(
        rows=num_sensors, cols=1,
        shared_xaxes=True,
        vertical_spacing=0.03,
        subplot_titles=subplot_titles
    )

    for i, col in enumerate(numeric_cols, start=1):
        y_values = df[col]
        y_min = y_values.min()
        y_max = y_values.max()
        margin = (y_max - y_min) * 0.1 if y_max != y_min else 1

        fig.add_trace(
            go.Scatter(
                x=time,
                y=y_values,
                mode='lines',
                name=extract_trace_name(col),  # for tooltip only
                line=dict(width=1)
            ),
            row=i, col=1
        )

        fig.update_yaxes(
            title_text=extract_y_label(col),  # this is the y-axis label on left
            range=[y_min - margin, y_max + margin],
            row=i, col=1
        )

    fig.update_xaxes(title_text='Time [s]', row=num_sensors, col=1)

    fig.update_layout(
        # title="Sensor Time Series - One per Subplot",
        height=300 * num_sensors,
        width=1400,
        showlegend=False,
        hovermode='x unified'
    )

    if save_fig:
        fig.write_html(output_path)
        print(f"Interactive subplot saved to {output_path}")

    return fig
```

#### 3.2- Plotting all features from a specific data:
```python
multi_sensor_subplots(features_as_pandas_dataframe, save_fig=True, output_path=f"Exp_{experiment_number}_all_sensors")```
```

#### 3.3- Plotting one feature from specific data:
```python
sensor_name = 'SENSOR_MANDREL_AXIAL_Load_[kN]'

multi_sensor_subplots(features_as_pandas_dataframe[['Time_[s]', sensor_name]], save_fig=True, output_path=f"Exp_{experiment_number}_{sensor_name}_sensors")

# Other options can be in this list:
print(list(features_as_pandas_dataframe.columns)[1:])
```

---

📄 This dataset is shared for preview/research purposes. The license will be added later.
