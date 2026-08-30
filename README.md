# Academic CV for Adam Pollack

This repository contains the source data, templates, and scripts used to generate my academic CV.

Most sections of the CV are stored in `.yaml` files. The publications and conference presentations are maintained in `.bib` files in a separate bibliography repository, which is included here as a Git submodule.

This repository separates **content** from **formatting**:

* `.yaml` and `.bib` files contain the CV data.
* `templates/cv.latextemplate` defines the LaTeX structure and formatting.
* `filters.py` contains custom processing and sorting logic.
* `generate_cv.py` processes the data and template and generates the final `.tex` file in `docs/`.

This makes larger changes to the CV substantially easier because the same template and processing logic can be applied consistently to all of the underlying data.

## Python environment

The repository uses a small Python environment to process the CV data and generate the LaTeX source.

The environment is defined in `environment.yml`. The main script is:

```bash
python generate_cv.py
```

This reads the configuration and section data, processes the information outside of the LaTeX template, applies the custom filters, and generates:

```text
docs/Pollack-CV.tex
```

The generated `.tex` file should not be edited directly. Changes should instead be made in the relevant YAML/BibTeX data, template, or Python processing code, and the document should then be regenerated.

## Compiling the CV

The generated LaTeX document is compiled locally in **Visual Studio Code** using the **LaTeX Workshop** extension.

The CV uses XeLaTeX, including because the document uses locally stored fonts.

My LaTeX Workshop configuration is:

```json
{
  "terminal.integrated.commandsToSkipShell": [
    "language-julia.interrupt"
  ],
  "julia.symbolCacheDownload": true,
  "latex-workshop.latex.autoBuild.run": "never",
  "latex-workshop.latex.recipe.default": "latexmk (xelatex)",
  "latex-workshop.latex.tools": [
    {
      "name": "latexmk-xe",
      "command": "latexmk",
      "args": [
        "-xelatex",
        "-file-line-error",
        "-interaction=nonstopmode",
        "-synctex=1",
        "%DOC%"
      ]
    }
  ],
  "latex-workshop.latex.recipes": [
    {
      "name": "latexmk (xelatex)",
      "tools": ["latexmk-xe"]
    }
  ]
}
```

With `docs/Pollack-CV.tex` open in VS Code, the LaTeX Workshop build command runs the `latexmk (xelatex)` recipe. `latexmk` handles the compilation steps required by the document, including updating the bibliography with Biber when necessary.

Automatic builds are disabled, so saving the `.tex` file does not trigger a compilation.

There may be additional operating-system-specific configuration required to get the LaTeX toolchain working locally, particularly for MacTeX, Biber, fonts, and related system tools.

## Typical workflow

For a typical CV update:

```text
Edit data
   ↓
generate_cv.py
   ↓
docs/Pollack-CV.tex
   ↓
LaTeX Workshop / latexmk / XeLaTeX
   ↓
CV PDF
```

## Acknowledgements
Thanks to Vivek Srikrishnan and James Doss-Gollin for doing all the hard work
