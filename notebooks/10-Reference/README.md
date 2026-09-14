# 10 — Reference

Notebooks in this folder are not tied to a class day and there is nothing to submit from them. They answer questions that come up all semester, and they are meant to be reopened rather than worked through once.

| Notebook | What it answers |
|---|---|
| `Colab File Persistence and Google Drive — Reference.ipynb` | Where files go on Colab, why they vanish, and how to mount Google Drive so they don't |

---

## The short version of the Colab notebook

A Colab runtime is temporary. Anything you write to `/content` is deleted when the runtime disconnects — closing the tab, going idle, restarting, or hitting the session cap. Your `.ipynb` survives because it lives in Drive; your data files do not.

Three places a file can be, and only two of them last:

| Location | Path | Survives disconnect? |
|---|---|---|
| Runtime disk | `/content/...` | **No** |
| Google Drive | `/content/drive/MyDrive/...` | **Yes** |
| Your laptop | wherever you downloaded it | **Yes** |

So mount Drive, define one folder, and build every path from it:

```python
from pathlib import Path
from google.colab import drive

drive.mount('/content/drive')

DATA_DIR = Path('/content/drive/MyDrive/DS2002/data')
DATA_DIR.mkdir(parents=True, exist_ok=True)

df.to_csv(DATA_DIR / 'output.csv', index=False)
assert (DATA_DIR / 'output.csv').exists()
```

You re-authorize the mount once per session. That is normal and unavoidable.

Two habits worth making automatic: `mkdir(parents=True, exist_ok=True)` before you write, because `open()` will not create missing folders, and an `assert path.exists()` after a save that matters, because "I thought it saved" is not a recoverable position the night before a deadline.
