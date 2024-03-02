+++
archetype = "section"
title = "Permanent"
weight = 3
+++

This language section, identified by the keyword `permanent`, is used to specify those files that will be stored on the filesystem at the end of the workflow execution. Its value is an array of file names (whose values can be also aliases).  

## Example

The following snippet is a valid example of how the `permanent` section should be specified:

```json
{
    "name" :"my_workflow",
    "permanent" :["output.dat", "group0" ]
}
```

At the end of the execution of “my_workflow”, the file output.dat and all files belonging to group0 will be stored in the file system.
