+++
archetype = "section"
title = "Alias"
weight = 0
+++
This section does not have any changes from V1. 
The aliases section, identified by the keyword aliases is useful to reduce the verbosity of the enumeration of files an application can consume or produce. It is a vector of objects composed of the following items:

- `group_name`: the alias identifier  
- `files`: an array of strings representing file names.

## Example

The following snippet is a valid alias section for CLIO configuration language

```json
{
    "name" : "my_workflow",
    "aliases" : [
      {
        "group_name" : "group-even",
        "files" : ["file0.dat", "file2.dat", "file4.dat"]
      },
      {
        "group_name" : "group-odd",
        "files" : ["file1.dat", "file3.dat", "file5.dat"]
      }
   ] 
}
```  
