# devcontainer-templates

Opinionated Dev Container templates for common workloads. These templates are meant to be copied or referenced in your own repositories and work with VS Code Dev Containers and the devcontainer CLI.

## Templates

| Directory | Use case | Base image | Highlights / notes |
| --- | --- | --- | --- |
| `latex` | Japanese LaTeX authoring | `paperist/texlive-ja:latest` | LaTeX Workshop setup, `algorithmicx`/`algpseudocodex`, `plistings.sty`, `.latexmkrc`, common CLI utilities |
| `python-data-science` | Python data analysis and notebooks | `mcr.microsoft.com/devcontainers/python:3.12-bookworm` | `uv` workflow, `ruff`/`pytest`/`ipykernel`, cached dependency installs, VS Code Python/Jupyter settings |
| `python-opencv` | Python + OpenCV | `mcr.microsoft.com/devcontainers/python:3.12-bookworm` | OpenCV OS deps (`libgl1`, `libglib2.0-0`, audio libs), `uv` workflow, `ruff`/`pytest`/`ipykernel`, cached installs |

## Repository layout

Each template is self-contained and typically includes:

- `Dockerfile` for the image build
- `.devcontainer/devcontainer.json` for VS Code and devcontainer CLI
- Optional project scaffolding such as `pyproject.toml` and `uv.lock`

## How to use

Pick the option that fits your workflow. All options assume you already know how to open a Dev Container.

### Option A: Copy a template into your repo

This is the simplest approach and works well for one-off projects.

```bash
# Example: python-data-science
git clone https://github.com/Nanase-tohoku-univ/devcontainer-templates
cp -R devcontainer-templates/python-data-science <your-project>
```

Then open your repo in a Dev Container.

### Option B: Add as a Git submodule (recommended for long-lived repos)

Keeps templates up to date while remaining pinned to a revision you control.

```bash
git submodule add https://github.com/Nanase-tohoku-univ/devcontainer-templates .devcontainer/templates
```

Note: Python templates expect `pyproject.toml` and `uv.lock` at the build context root. Copy or generate them in your repo as needed.

## Template notes

- Python templates use `uv` for dependency management. Update `pyproject.toml`, then run `uv lock` to refresh `uv.lock`.
- The OpenCV template installs OS libraries only. Add `opencv-python` or `opencv-contrib-python` to your dependencies if needed.
- The LaTeX template includes a sample `main.tex` and an `out/` directory with build artifacts. You can delete `out/` if you do not need it.

## Compatibility

- VS Code + Dev Containers extension
- devcontainer CLI

## Contributing

Issues and pull requests are welcome. Please include the template name and expected behavior when reporting bugs.
