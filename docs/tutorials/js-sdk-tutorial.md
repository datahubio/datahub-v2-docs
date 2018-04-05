---
Title: Datahub JS SDK Tutorial
---

## Introduction

The [DataHub](http://datahub.io/) platform stores a lot of different datasets - which are packages of useful data alongside with the description (here is [the dataset specification](https://frictionlessdata.io/docs/data-package/)). The data, stored on the DataHub, has a nice structure, views and a description that help people to get insights.
You can also store and share your own datasets on the DataHub

As a programmer, you may want to automate the process of getting or storing the data. Also you may want to integrate your project and the DataHub.

This tutorial will cover information you need to create the data driven project that will use the DataHub to get or store the data.

Let's explore the tutorial together below.

**Important notes:**
- You need to use **Node version > 7.6**
- When you see the **await** keyword you should wrap the code in the **async** function.


## Install

Add `datahub-client` package into your project:
```shell
npm install datahub-client --save
```
The `datahub-client` is a JS library to communicate with the DataHub and to create and validate datasets. It also includes the `data.js` package.
`Data.js` provides `Dataset` and `File` classes, that represents a datapackage and a data-file.


## Quick overview

After loading 'datahub-client' into your program:
`const datahub = require('datahub-client')` ,
you can do things like:

- login to DataHub
- authenticate with the jwt token
- push a dataset:
- get the data from the DataHub:
- init a dataset
- validate a dataset
- convert tabular data files into different formats

Let's explore some of the `datahub-client` features more deeply below.


## Push data

### Prepare the test data and user credentials
1. Here is a [Sample dataset on Github](https://github.com/datasets/finance-vix/archive/master.zip).  Please, download and unpack it.

2. Get the user credentials from the server (token and ownerid):
   - install our cli tool: [installation instructions](https://datahub.io/docs/getting-started/installing-data)
   - run `data login` and login on the DataHub in the browser
   - run `cat ~/.config/datahub/config.json`
```
{
  "token": "secure jwt token",
  "profile": {
    "id": "userId",
    /* other user info */
  }
}
```

### Push code example

```javascript
const {DataHub} = require('datahub-client')
const {Dataset} = require('data.js')

async function pushDataset(datasetPath){
   // load dataset that we want to push
   let dataset = await Dataset.load(datasetPath)
   // Create an instance of the DataHub class, using the data from the user config
   const datahubConfigs = {
     apiUrl: 'http://api.datahub.io/',
     token: 'secure jwt token',
     debug: false,
     ownerid: 'userId',
   }
   const datahub = new DataHub(datahubConfigs)

   // now use the datahub instance to push the data
   const res = await datahub.push(dataset, {findability: 'unlisted'})
   console.log(res)
}
```

**Note:** findability: could be 'unlisted', 'published' or 'private'

Using the function:
```javascript
pushDataset('datasetPath')
```
This is an example of correct console output:
```javascript
{ dataset_id: 'username/finance-vix',
  errors: [],
  flow_id: 'username/finance-vix/40',
  success: true }
```

If you get any errors - change the debug option: `datahubConfigs = {debug: true, ...}`, to see the detailed log.

## Get data from the datahub

Getting data is much easier, because you don't need to authorize for it (except the private datasets).

You can get a dataset using only the `data.js` library:
```javascript
const {Dataset} = require('data.js')

const descriptorUrl = 'https://datahub.io/core/finance-vix/datapackage.json'
const dataset = await Dataset.load(descriptorUrl)
```
Then you can use the data from the dataset in different ways:
- get the list of all resources:
```javascript
for (let res of dataset.resources) {
  console.log(res._descriptor.name)
}
```

- get all tabular data (if exists):
```javascript
for (let res of dataset.resources) {
  if (res._descriptor.format === "csv") {
    // Get a raw stream
    const stream = await res.stream()
    // entire file as a buffer (be careful with large files!)
    const buffer = await res.buffer
    // print data
    stream.pipe(process.stdout)
  }
}
```

## The end

Congratulations! Your program can now push and get the data from the datahub! To learn how to do a lot of other things, please read the full documentation here: https://github.com/datahq/datahub-client

For any questions you have - please visit our [gitter channel](https://gitter.im/datahubio/chat).
