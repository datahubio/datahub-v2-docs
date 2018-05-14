---
title: Datahub JS SDK Tutorial
---

This tutorial helps you to get starting with DataHub JS SDK for data deployment.

[[toc]]

## Introduction

### Prerequisites

You need to have NodeJS (`>= 7.6`) and NPM.

### Libraries

The `datahub-client` is a JS library to communicate with the DataHub platform:

https://npmjs.com/package/datahub-client

The `data.js` library is a lightweight library providing a standardized interface for accessing data files and datasets:

https://npmjs.com/package/data.js

## Installation

```bash
npm install datahub-client data.js --save
```

## Deploy data

### Prepare the test data and user credentials

1. Here is a [Sample dataset on Github](https://github.com/datasets/finance-vix/archive/master.zip).  Please, download and unpack it.

2. Get the user credentials (token, id and username) and set them as environment variables:

```bash
export token=token id=id username=username
```

:::info
If you don't have them, you need to generate using our [CLI tool](https://datahub.io/download). After running `data login` command, credentials can be found at `~/.config/datahub/config.json`.
:::

### Push code example

```javascript
const {DataHub} = require('datahub-client')
const {Dataset} = require('data.js')

async function pushDataset(datasetPath){
   // Load dataset that we want to push
   const dataset = await Dataset.load(datasetPath)
   // Create an instance of the DataHub class, using the data from the user config
   const configs = {
     apiUrl: 'http://api.datahub.io/',
     token: process.env.token,
     ownerid: process.env.id,
     debug: false
   }
   const datahub = new DataHub(configs)

   // Now use the datahub instance to push the data
   const res = await datahub.push(dataset, {findability: 'published'})
   console.log(res)
}

pushDataset('path/to/dataset')
```

**Note:** findability: could be 'unlisted', 'published' or 'private'.

This is an example of correct console output:

```javascript
{ dataset_id: 'username/finance-vix',
  errors: [],
  flow_id: 'username/finance-vix/1',
  success: true }
```

If you get any errors - change the debug option: `datahubConfigs = {debug: true, ...}`, to see the detailed log.

## Get data from the datahub

Getting data is much easier, because you don't need to authorize for it (except for the private datasets).

```javascript
const datahub = require('datahub-client')
const {Dataset} = require('data.js')

const dataset = await Dataset.load(datasetUrl)
const resources = await datahub.get(dataset)
```

`Dataset.load()` takes a path to the data package and returns a dataset object: https://github.com/datahq/data.js#datasets

`datahub.get()` method accept the dataset object and returns an array with resources from it.

Each resource in the `resources` is the special File object from the `data.js` lib: https://github.com/datahq/data.js#files

## The end

Congratulations! Your program can now deploy and get the data from the DataHub! To learn how to do a lot of other things, please read the full documentation here: https://github.com/datahq/datahub-client

For any questions you have - please visit our [gitter channel](https://gitter.im/datahubio/chat).
