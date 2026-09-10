# Early Warning of Meteorological Drought in Morocco's Atlas Mountains

Reproducibility materials for the manuscript:

> Aderdour N, Rhinane H, Rueff H, Maanan M. Early Warning of Meteorological Drought in
> Morocco's Atlas Mountains: A Multi-Paradigm Benchmark of Satellite-Augmented Deep
> Learning and Tree-Based Models. Submitted to *Theoretical and Applied Climatology*.

This repository reproduces the figures and tables reported in the paper by loading the
pre-trained ConvGRU (Model B) weights and running inference on the held-out test set. No
model training is performed; all outputs are deterministic (fixed random seeds).

## Repository structure

```
.
├── notebooks/
│   ├── 02_reproduce_figures.ipynb     # figures, ConvGRU metrics, DM tests, permutation
│   └── 03_comparison_models.ipynb     # 5-model comparison (XGBoost, RF, ConvLSTM, Pixel-LSTM)
├── data/
│   ├── monthly_1981.tif ... monthly_2026.tif   # yearly input composites (9 variables/month)
│   └── spi6_cache.npz                 # optional: cached SPI-6 (speeds up the run)
├── weights/
│   └── convgru_h{1,2,3}_s{42,43,44}.weights.h5  # 9 trained ConvGRU weight files
├── configs/
│   ├── outputs_final_config.json      # locked model/training/split parameters
│   ├── seed_list.json
│   └── best_seed_map.json
├── logs/
│   └── training_log_h{1,2,3}_seed{42,43,44}.csv  # per-run training logs (learning curves)
├── requirements.txt
├── LICENSE
└── README.md
```

## Requirements

Python 3.10+ and the packages in `requirements.txt` (NumPy, Pandas, SciPy, scikit-learn,
Matplotlib, rasterio, tqdm, TensorFlow). A GPU is optional; the notebook runs on CPU for
inference (slower). Install with:

```
pip install -r requirements.txt
```

## How to run

- **Locally:** open `notebooks/02_reproduce_figures.ipynb` in Jupyter from the repository
  root and run all cells in order.
- **Binder:** launch the repository on https://mybinder.org to run it in a clean cloud
  environment without any local setup.
- **Google Colab:** open the notebook from this repository; place the `data/`, `weights/`,
  `configs/`, and `logs/` folders where the paths at the top of the configuration cell point.

Outputs (tables, figures, GeoTIFFs) are written to an `outputs/` folder.

## Notes on the reported numbers

ConvGRU RMSE is reported in the paper as the **ensemble-mean prediction** RMSE (0.696 at
H = 1), computed by averaging the three-seed predictions before scoring. The per-seed mean
RMSE (0.714) is a different aggregation and appears in `seed_summary.csv`; it is not the
value reported in the tables.

## Data availability

The yearly input composites were assembled in Google Earth Engine from CHIRPS
(precipitation), ERA5-Land (soil moisture, evaporation, skin temperature), AVHRR and MODIS
(NDVI), and SRTM (terrain), over 1981-2026 at approximately 5.5 km resolution.

## Citation

If you use these materials, please cite the paper (DOI to be added upon acceptance).

## License

See `LICENSE`. Code is released under the MIT License; data and figures under CC BY 4.0.
