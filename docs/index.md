# Welcome to the DiscoSat Bible
This is the automatic documentation builder for DiscoSAT.
It imports the documentations from all subprojects and provides them in one central place.
This is achieved through the use of [mkdocs](https://www.mkdocs.org) and the [mkdocs-multirepo-plugin](https://github.com/jdoiro3/mkdocs-multirepo-plugin).

## Getting started
??? tip "Usage of virtualenv"

    This guide assumes that the `virtualenv` tool is available on your system.  
    Please refer to your distribution manual or see [docs.python.org](https://docs.python.org/3/library/venv.html)

```shell
# Create a new virtual environment
virtualenv venv && source venv/bin/activate

# Install all the requirements for this project
pip install -r requirements.txt
```

## Build and serve
Building and serving this documentation is done locally by using the `mkdocs` cli.
To serve the documentation using the built-in local development server use `mkdocs serve`

If you want to build a version of the page for remote deployment use `mkdocs build` and transfer the resulting `site` folder to a web-server of your choice.

## Commands

* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout
    mkdocs.yml      # The main configuration file.
    disat_theme/    # Custom theme overrides with extra css and local fonts
    docs/
        index.md    # This page
        ...         # Other markdown pages, images and other files.
    .gitlab-ci.yml  # The gitlab ci runner config for automatic deployment
