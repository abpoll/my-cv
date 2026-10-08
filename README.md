# Academic CV for Adam Pollack

This repository contains the source data, templates, and scripts used to generate my academic CV.

Most sections of the CV are stored in `.yml` files in `data/`. The publications and conference presentations are maintained in `.bib` files in a separate bibliography repository, which is included here as a Git submodule.

This repository separates **content** from **formatting**:

* `data/*.yml` and the `.bib` files in `bibliography/` contain the CV data.
* `_config.yml` contains personal details and lists which data files make up each section.
* `templates/cv.latextemplate` defines the LaTeX structure and formatting.
* `filters.py` contains custom processing and sorting logic.
* `generate_cv.py` processes the data and template and generates the final `.tex` file in `docs/`.

This makes larger changes to the CV substantially easier because the same template and processing logic can be applied consistently to all of the underlying data.

## Files in `docs/`

| File | How it is made |
|---|---|
| `Pollack-CV.tex` / `.pdf` | **Generated** by `generate_cv.py`. Do not edit the `.tex` by hand: changes should be made in the YAML/BibTeX data, the template, or the Python code, and the document regenerated. |
| `Pollack-CV-2page.tex` / `.pdf` | **Hand-edited** two-page version (written for a NOAA CIROH proposal). It copies the preamble, header, and first sections of the generated file, then has hand-written, trimmed sections. It is *not* produced by `generate_cv.py`, so regenerating the full CV does not touch it. If the underlying data changes (new publication, new grant, a corrected typo), update it by hand too. |

## Setup (one time)

### 1. Clone with the bibliography submodule

`bibliography/` is a Git submodule (`abpoll/bibliography`). It is empty after a plain `git clone`, and the CV will not compile without it:

```bash
git clone git@github.com:abpoll/my-cv.git
cd my-cv
git submodule update --init
```

Git will show the submodule as being on a **detached HEAD**. This is normal: the parent repo pins `bibliography/` to one specific commit. Do not `git pull` inside it. To move to a newer version of the bibliography:

```bash
git submodule update --remote   # then commit the changed pointer in this repo
```

If you ever need to *edit* the `.bib` files, run `git checkout main` inside `bibliography/` first, commit and push there, and then commit the updated pointer here.

### 2. Python environment (pixi)

The Python environment (just `jinja2` and `pyyaml`) is managed with [pixi](https://pixi.sh). Install pixi, then from the repo root:

```bash
pixi install
```

`pixi.toml` defines the dependencies and `pixi.lock` pins the exact versions. Do not edit `pixi.lock` by hand. Both files are committed.

### 3. LaTeX

The CV is compiled with **XeLaTeX** (it uses locally stored fonts from `fonts/`) and **Biber** (for the bibliography), driven by `latexmk`.

* Install [MacTeX](https://tug.org/mactex/) (it includes `xelatex`, `latexmk`, and `biber`), then restart your terminal.
* Check that these print versions: `xelatex --version`, `biber --version`, `latexmk -v`.
* In VS Code, install the **LaTeX Workshop** extension (`James-Yu.latex-workshop`).

The repo includes `.vscode/settings.json`, which defines the `latexmk (xelatex)` build recipe. Automatic builds are disabled, so saving a `.tex` file does not trigger a compilation.

## Typical workflow

```text
Edit data (data/*.yml, bibliography/*.bib) or the template
   ↓
pixi run build-cv          # runs generate_cv.py
   ↓
docs/Pollack-CV.tex
   ↓
LaTeX Workshop (build: Cmd+Option+B) / latexmk -xelatex
   ↓
docs/Pollack-CV.pdf
```

1. Run `pixi run build-cv` to regenerate `docs/Pollack-CV.tex`.
2. Open the `.tex` file (in `docs/`) in VS Code and build with the LaTeX Workshop command **Build LaTeX project** (Cmd+Option+B). View the result with **View LaTeX project** (Cmd+Option+V).

Always build from a `.tex` file inside `docs/`. The fonts and bibliography are referenced as `../fonts/` and `../bibliography/`.

To build the same way from a terminal (for example, `docs/Pollack-CV-2page.tex`):

```bash
cd docs
latexmk -xelatex -interaction=nonstopmode Pollack-CV-2page.tex
```

## Notes

* There is no automated (GitHub Actions) build at the moment. The workflow inherited from the original template pointed at files that do not exist in this repo and was removed. The PDF is built locally and committed.
* Known issue in the bibliography repo (not fixed here): the Gourevitch REEP entry has placeholder `pages = {000--000}` (and volume), so it prints as "0 0, pp. 000-000".

## Acknowledgements
Thanks to Vivek Srikrishnan and James Doss-Gollin for doing all the hard work
