# Crop Classification DL Project

This repository contains the code and data pipeline for classifying crop types in Cyprus using Sentinel-2 time-series data and street-level imagery. Two hybrid deep-learning models are implemented:

* **HybridResNetLSTM**: Combines a ResNet-50 image encoder with an LSTM-based time-series branch.
* **HybridResNetTempCNN**: Combines a ResNet-50 image encoder with a 1D temporal CNN branch.

## Table of Contents

* [Project Structure](#project-structure)
* [Data](#data)
* [Installation](#installation)
* [Usage](#usage)
* [Models](#models)
* [Training](#training)
* [Evaluation](#evaluation)
* [Contributing](#contributing)
* [License](#license)

## Project Structure

```
├── nasa_data/
│   ├── fs_sentinel_cyprus_2022_buffer5_interp.csv  # Sentinel-2 time-series
│   ├── meta/
│   │   ├── cyprus_2022_meta.csv                    # Field metadata with crop labels
│   │   ├── cyprus_2022_meta.shp                    # Shapefile for field geometries
│   │   └── cyprus_2022_street_level_meta.csv       # Metadata for street-level images
│   └── street_level/                               # Directory of Mapillary street images
├── CS_Project_V1
├── CS_V2
├── CS_DataHandling
├── Data_Description
├── Data_structure.md
├── requirements.txt        # Python dependencies
└── README.md               # Project overview (this file)
```

## Data

The data pipeline uses:

1. **Sentinel-2 optical & Sentinel-1 SAR time-series**

   * Preprocessed to 5-day temporal resolution, with spectral bands and vegetation indices.
2. **Mapillary street-level images**

   * Ground-level imagery to add spatial context.
3. **Metadata**

   * Field identifiers, crop codes/names, area, pixel counts.

Place all data files under the `data/` directory as shown above.

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/theBlackfish01/CropStats
   cd crop-classification-dl
   ```

2. Create a Python environment (recommended with conda):

   ```bash
   conda create -n crop-classify python=3.11
   conda activate crop-classify
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

## Usage

### Quick Shape Check

In the Python REPL or notebook, run:

```python
from src.data_loader import get_loaders_fast
train_loader, val_loader = get_loaders_fast(
    meta_csv   ='data/meta/cyprus_2022_meta.csv',
    sentinel_csv='data/fs_sentinel_cyprus_2022_buffer5_interp.csv',
    image_dir  ='data/street_level/',
    code_to_idx=code_to_idx_map,
    num_classes=num_classes,
    batch_size =32,
    num_workers=0
)
batch = next(iter(train_loader))
_, T_steps, C_feats = batch['series'].shape
print(f"Time steps: {T_steps}, Bands: {C_feats}")
```

### Training

```bash
python src/train.py \
  --meta_csv data/meta/cyprus_2022_meta.csv \
  --sentinel_csv data/fs_sentinel_cyprus_2022_buffer5_interp.csv \
  --image_dir data/street_level/ \
  --batch_size 32 \
  --epochs 30 \
  --model lstm  # or "tempcnn"
```

Model checkpoints (`.pth`) will be saved in the project root.

## Models

* **HybridResNetLSTM**: Uses a pre-trained ResNet-50 (images) + bidirectional LSTM (time-series).
* **HybridResNetTempCNN**: Uses a pre-trained ResNet-50 (images) + 1D temporal CNN.

Both share a fusion MLP head and support freezing the CNN backbone.

## Evaluation

Validation accuracy and loss are printed each epoch, with early stopping based on validation loss. Adjust hyperparameters in `src/train.py` as needed.

## Contributing

Contributions are welcome! Please open an issue or pull request for bug fixes, improvements, or new features.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
