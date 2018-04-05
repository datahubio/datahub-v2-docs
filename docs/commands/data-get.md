---
title: Get Command
date: 2018-29-03
---

[[toc]]

This guide covers complete usage of `data get` command.

Originally `data get` was designed to easily get datasets from DataHub on your machine, but you can download pretty much everything.

## Usage

```
data get <URL>
```

URL can be one of:

* dataset in DataHub
* dataset in GitHub
* specific version of dataset in DataHub
* direct URL to a file


*Datasets* will be placed inside `<publisher>/<dataset>` directory (relative to your current working directory).

*Files* will be placed in you current working directory.

## Options:

```
-h, --help               Outputs usage information
```

## Example:

```
# Get dataset from DataHub
# Following dataset will be saved in core/co2-ppm
■ data get https://datahub.io/core/co2-ppm

# Get dataset from DataHub
# Following dataset will be saved in core/co2-ppm
■ data get https://datahub.io/core/co2-ppm/v/5

# Get dataset From GitHub
# Following dataset will be saved in datasets/co2-ppm
■ data get https://github.com/datasets/co2-ppm

# Get file from DataHub with resource name or index
■ data get https://datahub.io/core/co2-ppm/r/co2-mm-mlo.csv
■ data get https://datahub.io/core/co2-ppm/r/0.csv

# Get file from GitHub
■ data get https://github.com/datasets/co2-ppm/blob/master/data/co2-mm-mlo.csv
```
