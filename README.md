📘 README.md
markdown
Copy
Edit
# 🚦 DCRNN for Traffic Prediction with Integrated Traffic Analysis

This project extends the original [DCRNN_PyTorch](https://github.com/chnsh/DCRNN_PyTorch) repository by integrating **traffic data analysis** alongside training and evaluation. While the original repo focused only on model implementation, traffic data analysis was located in a separate repository. We combined both to create a complete, end-to-end pipeline for forecasting traffic speed using DCRNN.

---

## 📍 Project Highlights

- ✅ Implemented full pipeline: data preprocessing, model training, validation, and CSV-based metric export.
- ✅ Integrated **METR-LA** traffic dataset for training.
- ✅ Added logic for **scaling, truncating, and evaluating** results with reduced compute resources.
- ✅ Exported training logs and metrics (loss values) to CSV and computed `mean ± std` summaries.

---

## ⚠️ Challenges Faced

Training the full model for 100 epochs (as in the original paper) on the full dataset was not feasible due to:

- 💻 Hardware limitations (local machine couldn't support multi-day training).
- ❌ Unexpected interruptions (training jobs terminated mid-run due to resource exhaustion or crashes).

---

## ✅ Solution

To resolve this:

- We **reduced the number of training epochs to 10**, which allowed us to complete training within a reasonable time frame (under 3 hours).
- Used **dataset truncation (10%)** to simulate a realistic training scenario while maintaining representative results.
- Computed **validation loss**, exported scalar logs to `.csv`, and summarized results using `mean ± std`.

---

## 🏁 Results

Here’s an example of validation loss output after 10 epochs:

Val Loss: 2.398 ± 0.215

yaml
Copy
Edit

These results demonstrate that even with fewer epochs, the model maintains reasonable prediction quality, supporting the feasibility of lightweight experimentation.

---

## ▶️ Quick Start (Colab or Local)

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Truncate data for fast training
python truncate_h5.py

# 3. Train with reduced epochs
python dcrnn_train_pytorch.py --config_filename=data/model/dcrnn_la.yaml

# 4. Evaluate
python run_demo_pytorch.py --config_filename=data/model/dcrnn_la.yaml
📤 Exporting Metrics
To convert loss logs into mean ± std:

python
Copy
Edit
import pandas as pd
df = pd.read_csv("Loss_val.csv")
print(f"{df['Value'].mean():.3f} ± {df['Value'].std():.3f}")

References:

Original DCRNN implementation: https://github.com/chnsh/DCRNN_PyTorch

Dataset: METR-LA Traffic Speed from https://github.com/liyaguang/DCRNN

