# Hajj Fund Anomaly Detection

Static, client-side demo. No server or backend required — the trained
Isolation Forest model (scikit-learn) runs directly in the browser via
ONNX Runtime Web.

## How to use

1. Open `index.html` (via GitHub Pages, or any static file host).
2. Click "Download sample_CSV" to get an example file, or use your own
   CSV with the required columns (see below).
3. Drop the CSV into the upload area and click "Run detection".
4. Flagged transactions appear in a table, ranked by anomaly score.

## Deploying to GitHub Pages

1. Push these 3 files (`index.html`, `model.onnx`, `sample_data.csv`) to
   a GitHub repository.
2. Go to Settings → Pages → set Source to your main branch, root folder.
3. Wait a minute, then visit the URL GitHub gives you
   (usually `https://<username>.github.io/<repo-name>/`).

No build step, no server, no environment variables needed.

## Required CSV columns

| Column | Description |
|---|---|
| `transaction_id` | Unique transaction identifier |
| `timestamp` | e.g. `2026-04-07 14:56:53` |
| `transaction_type` | `deposit`, `investment`, or `disbursement` |
| `sub_category` | e.g. `setoran_awal`, `sukuk_sbsn`, `living_cost` |
| `amount_idr` | Transaction amount in IDR |
| `account_origin` | Origin account identifier |
| `account_destination` | Destination account identifier |
| `channel` | e.g. `bank_transfer`, `teller`, `RTGS`, `virtual_account` |

## How it works

- `model.onnx` is the exact Isolation Forest model reported in the
  accompanying paper (scikit-learn, converted to ONNX via `skl2onnx`),
  verified to produce identical predictions to the original Python model.
- Feature engineering (amount z-score, time-since-last-transaction,
  transaction velocity, etc.) is re-implemented in JavaScript inside
  `index.html`, mirroring the Python `engineer_features()` logic exactly.
- Everything runs locally in the browser. No data is uploaded anywhere.

## Limitations

This is a research prototype trained on synthetic data, not a
production fraud detection system. See the accompanying paper's
Discussion and Limitations sections for details on detection
performance and known weaknesses (e.g. lower recall for purely
time-based anomalies).
