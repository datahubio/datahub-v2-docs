---
title: API instructions
---

# Intro

The datahub stores datapackages (datasets). Datapackage is consist of:
- the **Data**: file(s) with the data. Usually it is tabular (csv, xls) but could be any file type.
- the **Metadata**: descriptor file, named `datapackage.json`, that contains all the information you need to find and to use the data itself

Detailed description of the *datapackage* format is here: http://datahub.io/docs/data-packages

# GET API

## Get files via `/r/` endpoint
Use our `/r/` endpoint, if you know the name of the file you need in the dataset, e.g. for `data.csv`:
```
GET https://datahub.io/<owner>/<dataset_name>/r/data.csv
```
If you don't know the filename, or there is a lot of files in the dataset, use our enumeration logic:
```
GET https://datahub.io/<owner>/<dataset_name>/r/0.csv
GET https://datahub.io/<owner>/<dataset_name>/r/1.csv
GET https://datahub.io/<owner>/<dataset_name>/r/2.csv
...
```

The datahub.io path logic is described here: [getting-data#perma-urls-for-data](http://datahub.io/docs/getting-started/getting-data#perma-urls-for-data)

## Get the descriptor
If you need to see the list of the files in the dataset, or other metadata, then get the descriptor (`datapackage.json`):
```
GET https://datahub.io/<owner>/<dataset_name>/datapackage.json
```
Now you could parse the descriptor to get each resource (data) path:
```python
descriptor = json.load('datapackage.json')
for resource in descriptor.resources:
    print(resource.name, resource.path)
```
Then you can easily get all the files.

# POST API

The easiest way to post data on the datahub is to use our CLI tool, instructions is here: [publishing_data](http://datahub.io/docs/getting-started/publishing-data).

Here is the way to POST data from your application:
* Read the Documentation: http://docs.datahub.io/developers/api/
* Use our JS lib, that interacts with the datahub: https://github.com/datahq/datahub-client
    - You'll need following module to work with DataHub API: https://github.com/datahq/datahub-client/blob/master/lib/utils/datahub.js
* Example of use: https://github.com/datahq/data-cli/blob/master/bin/data-push.js
