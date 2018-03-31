---
title: "data: Command Line Tool"
---

`data` is the command-line tool to prepare, push and get data. With `data` you will be able to:

* Push data online
* Get data from online sources
* Get information about particular data files and datasets both locally and remotely including those on the DataHub
* Validate your data to ensure its quality

[[toc]]

# Install

See [Install `data`][install].

[install]: /docs/getting-started/installing-data

# Usage

You can see the latest commands and get help by doing:

```bash
data help
```

## `push`

Putting data online is one command: `push`.

```bash
data push [FILE-or-DATA-PACKAGE-PATH]
```

**Note:** *by default, findability flag for your dataset is set to **unlisted**, meaning nobody else is able to see it, except you. Use **published** flag to make it publicly available*

```
data push [FILE-or-DATA-PACKAGE-PATH] --published
```

**Login before pushing**: you will need to login (or signup) before pushing:

```
data login
```

This will carry out login / signup entirely from the command line.

## `init`

`data` has an `init command that will automatically generate Data Package metadata including scanning the current directory for data files and inferring [table schema] for tabular files.

Find out more by running:

```
data help init
```

