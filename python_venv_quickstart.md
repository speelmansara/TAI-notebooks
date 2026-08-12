# Setting Up a Virtual Environment from `requirements.txt`

A quick reference for creating an isolated Python environment for a repo and using it inside Jupyter. This file is largely based on the [Python Packaging User Guide page on installing packages in a virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/index.html).

**Prerequisites:** Python 3.8+ installed and available on your PATH. Check with:
```
python --version      # or python3 --version
```

---

## 1. Navigate to the repo

```bash
cd path/to/the/repo
```
Confirm `requirements.txt` is there:
```bash
ls        # Linux/macOS
dir       # Windows
```

---

## 2. Check the Python version the repo needs

`venv` doesn't install Python versions — it just uses whichever interpreter you call it with. Check the repo's README or `requirements.txt`/`pyproject.toml` for a required version, then confirm you have it:
```bash
python3.11 --version   # example: check if 3.11 is installed
```
If it's missing, install it via [python.org](https://python.org), your OS package manager, or [`pyenv`](https://github.com/pyenv/pyenv) (`pyenv-win` on Windows) if you need to juggle multiple versions across projects. Then point `venv` at that specific interpreter (e.g. `python3.11 -m venv .venv` instead of plain `python -m venv .venv`).

## 3. Create the virtual environment

Create it **inside the repo folder** as `.venv` (convention, and usually already in `.gitignore`).

### Windows (PowerShell or CMD)
```powershell
python -m venv .venv
# or, to pin a specific version via the py launcher:
py -3.11 -m venv .venv
```

### Linux
```bash
python3 -m venv .venv
# or a specific version:
python3.11 -m venv .venv
```

### macOS
```bash
python3 -m venv .venv
# or a specific version:
python3.11 -m venv .venv
```

---

## 4. Activate the virtual environment

You must activate it *every time* you open a new terminal session to work on this repo.

| OS | Shell | Command |
|---|---|---|
| Windows | PowerShell | `.venv\Scripts\Activate.ps1` |
| Windows | CMD | `.venv\Scripts\activate.bat` |
| Linux | bash/zsh | `source .venv/bin/activate` |
| macOS | bash/zsh | `source .venv/bin/activate` |

Your prompt should now show `(.venv)` at the start.

> **Windows PowerShell note:** if you get an error about "running scripts is disabled," run this once (as your user, not admin, unless required):
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
> ```
To check the location of your interpreter to verify that the virtual environment is activated:

Windows:
```powershell
where python
```
Linux and MacOS:
```bash
which python
```
To deactivate at any time (any OS):
```bash
deactivate
```

---

## 5. Install the dependencies

With the venv **activated**:
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

---

## 6. Register the venv as a Jupyter kernel

Still with the venv activated, install `ipykernel` and register the environment:

```bash
pip install ipykernel
python -m ipykernel install --user --name=repo-venv --display-name "Python (repo-venv)"
```

- `--name` is an internal identifier (keep it unique per project).
- `--display-name` is what you'll see in Jupyter's kernel list — make it recognizable.

---

## 7. Select the kernel in Jupyter

1. Launch Jupyter (Notebook or Lab) — can be from inside or outside the venv, it doesn't matter now since the kernel is globally registered.
2. Open your notebook.
3. Go to **Kernel → Change Kernel** (or click the kernel name top-right in Lab).
4. Select **Python (repo-venv)**.

To verify you're in the right environment, run this in a notebook cell:
```python
import sys
print(sys.executable)
```
The path should point inside your repo's `.venv` folder.

---

## Cleanup (optional)

To remove a registered kernel later:
```bash
jupyter kernelspec uninstall repo-venv
```

**Disclaimer**: GenAI was used in creating this guide.