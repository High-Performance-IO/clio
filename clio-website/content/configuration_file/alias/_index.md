+++
archetype = "section"
title = "Alias"
weight = 2
+++

The aliases section, identified by the keyword aliases is useful to reduce the verbosity of the enumeration of files an application can consume or produce. It is an object made up of vectors that have the alias name as key.


## Example

The following snippet is a valid alias section for CLIO configuration language

```json
  "aliases": {
    "group-even": ["file0.dat","file2.dat","file4.dat"],
    "group-odd": ["file1.dat","file3.dat","file5.dat"]
  }
```
