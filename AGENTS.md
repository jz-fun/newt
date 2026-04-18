# Repository Guidelines

## Project Structure & Module Organization
`tdmpc2/` contains the Python source for training, evaluation, buffers, world-model code, and environment adapters. Key entrypoints are `tdmpc2/train.py`, `tdmpc2/generate_demos.py`, and the shared config in `tdmpc2/config.py`. Environment-specific code lives under `tdmpc2/envs/`; reusable components live under `tdmpc2/common/`.

Top-level supporting data is kept outside the package: `assets/` stores figures and media used in documentation, `csv/` contains published benchmark results, `docker/` contains the Dockerfile and Conda environment, and `tasks.json` defines task metadata and embeddings. There is currently no dedicated `tests/` directory.

## Build, Test, and Development Commands
Create the local environment with:

```bash
conda env create -f docker/environment.yaml
conda activate newt
pip install --no-cache-dir 'ale_py==0.10'
```

Build the container with `cd docker && docker build . -t <user>/newt:1.0.0`.

Run training from the package directory so local imports resolve:

```bash
cd tdmpc2
python train.py task=walker-walk model_size=B enable_wandb=False
python generate_demos.py task=walker-walk +num_demos=10 data_dir=<path>
```

For a quick smoke test, prefer a short run such as `python train.py task=walker-walk model_size=B steps=1000 eval_episodes=1 enable_wandb=False compile=False`.

## Coding Style & Naming Conventions
Follow the existing Python style in the touched file. This codebase primarily uses tabs for indentation, `snake_case` for functions and variables, `PascalCase` for classes, and hyphenated task identifiers such as `walker-walk` or `mw-door-open`. Keep modules focused and colocate new environment logic under the matching `tdmpc2/envs/` area.

No formatter or linter configuration is committed. Keep imports tidy, avoid broad refactors in unrelated files, and preserve existing formatting conventions when editing.

## Testing Guidelines
There is no formal automated test suite yet. Before opening a PR, run the smallest affected workflow you can: a short Hydra training run, demo generation for the touched task, or a targeted import/sanity check. Document what you ran and any hardware assumptions, especially CUDA or simulator dependencies.

## Commit & Pull Request Guidelines
Recent commits use short, imperative subjects in lowercase, for example `add results as csv` and `update readme`. Follow that pattern and keep each commit scoped to one logical change.

PRs should include a clear summary, reproduction or validation steps, linked issues when applicable, and notes about required assets, checkpoints, or environment variables such as `MANISKILL_ASSET_DIR` and WandB settings.
