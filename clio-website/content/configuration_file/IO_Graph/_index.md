+++
archetype = "section"
title = "I/O Graph"
weight = 2
+++


This section defines the dependencies between input and output streams between application modules comprising the workflow. It is identified by the keyword `IO_Graph`, and it requires an array of objects. Those objects specify the input and output streams for each application component. Each object defines the following items:

- `name`: The name of the application. This keyword is mandatory
- `input_stream`: This keyword identifies the input files and directories the application module is expected to read. Its value is a vector of strings. This keyword is optional
- `output_stream`: This keyword identifies a vector of files and directory names, i.e., the files and directories the application module is expected to produce. Its value is a  vector of strings. This keyword is optional.
- `streaming`: This optional keyword identifies the files and directories with their streaming semantics, i.e., the “commit and firing rules” (please see [Semantics](semantics.md) for a more detailed description). Its value is an array of objects.

## Streaming section
Each object inside the streaming section may define the following attributes:
- `name`: The filenames to which the rule applies. The value of this keyword is an array of filenames.
- `dirname`: The directory names to which the rule applies. The value of this keyword is an array of directory names.
- `committed`: This keyword defines the “commit rule” associated with the files or directories identified with the keywords name and dirname, respectively. Its value can be either `on_close:N` (where N is an integer ≥ 1), `on_termination`, or `on_file` if the “commit rule” applies to filenames (i.e., if the name keyword is name). Instead, if the “commit rule” applies to directory names its value can be either `on_termination`, `on_file`, or `n_files:N` (where N is an integer ≥ 1). If the committed keyword is not specified, the default “commit rule” is `on_termination`. If the commit rule semantics is `on_file`, then the keyword `files_deps`, whose value is an array of filenames or directory names, defines the set of dependencies.
- `mode`: his keyword defines the “firing rule” associated with the files and directories identified with the keys name and dirname, respectively. Its value can be either `update` or `no_update`. If the mode keyword is not specified, the default “firing rule” is `update`.

## Example

The following snippet is a valid example of the `IO_Graph` section:

```json
{
    "name" : "my_workflow",
    "IO_Graph": [
      {
        "name": "writer",
        "output_stream": ["file0.dat","file1.dat","file2.dat","dir"],
        "streaming": [
            {
                "name": [ "file0.dat" ],
                "committed": "on_termination",
                "mode": "update"
            },
            {
                "name": [ "file1.dat" ],
                "committed": "on_close",
                "mode": "update"
            },
            {
                "name": [ "file2.dat" ],
                "committed": "on_close:10",
                "mode": "no_update"
            },
            {
                "dirname": [ "dir" ],
                "committed": "n_files:1000",
                "mode": "no_update"
            }
        ]
      },
      {
        "name": "reader",
        "input_stream":  ["file0.dat", "file1.dat", "file2.dat", "dir"]
      }
    ]
}
```
