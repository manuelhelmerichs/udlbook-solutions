# udl-solutions

This repository provides a ready-to-use Python environment (managed by `uv`) for working through the *Understanding Deep Learning* book in Jupyter notebooks.

## Features

- Python 3.11+ compatible (pinned range in `pyproject.toml`)
- Core scientific stack: NumPy, SciPy, pandas, scikit-learn
- PyTorch + torchvision + torchaudio (CPU build by default)
- Visualization: matplotlib, seaborn, (optional) Plotly
- Productivity: tqdm, rich, einops
- Notebook tooling: JupyterLab, ipykernel
- Optional extras for NLP (Transformers, Datasets) and visualization
- Dev tooling (optional): pytest, ruff, black, mypy, pre-commit

## Prerequisites

Install `uv` (fast Python package and environment manager):

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
# Make sure ~/.local/bin (or the installation target) is on your PATH
```

Verify:

```bash
uv --version
```

## Create & Sync the Environment

Inside this project directory:

```bash
uv sync
```

This creates a local `.venv` (because `[tool.uv] package = true`) and installs dependencies.

To include optional groups:

```bash
uv sync --extra viz --extra nlp --extra dev
```

## Activating the Environment

You can run commands with `uv run` without manual activation:

```bash
uv run python -c "import torch, numpy; print(torch.__version__)"
```

Or activate the virtual environment manually (optional):

```bash
source .venv/bin/activate
```

## Adding the Kernel to Jupyter

`uv sync` plus `ipykernel` should auto-enable using the interpreter. To explicitly register:

```bash
uv run python -m ipykernel install --user --name udl --display-name "Python (UDL)"
```

Then select the kernel in JupyterLab / VS Code.

## CUDA / GPU Support

By default PyTorch CPU wheels are installed. For CUDA (example: CUDA 12.1):

```bash
uv pip install --force-reinstall torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

Adjust `cu121` to match your driver/toolkit. See https://pytorch.org/ for options.

## Running a Notebook

```bash
uv run jupyter lab
```

Or open VS Code and select the `Python (UDL)` kernel.

## Development Tooling (Optional)

Install with extras:

```bash
uv sync --extra dev
```

Then you can run:

```bash
uv run ruff check .
uv run black .
uv run pytest
```

## Pre-commit Hooks (Optional)

```bash
uv run pre-commit install
```

## Updating Dependencies

```bash
uv lock --upgrade
uv sync
```

## Minimal Test

```bash
uv run python - <<'PY'
import torch, numpy as np
print('Torch:', torch.__version__)
print('PyTorch CUDA available?', torch.cuda.is_available())
print('NumPy:', np.__version__)
PY
```

## Troubleshooting

- If the kernel doesn't appear, re-run the `ipykernel install` command.
- For SSL or network errors, try `uv sync --no-build-isolation`.
- If mixing system CUDA + PyTorch wheels causes issues, prefer the official index URL reinstall.
