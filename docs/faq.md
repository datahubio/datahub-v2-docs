---
Title: FAQ
---

[[toc]]

### What is the DataHub project

Full answer is here: https://datahub.io/docs/about

Shortly, DataHub is a place where people can:
- find a lot of high-quality data
- store their own data
- share data with other people (colleagues, customers)

### What do you mean, saying "data"?

In the narrow sense `data` is just files with records, e.g. CSV, xls, etc...
In the broad sense `data` is a organized set of information. In that case we call it `Data Package`, `DataSet`, `Package`
Also `data` is the name of our [command line tool](https://datahub.io/docs/features/data-cli), that used to work with data packages and Data Hub.

*So you can use the data program to get a data package with data files from the data hub.*

### What are 'Data Package' and 'Dataset'?

Data Package or DataPackage or Dataset or Package are is the same thing in our context.
It is a way of "packaging" up data - put the set of data files together with a special "metadata" file (also called 'descriptor'), that describes:
- name, title, source of the data set
- list of "resources" - data files presented in the package
- each resource file structure
- license 

Data package also has a `README.md` file, that contains a general description of the data package for humans.

This links would help you, too:
- [data package description](https://datahub.io/docs/data-packages).
- [data package specification](https://frictionlessdata.io/docs/data-package/).

### What is the descriptor?

The descriptor is the central file in a Data Package. It provides:

- General metadata such as the package's title, license, publisher etc
- A list of the data "resources" that make up the package including their location on disk or online and other relevant information (including, possibly, schema information about these data resources in a structured form)

A Data Package descriptor MUST be a valid JSON object. (JSON is defined in [RFC 4627](https://www.ietf.org/rfc/rfc4627.txt)). When the descriptor is available as a file it MUST be named `datapackage.json` and it MUST be placed in the top-level directory (relative to any other resources provided as part of the data package).

Read the [full descriptor specification here](https://frictionlessdata.io/specs/data-package/#specification)

### How to store my data on the Data Hub?

Install our `data` [command line tool](https://datahub.io/download).  
Then:
```bash
# go into the folder with your data
cd path/to/my/data

# create a data package
data init

# login on the DataHub (once)
data login

# push the data package
data push

# follow the online link `data push` has printed
```

Also you can read this guides:
- https://datahub.io/docs/data-packages/publish-faq
- https://datahub.io/docs/getting-started/publishing-data
- https://datahub.io/docs/getting-started/datapackage-find-prepare-share-guide

### Questions about the showcase page

Q: What is the showcase page?
A: It is a web-page that presents one particular data package, stored on the DataHub, e.g. http://datahub.io/core/finance-vix

Q: Will keywords property be used?
A: keywords property from the descriptor are not used on the showcase page ATM

Q: Will description property in datapackage.json be used to populate the Showcase description? How does that relate to the Readme section?
- Yes, the showcase page description is populated from the description property in the datapackage.json.
- If the description property is not provided - it is populated from the Readme section.
- If the Readme property is not provided - it is populated from the `README.md` file from the package folder, when you run `data init`

Q: Will homepage property be used? I'm using it to link back to the original dataset page on UKDS.
- Showing the homepage property is in our roadmap: https://github.com/datahq/datahub-qa/issues/221
- we also show the link from `sources[0].path` 

Q: Will additional admin tools be available for the Showcase page?
A: we are going to implement 'delete' and some other functions, and probably we'll make an admin page for these.

Q: Currently page is 'unlisted', is there a way to change this to 'public'? 
A: You have to republish your data with `data push --public` command

Q: Are my datasets listed anywhere, I can't see my 'unlisted' datasets on my profile page or dashboard.
A: We have fixed this bug, so:
   You can see your 'unlisted' datasets on the profile page and dashboard.

### Can I get an organization account/username rather than my personal username?

Yes, please contact us via support@datahub.io or https://gitter.im/datahubio/chat

### Is there a way I can upload a dataset and be part of an organization?

This feature is in the roadmap:
- https://github.com/datahq/datahub-qa/issues/13
- https://github.com/datahq/datahub-qa/issues/21

