---
title: Publish a data package on Datahub
date: 2020-03-11
---
[cli]: https://datahub.io/tools

**In this guide, we'll have a look at how to publish a data package using the [data CLI tool][cli]. Before we dive in, let's review what a data package is.**

# What is Data Package?
[dp]: https://datahub.io/docs/data-packages

[**Data Package**][dp] - *is a data plus a meta-information about it.*

Data Package is a simple container format used to describe and package a collection of data.

Data Package stores the meta-information in the special **Descriptor File**. It has a [JSON](https://en.wikipedia.org/wiki/JSON) format and a special name: `datapackage.json`. Here you can read a short [descriptor specification](https://frictionlessdata.io/specs/data-package/#specification).

*Descriptor File* could have a complex structure, but don't worry - you don't need to type it manually or write programs for creating it. Soon we'll show how to make it automatically.

[You can learn more information about data packages here.][dp]

# Create a Data Package
[csv]:https://datahub.io/docs/data-packages/csv
We have a collection of data for airport codes stored in [CSV][csv] format and want to create a data package and then publish it online.
First, to create a **Data Package** we need to describe the data structure and save this information into a `datapackage.json` descriptor file.

The easiest way to do this is to use the [command line tool](https://datahub.io/Download) from the [DataHub project](https://datahub.io).

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
After fixing them you should see:
```bash
$ data validate path_to_data_package
> Your Data Package is valid!
```

## Publish your Data Package
Now everything is OK, its time to publish your data with the world. Use `data push` for that:
```
$ data push --public
Completed: scripts/process.py
Completed: datapackage.json
Completed: archive/source.csv
  Uploading [******************************] 100% (0.0s left)   archive/source.csv🙌  your data is published!
🔗  https://datahub.io/AcckiyGerman/airport-codes/v/1 (copied to clipboard)
```
The Data Package is now stored online and you and your colleagues can access it using the browser: [http://datahub.io/your-name/data-package-name/](#)

That's it. Our goal is reached! We have
- Created a Data Package
- Published it online using the [Data CLI][cli] tool

Thanks for reading.
