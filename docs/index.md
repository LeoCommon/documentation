# Welcome to the DiscoSat Bible
This is the automatic documentation builder for DiscoSAT.
It imports the documentations from all subprojects and provides them in one central place
This is achieved through the use of [mkdocs](https://www.mkdocs.org) and the [mkdocs-multirepo-plugin](https://github.com/jdoiro3/mkdocs-multirepo-plugin).

## Getting started
Install mkdocs and the required dependencies
```
pip install mkdocs mkdocs-material mkdocs-awesome-pages-plugin mkdocs-minify-plugin
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
    mkdocs.yml    # The main configuration file.
    fonts/        # Local copy of all google fonts
    css/          # Home of custom stylesheets
    docs/
        index.md  # This page
        ...       # Other markdown pages, images and other files.
