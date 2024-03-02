+++
archetype = "chapter"
title = "Examples"
weight = 3
+++

In this section, we introduce how to express the commit and firing semantics using the CLIO coordination language, providing simple examples.

> [!NOTE]
> For the following instances, we consider a workflow comprising two applications: an application called `writer` that produces two files and an application called `reader`, that reads these two files.

Typically, this straightforward workflow is executed in a classic batch execution. First, the `writer` application is launched to produce the input files for the `reader` application. Then, the `reader` application can start and consume the files, reading them from the filesystem.

CLIO, allows a CLIO middleware implementation  to run both applications concurrently without modifying the code of the two modules. To achieve this effectively, CLIO requires the declaration of the data streaming semantics (as described in [Semantics section](semantics.md)) it should enforce on the produced and consumed files to guarantee the correct execution.

## Commit on Termination, Fire on Commit (CoT-FoC)

The more stringent semantic in terms of streaming capabilities is expressed by `Commit on-Termination` (CoT) with `Fire On-Commit` (FoC) firing rule.

When this semantics is applied to a file, the reader can start reading that file only after the writer application has terminated. This proves helpful when a section of the file can be updated multiple times, and there is no knowledge about when the writer will cease adding data records. In this case, the correct behavior is to wait for the writer to terminate.

When the CLIO implementation captures a `read()` system call on a file with such stringent semantics, it will return the data only upon the termination of the writer application.

With the CoT-FoC semantics, there is no ongoing streaming communication. However, running both applications concurrently can still be beneficial, especially in cases where the  reader must perform substantial computation before reading the data from the writer application. The following configuration file expresses the CoT-FoC semantics for the simple example considered:

```json
{
   "name":"my_workflow",
   "IO_Graph":[
      {
         "name":"writer",
         "output_stream":["file1.dat", "file2.dat" ],
         "streaming":[
            {
               "name": [ "file1.dat", "file2.dat" ],
               "committed":"on_termination",
               "mode":"update"
            }
         ]
      },
      {
         "name":"reader",
         "input_stream":["file1.dat","file2.dat"]
      }
   ]
}
```

## Commit on Termination, Fire no Update (CoT-FnU)

In the next example, the structure of the workflow and its data dependency remain unchanged; only the producer-consumer semantics changes.

From a syntax standpoint, the only section that needs to be modified is the one related to the keyword `streaming`.

The example listed below, shows the configuration file for the workflow with the semantics `Commit on-Termination` (CoT), `Firing no-Update` (FnU). With this semantics, the reader may commence reading the files file1.dat and file2.dat as soon as the writer produces them. The reader will receive the End-of-Stream (EOS) signal upon reaching the end of the file and after the termination of the writer module. In this case, there are more opportunities for streaming communication than in the previous semantics.

This semantics is useful when the user knows that once a section of the file is written, it will not be modified, but they do not know when the writer will stop writing data into the file. For the workflow under consideration, this semantics involves streaming only on the first file (i.e., file1.dat) because the writer and reader write/read files in a sequential manner (first file1.dat, then file2.dat, and so on). In this scenario, the reader will read the first file until both of these conditions are true:

- It reaches the end of the file
- The writer terminates

When these two conditions are met, the CAPIO middleware will return the EOS signal to the reader, who will then proceed to read the second file.

```json
{
   "name":"my_workflow",
   "IO_Graph":[
      {
         "name":"writer",
         "output_stream":["file1.dat", "file2.dat"],
         "streaming":[
            {
               "name": [ "file1.dat", "file2.dat" ],
               "committed":"on_termination",
               "mode":"no_update"
            },
         ]
      },
      {
         "name":"reader",
         "input_stream":["file1.dat","file2.dat"]
      }
   ]
}
```
