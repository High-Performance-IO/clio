+++
archetype = "chapter"
title = "V1 configuration file"
weight = 2
+++

A valid CLIO V1 configuration file is comprised of five different sections:

- [Workflow name](workflow_name/_index.md): identifies the application workflow composed by multiple application modules;
- [IO_Graph](IO_Graph/_index.md): describes file data dependencies among application modules;
- [Alias](alias/_index.md): groups under a convenient name a set of files or directories;
- [Permanent](permanent/_index.md): defines which files must be kept on the permanent storage at the end of the workflow execution;
- [Exclude](exclude/_index.md): identifies the files and directories not handled by the CLIO implementation;
- [Home Node Policy](home_node_policy/_index.md): defines different file mapping policies to establish which CLIO servers store which files;