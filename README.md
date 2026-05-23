# Maritime CPB Scout — Streamlit Prototype

A scouting interface for Colorado potato beetle detection. Pairs with the
baseline YOLO model trained in `cpb_baseline.ipynb`.

## Quick start

```bash
# From a fresh terminal in this folder
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Place your trained weights in ./weights/best.pt
mkdir -p weights
# (copy best.pt from your Colab run / Google Drive into ./weights/)

streamlit run app.py
```

A browser tab opens at `http://localhost:8501`. If you don't have weights
yet, the app loads in **demo mode** — UI works, no detections.

## What's in here

- `app.py` — the Streamlit app itself
- `requirements.txt` — Python dependencies
- `weights/` — drop your trained `best.pt` here (gitignored)
- `feedback/corrections.csv` — auto-created the first time a correction is saved

## Three tabs

1. **Upload** — drop in 20–50 plant photos, get per-image counts and a
   block-level spray recommendation.
2. **Live capture** — single-shot webcam mode for in-field or in-greenhouse
   spot-checks.
3. **About** — what the tool does, what it does not, and where the
   thresholds came from.

## The recommendation logic

The block-level recommendation fires if **any** of the per-plant averages
exceeds its threshold:

| Class | Default threshold |
|---|---|
| Adults / plant | 1.5 |
| Larvae / plant | 4.0 |
| Egg masses / plant | 1.0 |

All three are editable in the sidebar. **The defaults are illustrative.**
Real action thresholds vary by region, crop stage, and which extension
service you follow — verify with AAFC, Perennia, or the PEI Department of
Agriculture before relying on them for real spray decisions.

## Feedback collection

The "📝 Correct counts" widget under each image lets the user enter true
counts when the model is wrong. Corrections append to
`feedback/corrections.csv` with timestamp, predicted counts, true counts,
and an optional note. This is your future training data — back it up.

## Demoing without a model

The app loads fine without weights present. Useful for showing the UI
to growers or entomologists before the model is fully trained.

## Deploying

For a real grower demo, two reasonable options:

- **Streamlit Community Cloud** (free) — push the repo to GitHub, connect
  via share.streamlit.io. Caveat: the free tier has limited CPU and no
  GPU, so inference will be slow on large batches.
- **Local on your laptop** — what you'll use for in-person demos. No
  network dependency. Faster than the cloud version.

A Dockerfile and a phone-friendly deployment guide are v0.2 work, after
you have at least one grower conversation telling you which they need.
