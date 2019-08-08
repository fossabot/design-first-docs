---
title: Generate
type: guide
order: 202
vue_version: 0.0.1
---

After modifying the `design.json` file, you should run the `generate` command. The `generate` command will take the services and actions specified and generate the necessary scaffolding.

The command overwrites the `internal` directory, so files in that directory should never be modified. All other files in all other directories will NEVER be overwritten so you can safely call the `generate` command multiple times without fear of losing your work. If the command encounters a file that it cannot overwrite, it will output a warning to the console that writing that file was skipped.

This skipping behavior of the `generate` command means that if you make any changes to your `design.json` file to an existing action, those changes will not be implemented (outside of any changes that would be implemented in the `internal` directory, because those files are overwritten). Therefore, you should first change the filenames of any files that need updating, e.g. `$ mv ./authenticate/service/action/index.ts ./authenticate/service/action/index.ts.bak`.

The `generate` command takes an optional design file location input but defaults to `./design.json`.

Shorthand aliases `gen` and `g` are also available.

```bash
$ design-first g my-design-file.json
```
