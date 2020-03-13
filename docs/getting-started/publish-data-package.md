---
title: Publish a Data Package on Datahub
date: 2020-03-11
---

**In this guide, we'll take a look at how to publish a data package using the [data CLI tool][cli].**

> To learn more about what a Data Package is and how to create one check out this [detailed tutorial.](/datapackage-find-prepare-share-guide)

## Publishing a Data Package

Now, we want to publish our Data Package with the world, to do that, we need to use the Data CLI command `data push`:

```bash
$ data push --public
Completed: scripts/process.py
Completed: datapackage.json
Completed: archive/source.csv
  Uploading [******************************] 100% (0.0s left)   archive/source.csv🙌  your data is published!
🔗  https://datahub.io/AcckiyGerman/airport-codes/v/1 (copied to clipboard)
```

The Data Package is now stored online and you and your colleagues can access it using the browser: [http://datahub.io/your-name/data-package-name/](/)

That's it. Our goal is reached! We have published a Data Package online using the [Data CLI][cli] tool.

Thanks for reading.

[cli]: https://datahub.io/tools
