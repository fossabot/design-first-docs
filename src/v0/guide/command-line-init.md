---
title: Initialize
type: guide
order: 201
vue_version: 0.0.1
---

Initialize is the first command to run when building a new api. The command only needs to be run once and is used to generate the api directory.

The command only accepts one input, the api name. The name can only contain alphanumeric characters, underscores and dashes.

The command will create a new directory in the current location with the name of the api. The command also copies some subdirectories, a package.json file, etc.

Shorthand aliases `init` and `i` are also available.

```bash
$ design-first i my-first-api
```

After creating a new api for the first time, change directories into the folder `$ cd my-first-api` and copy the two example files:

```bash
$ cp .env.example .env
$ cp design.example.json design.json
```
