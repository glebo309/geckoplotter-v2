# GeckoPlotter v2

A Streamlit-based scientific data analysis platform for HPLC/LC-MS chromatogram analysis, peak detection, calibration, and protein structure visualization.

![GeckoPlotter](resources/GeckoPlotter_logo.png)

## Quick Start

```bash
# Clone the repo
git clone https://github.com/glebo309/geckoplotter-v2.git
cd geckoplotter-v2

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run app.py
```

## Project Structure

```
geckoplotter/
├── app.py                    # Main entry point — file routing, tab layout
├── config/
│   └── settings.py           # Session state defaults, peak detection params, color palettes
├── data_readers/             # File format parsers
│   ├── base_reader.py        # Abstract base class
│   ├── chromatogram_reader.py # HPLC .txt files
│   ├── csv_reader.py         # CSV (Time/Value columns)
│   └── spectra_reader.py     # UV-Vis spectra
├── lcms/                     # LC-MS module
│   ├── cdf_reader.py         # NetCDF (.cdf) parser
│   ├── lcms.py               # Data structures
│   ├── lcms_view.py          # TIC + mass spectrum UI
│   ├── lcms_plot.py          # LC-MS plotting
│   └── ms_plotter.py         # Mass spectrum visualization
├── models/                   # Data models
│   ├── chromatogram.py       # Chromatogram data model + sample generators
│   └── calibration.py        # Calibration curve fitting (linear, polynomial, cubic)
├── ui/                       # Streamlit UI components
│   ├── sidebar.py            # Left sidebar — file upload, controls, settings
│   ├── plot.py               # Main Plotly chromatogram chart
│   ├── plot_settings.py      # Plot customization (axes, fonts, colors)
│   ├── plot_interactions.py  # Click-to-pick-peak, zoom, interactive features
│   ├── samples_view.py       # "Sample Results" tab — peak table, area %
│   ├── peaks_view.py         # "Compound Analysis" tab
│   ├── calibration_ui.py     # Calibration workflow UI
│   ├── export_options.py     # CSV export
│   └── custom_uploader.py    # File upload handler
├── utils/                    # Shared utilities
│   ├── peak_detection.py     # Peak detection algorithm (current)
│   ├── data_processing.py    # Smoothing, baseline correction
│   ├── file_upload_handler.py # Upload routing
│   ├── color_utils.py        # Color conversion
│   ├── colourmaps.py         # Color palettes
│   └── toast.py              # Notification system
├── pdb/                      # Protein structure viewer (standalone module)
│   ├── app.py                # PDB viewer entry point
│   ├── parser.py             # PDB file parser
│   ├── viz_3d.py             # 3D Py3Dmol visualization
│   └── ...                   # Sequence, network, AI analysis
└── resources/                # Static assets (logo, user guide)
```

## How the App Works

1. **Upload** — User uploads HPLC (.txt), CSV, or LC-MS (.cdf) files via the sidebar
2. **Parse** — The appropriate reader in `data_readers/` or `lcms/` parses the file
3. **Display** — Chromatograms are plotted with Plotly in `ui/plot.py`
4. **Detect Peaks** — Click on the plot or use auto-detection (`utils/peak_detection.py`)
5. **Analyze** — View peak properties (height, area, width, retention time) in the results tabs
6. **Calibrate** — Build standard curves against known concentrations (`models/calibration.py`)
7. **Export** — Download results as CSV

## Key Areas & What They Do

| Area | Files | Purpose |
|------|-------|---------|
| **Core app flow** | `app.py`, `config/settings.py` | Entry point, session state, tab layout |
| **Data import** | `data_readers/*`, `lcms/cdf_reader.py` | Parsing uploaded files into internal format |
| **Visualization** | `ui/plot.py`, `ui/plot_settings.py` | Plotly chromatogram rendering |
| **Peak analysis** | `utils/peak_detection.py`, `utils/data_processing.py` | Signal processing, peak finding, integration |
| **Calibration** | `models/calibration.py`, `ui/calibration_ui.py` | Standard curves, quantification |
| **LC-MS** | `lcms/*` | TIC display, mass spectrum extraction |
| **PDB viewer** | `pdb/*` | Protein structure analysis (independent module) |

## Contributing Guidelines

### Branching Workflow

- **`main`** — stable, working code. Never push broken code here.
- Create a **feature branch** for your work: `git checkout -b feature/your-feature-name`
- When done, open a **Pull Request** to merge into `main`

### Before You Push

- Make sure `streamlit run app.py` starts without errors
- Test file uploads (try .txt and .csv at minimum)
- Don't break peak detection or calibration — these are core features

### Things to Be Careful With

- **`utils/peak_detection.py`** — Core peak detection algorithm. Changes here affect all analysis.
- **`config/settings.py`** — Session state initialization. Adding/removing keys can break the app.
- **`app.py`** — Main routing logic. Changes ripple everywhere.
- **`models/calibration.py`** — Calibration math. Needs to stay accurate.

### Safe Areas to Work On

- **`pdb/`** — Protein viewer is mostly independent, safe to modify
- **`ui/`** components — UI tweaks are usually low-risk
- **`data_readers/`** — Adding new file format support is safe (just add a new reader)
- **`utils/color_utils.py`, `utils/colourmaps.py`** — Visual stuff, low risk

## Tech Stack

- **Framework**: Streamlit
- **Plotting**: Plotly
- **Data**: NumPy, Pandas, SciPy
- **ML/Stats**: PyMC, scikit-learn
- **Chemistry**: BioPython, FreeSASA (for PDB module)
