---
layout: post
title: How I got GitHub Actions working with Miniconda
category:
  - Blog
tags:
  - conda
  - github
  - ci
  - testing
---

I'm working on a Python package right now that I want to make sure is properly tested.
Previously I had been using [Travis](https://www.travis-ci.com/) for this but their new pricing structure made me look for alternatives.
In [EMPress](https://github.com/biocore/empress) we recently switched to GitHub actions as it is free for public repos.
However, when I looked at how it worked I was fairly confused at first.

One of the lead developers of EMPress was kind enough to walk me through EMPress's `main.yml` file but it still took me a bit to figure out the rest.
I've written a small walkthrough of how my own `main.yml` file works in the hopes that it will help someone else (or me in the future).

The GitHub link to the file can be found [here](https://github.com/gibsramen/BIRDMAn/blob/main/.github/workflows/main.yml).

```yaml
name: BIRDMAn CI
```

This is pretty self-explanatory - at the top level I have the name of the workflow as it appears on the repository page.

```yaml
on:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main
```

This block of code specifies when I want this workflow to be triggered.
For this project I want each PR and push to `main` to run the workflow.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

The project I'm working is designed for Linux/macOS so I'm going to focus on those testing environments.
The `build` name can be anything - it's just the name of the collection of instructions.

Here's where things start to get a little hairier.

```yaml
    steps:
      - uses: actions/checkout@v2
        with:
          persist-credentials: false
          fetch-depth: 0
```

The first step I list uses one of GitHub's officially supported actions: [`checkout`](https://github.com/actions/checkout)
This step checks out the project repository (here: gibsramen/BIRDMAn) so I can access the code.
Why this isn't defaulted I'm not really sure - I guess since GH actions are extensible there are times when it wouldn't make sense to do this first.

The `persist-credentials` option doesn't actually matter here (I think) - I just copied it from EMPress.
The `fetch-depth` option specifies to get all the history.

```yaml
      - uses: conda-incubator/setup-miniconda@v2
        with:
          activate-environment: birdman
          python-version: 3.7
```

This block is where things started getting *really* confusing.
I wanted to create a new conda environment to hold the dependencies so I use this action to specify the Python version.
In the future I'd like to test things out with future versions but for the moment that's not super important.

```yaml
      - name: Test conda installation
        shell: bash -l {0}
        run: conda info
```

One of the things that confused me most when writing this file was how to get the environment to persist.
In EMPress we use `conda run` to specify that commands should be run with a given environment but that seems a bit clunky to me.
I *think* the `shell: bash -l {0}` instruction allows me to use the conda environment we just created.
The login shell I'm pretty sure allows me to make use of the environment variables and stuff for using conda.
I use this command throughout the rest of the file but there's likely a way to use it only once and have it persist.
Here, I just test that the conda installation works - if it does the build will show `birdman` as the current environment.

```yaml
      - name: Install PyStan
        shell: bash -l {0}
        run: pip install pystan==3.0.0b7

      - name: Install conda packages
        shell: bash -l {0}
        run: |
          conda install -c conda-forge dask biom-format patsy pytest xarray scikit-bio
```

This is the block that installs the required packages for running BIRDMAn.
I first install a specific version of PyStan through `pip` and then the rest through conda.
The version of PyStan I use is still in beta so it's not yet on conda.

```yaml
      - name: Install BIRDMAn
        shell: bash -l {0}
        run: pip install -e .
```

This is just installing the BIRDMAn package locally so that I can test it.
For this reason I used `checkout` above to get the actual code.

```yaml
      - name: Run tests
        shell: bash -l {0}
        run: pytest --disable-pytest-warnings
```

Finally, I run the unit tests I wrote to make sure things work.
If all of the above tasks complete successfully, our workflow passes and we are good to go.

In the future I'll probably add `flake8` testing or other such stylistic checks.
I'll also probably have to install `node.js` if I end up making a visualization for model diagnostics.

That's it for now! Hope this helps you use GitHub actions with conda (even if the person reading this is me in several months).
