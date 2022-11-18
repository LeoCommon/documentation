---
tags:
  - SATOS
  - Maintenance
---
# Buildroot Maintenance
Our Buildroot fork comes with certain package tweaks and version bumps which are not available (yet) upstream.

???+ danger "Custom packages"

    No custom packages shall be directly added to the buildroot fork.
    Please manage all of them inside SATOS!

## New LTS Versions
### Minor
This project is based on 2022.02 LTS, check [Buildroot Tags](https://github.com/buildroot/buildroot/tags) to determine if a new LTS update is available.

```bash
# If not done already, add the upstream repository to the buildroot directory
git remote add upstream https://github.com/buildroot/buildroot.git

# Fetch tags and version history from upstream
git fetch upstream --tags

# Rebase to the new version number you looked up
git rebase 2022.02.X

# Create a new feature branch
git checkout -b 2022.02.X-satos

# Push it to our repository
git push --set-upstream origin 2022.02.X-satos
```

??? Warning "Submodule updates"

    Dont forget to adjust the submodule references if working on a clone of the repository.

If done make sure to conduct a **clean** rebuild of the entire buildroot sources to make sure everything re-builds correctly.

### Major
If a new major LTS version is available, the package level adjustments outlined below need to be re-evaluated.  

## Changed upstream packages
### Gnuradio 3.9
Support for **3.9.7.0** was added based off a past pull request to upstream buildroot.

!!! note "Future upstream support"
    Support did not make it into the 2022.02.X LTS we are using for this project at the current time.  
    However some restructuring was done in later upstream releases so breaking changes are expected when switching to a new LTS.

### gr-osmosdr
Version bumped and config modified to support the HACKRF SDR.

### python-numpy
Uses the more recent [https://numpy.org/devdocs/release/1.23.0-notes.html](1.23.0) version to allow for better optimization and interopability with recent > 3.8 GnuRadio versions.

### python-cython
Version bumped to [https://pypi.org/project/Cython/0.29.30/](0.29.30) to allow `python-numpy` to build.

### hackrf
Bumped to [https://github.com/greatscottgadgets/hackrf/releases/tag/v2021.03.1](2021.03.1) contains some bugfixes.

### usbmount && udisks
Legacy tweaks that are scheduled for removal once the situation surrounding the config USB stick is clear.

## Regular maintenance tasks
The following tasks need to be performed manually and are essential to the security of the project.

### Raspberry PIs
The RaspberryPi targets come with custom versions of kernels and firmware that are solely tracked inside the `SATOS` repository.

???+ note
    This should provide an outline on how to maintain discosats buildroot fork specific parts of the repository, if unified with SATOS, the maintenance tasks that need to be performed there should also be added to this document.
