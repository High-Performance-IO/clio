+++
archetype = "section"
title = "I/O Graph"
weight = 2
+++

This section defines the dependencies between input and output streams between application modules comprising the workflow. It is identified by the keyword `IO_Graph`, and it requires an array of objects. Thoose object have as key the application step name. Each IO_Graph step can define two different sections: `input` and `output`:

```json
"IO_Graph": {
  "step1":{
    "input": {
    },
    "output":{
    }
  }
}
```

Bare in mind that the definition of the input and output section inside the IO_Graph elements can be optional, and if omittted (ie. only the step name is defined, the input and output streams are considered to be running with default options).

## Input and output objects

The definition of thoose sections are the same, we shall describe here the general syntax. Inside them, we have a set of objects with the name (or the glob) as key:

```json
"IO_Graph": {
  "step1":{
    "input|output":{
      "file1": {
      },
      "file2": {
      }
    }
  }
}

```

## File object

For each file, the following fields can be specified:

- `committed`: This keyword defines the “commit rule” associated with the files or directories identified with the keywords name and dirname, respectively. Its value can be either `on_close:N` (where N is an integer ≥ 1), `on_termination`, or `on_file` if the “commit rule” applies to filenames (i.e., if the name keyword is name). Instead, if the “commit rule” applies to directory names its value can be either `on_termination`, `on_file`, or `n_files:N` (where N is an integer ≥ 1). If the committed keyword is not specified, the default “commit rule” is `on_termination`. If the commit rule semantics is `on_file`, then the keyword `files_deps`, whose value is an array of filenames or directory names, defines the set of dependencies.
- `mode`: t his keyword defines the “firing rule” associated with the files and directories identified with the keys name and dirname, respectively. Its value can be either `update` or `no_update`. If the mode keyword is not specified, the default “firing rule” is `update`.
- `permanent`: this is a boolean flag, used to specify those files that will be stored on the filesystem at the end of the workflow execution
- `exclude`: this is a boolean flag, used to specify those files that will not be handled by CLIO even if they will be created inside the CLIO_DIR directory.
- `policy`: this is an object that defines the home node policy of the file. See below for more informations.

## Home Node Policy

This object defines the Home Node Policy for the file. There are three different policies in capio.

- `create` (default option)
- `manual`
- `hashing`

To express the policy the syntax is the following:

```json
"policy":{
  "name": "create|manual|hashing",
  "source": "stepName:LogicalName"
}
```

The name specifies the home node policy. If the value for name is `create` of `hashing`, the `"source"` key is ignored. If the value for name is `manual` the key `"source"` is then required and it will contain the step name followed by the logical name of the the application. The logical name is a name that is expected to be given to the application that runs the step that is expected to produce or read the file.

## Example

The following snippet is a valid example of the `IO_Graph` section:

```json
  "IO_Graph": {
     "writer": {
       "input": {
         "alias1": {
           "committed": "on_termination",
           "mode": "update",
           "permanent": false,
           "exclude": false
         },
         "file1.dat": {
           "committed": "on_close",
           "mode": "update",
           "permanent": true,
           "exclude": false,
           "policy": {
             "name": "hashing"
           }
         },
         "file2.dat": {
           "committed": "on_termination",
           "mode": "update",
           "permanent": false,
           "exclude": false,
           "policy": {
             "name": "manual",
             "source": "stepName:LogicalName"
           }
         }
       },
       "output": {
         "file3.dat": {
           "committed": "on_close:10",
           "mode": "no_update",
           "permanent": false,
           "exclude": false
         },
         "dir": {
           "kind": "directory",
           "committed": "n_files:1000",
           "mode": "no_update"
         }
       }
     }
   }

```
