# Exclude section

This section, identified by the keyword `exclude`, is used to specify those files that will not be handled by CLIO even if they will be created inside the CLIO_DIR directory. Its value is an array of file names (whose values can also be aliases).  

## Example

The following snippet is a valid section for the `exclude` section:

```json
{
    "name" :"my_workflow",
    "exclude" :["file1.dat", "group0", "*.tmp", "*~" ]
}
```
