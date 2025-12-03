Create an environment file from an existing conda env

From ~/scripts/conda-export/src run:

```
python -m ecoport -f environment.yaml
```


```
git clone git@github.com:LMLhub/example-repo.git
```

Create new conda environment from scratch
```
mamba create -n env-github-pages -c conda-forge python=3.14
```
or use an existing environment file:
```
 mamba env create --file environment.yaml
```

Activate new environment:
```
mamba activate env-example-repo
```

Create src/ folder for code and docs/ folder for documentation and notes.

```
mkdir -p docs/docs docs/notes
```

Add some starter files:

```docs/index.md``` (top-level landing page)

```
# Example repo

Welcome to the documentation site.

- See [Documentation](docs/index.md) for details about the code.
- See [Notes](notes/index.md) for conceptual background.
```

```docs/docs/index.md``` (code documentation landing page)

```
# Documentation

This section explains how the code works and how to use it.

## Example

Some inline math: $E = mc^2$.

A display equation:

$$
\nabla^2 \psi = \frac{1}{c^2}\frac{\partial^2 \psi}{\partial t^2}
$$

```

```docs/notes/index.md``` (notes landing page)

```
# Notes

This section contains conceptual and theoretical background.

For example:

$$
\int_{0}^{\infty} e^{-x^2} \, dx = \frac{\sqrt{\pi}}{2}
$$
```

Create ```mkdocs.yml``` in the repo root folder. 
This configures the docs and specifies the layout of the static site:

```
site_name: Example repo 

site_url: https://<user>.github.io/<repo>/

theme:
  name: material

docs_dir: docs

nav:
  - Home: index.md
  - Documentation:
      - Overview: docs/index.md
  - Notes:
      - Overview: notes/index.md

markdown_extensions:
  - admonition
  - toc:
      permalink: true
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.superfences
  - pymdownx.highlight

extra_javascript:
  - javascripts/mathjax.js
  - https://unpkg.com/mathjax@3/es5/tex-mml-chtml.js
```

Check that mkdocs runs locally on ```http://127.0.0.1:8000/```:

```
mkdocs serve
```
(or just ```mkdocs build``` to check that the build works)

Add ```requirements.txt``` to repo root folder:

```
mkdocs
mkdocs-material
pymdown-extensions
```

Add a GitHub Actions workflow at ```.github/workflows/docs.yml```:

```
        uses: actions/deploy-pages@v4
~
~
name: Build and Deploy MkDocs

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Build site
        run: |
          mkdocs build --site-dir ./_site

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./_site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Ensure GitHub Pages has been enabled at https://github.com/LMLhub/example-repo/settings/pages
Under "Build and Deployment" set the source to "GitHub actions".
Docs static site should now build every time a push is made to [main]

