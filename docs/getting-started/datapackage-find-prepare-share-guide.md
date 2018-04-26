---
title: Data Package Find-Prepare-Share Guide
date: 2018-04-26
authors: Dmytro Gierman, Branko Djordjevic
---

**In this guide we describe the way to gather the information and to make it presentable and easy to use for your customers.**

With the proliferation of internet one cannot escape the overwhelming information all around us. With this much data, fortunes are made by those that can make sense of datasets and present it in a clear and succinct manner.

[[toc]]

# What is Data Package

Let's say that you obtained data about pharmaceutical drug spending by countries and that you already have it saved in a [comma separated values](https://en.wikipedia.org/wiki/Comma-separated_values) (CSV) file.

You wish to present the data to the appropriate audience and csv format is great for tabular representation. The user can clearly see what ammount in which year has the country spent on pharmaceuticals. But there are things that csv file can't show us. We don't know for example if in the column **TIME** there are only integer type years like 1972 or will we find something like April 1972 which is string.

Users also may want to see:
- general description of the data
- information about the sources 
- license information
- data file structure information
- etc

All this information is called **metadata** and should be stored alongside with the data to make the information frame around the raw data you have, so users could understand the context.

And here comes the idea of [**Data Package**](https://datahub.io/docs/data-packages).

**Data Package** - *is a data plus a meta-information about it.*

Data Package is a simple container format used to describe and package a collection of data. The format provides a simple contract for data interoperability that supports frictionless delivery, installation and management of data.

Data Package stores the meta-information in the special **Descriptor File**. It has [JSON](https://en.wikipedia.org/wiki/JSON) format and a special name: `datapackage.json`. Here you can read a short [descriptor specification](https://frictionlessdata.io/specs/data-package/#specification).

*Descriptor File* could have a complex structure, but don't worry - you don't need to type it manually or write programs for creating it. Soon we'll show how to make it automatically.

But, before creating a data package you need to get the data and to clean it.
Please, continue to read about how to get data for your Data Package.

# Getting data

After finding the source of data (some organization, web-site, internet archive, etc), there are few common ways to get the data:

### 0. you already have the data files

You could have a lot of useful data already as a result of your work.

### 1. manually download data files

If you found a target files in the web, and the data is not updating regularly - you can just download files and put them into, say, "sources" or "archive" folder for future analysis.

### 2. automatic download

If the data source is updated regularly, it would be natural to automate the process of getting data, using some script.

Here is simple example in Python:
```python
# python programm
import urllib

source = 'http://ourairports.com/data/airports.csv'
archive = 'data.csv'

urllib.urlretrieve(source, archive)
```
You can also use a shell script:
```shell
#!/usr/bin/sh
SOURCE='http://ourairports.com/data/airports.csv'
wget -O data.csv $SOURCE
```
As a result you'll get the `data.csv` file in the current program directory.

> You also need to setup cronjob to actually automate it (and probably more). Not sure why do we need this part [name=Irakli] 
> Why not? 50% of processing scripts has a download code.
 [name=Dima]

### 3. Get data from the API

Sometimes the data source organization has a web API endpoints.

In that case you should read the API documentation to use it.

For example U.S. Energy Information Administration published their instructions here: https://www.eia.gov/opendata/commands.php 
Example of using their API:
```python
# python script
import requests
import json

API_URL = 'http://api.eia.gov/series/'
series_id = 'NG.RNGWHHD.D'

response = requests.get(API_URL, params={
    'series_id': series_id,
    'out': 'json'
})

data = response.json()
```

Other good example of getting data via API is described here: http://okfnlabs.org/handbook/data/tutorial/#from-an-api

Web APIs often return data in json format, so then you need to analyze the json file structure and write a script to derive the information you need.

If the API returns a CSV file - you can save this file or load it directly into your program.


# clean and transform data (and why)

Now you have data, stored on your disk, probably it is a CSV or a XLS file.

Let's make this data clean and nice, so it becomes machine-friendly and your customers could use their programs to analyze and process information automatically.

## clean and prepare

Use this check-list:
- data files has no comments (move them into README)
- data files has no empty rows
- tables are unpivoted ([what is pivot and unpivot](https://www.excel-university.com/unpivot-excel-data/))
- We prefer using . rather than , to separate decimal values.
- If data is not available, you either make that cell as 0 or NaN.
- The data package name should be your-package.csv. We rather use `-` to refer to spaces.

## use python

By now you can understand why we use programming skills here, since you can basically automate most of these processes.

We are not going to teach you using Python, but here is some tips, that would help:
- refresh your memory with a list of [common String operations](https://docs.python.org/2/library/string.html)
- use `csv` module for working with csv
- use `xlrd` or any similar module for handling excel files
- use `pandas` module and its `melt()` method to unpivot tables
- don't try to create a perfect program but rather a simple script
  Our focus here is the data itself, not the data processing combine
- use git & [github](http://github.com/) to keep your code

The project structure could be like this:
```bash
folder
 ├── archive
 │    └── source.csv    # original files
 ├── data
 │    └── data.csv      # cleaned files
 ├── README.md          # Full description of the package for Humans
 └── scripts
      └── process.py    # script that gets and cleans the data
```

If you prefer to use UNIX shell scripts instead of Python, this guide will help you:
https://github.com/rufuspollock/command-line-data-wrangling

We also have to describe the data files format, which gives the ability to prove that data is valid. The next section describes how to do it:

# create and share a data package

Now you have downloaded and prepared the data.

To create a **Data Package** you need to describe the data structure and to save this information into `datapackage.json` descriptor file.

The easiest way is to use the [command line tool](https://datahub.io/Download) from the [DataHub project](https://datahub.io).

Install the program, go to the data project folder and run `data init ` there:
```
$ data init
> This process initializes a new datapackage.json file.
> Once there is a datapackage.json file, you can still run `data init`
  to update/extend it.
> Press ^C at any time to quit.

> Detected special file: README.md
> archive/source.csv is just added to resources
> data/data.csv is just added to resources
> scripts/process.py is just added to resources
> Default "ODC-PDDL" license is added. If you would like to add a different
  license, run `data init -i` or edit `datapackage.json` manually.
> 
💾 Descriptor is saved in "datapackage.json"
```

Now you can examine the descriptor file and update or fix it if you think something is wrong (title, description, data field formats, redundant resources, etc):
```json
{
  "name": "airport-codes",
  "title": "Airport codes",
  "resources": [
    {
      "path": "archive/source.csv",
      "pathType": "local",
      "name": "source",
      "format": "csv",
      "mediatype": "text/csv",
      "encoding": "ISO-8859-1",
      "dialect": {
        "delimiter": ",",
        "quoteChar": "\""
      },
      "schema": {
        "fields": [
          {
            "name": "id",
            "type": "integer",
            "format": "default"
          },
....
```
After making changes, to ensure that descriptor and data are correct, run `data validate`. You will see if any errors appear:
```bash
$ data validate path_to_invalid_data_package
> Error! Validation has failed for "missing-column"
> Error! The column header names do not match the field names in the schema on line 2
```
After fixing them you will see:
```bash
$ data validate path_to_data_package
> Your Data Package is valid!
```

Now everything is OK and its time to share your data with the world. Use `data push` for that:
```
$ data push --public
Completed: scripts/process.py
Completed: datapackage.json
Completed: archive/source.csv
  Uploading [******************************] 100% (0.0s left)   archive/source.csv🙌  your data is published!
🔗  https://datahub.io/AcckiyGerman/airport-codes/v/1 (copied to clipboard)
```
Now the Data Package is stored online and your colleagues could reach it with the browser: [http://datahub.io/your-name/data-package-name/]()

That's it. Our goal is reached! We have
- gathered information
- cleaned the data
- created a Data Package
- pushed it online

Thank you for reading.

## Additional materials

Read more about [DataHub command line tool](https://datahub.io/docs/features/data-cli).

Creating Data Packages examples and a [lot of information about the Data Hub](https://datahub.io/docs).

Ask your question in the chat channel: https://gitter.im/datahubio/chat
