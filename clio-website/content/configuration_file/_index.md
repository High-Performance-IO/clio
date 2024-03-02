+++
archetype = "chapter"
title = "V1.1 configuration file"
weight = 2
+++

A valid CLIO V1.1 configuration file is comprised of five different sections:

- [Workflow name](workflow_name/_index.md): identifies the application workflow composed by multiple application modules.
- [Alias](alias/_index.md): groups under a convenient name a set of files or directories.
- [IO_Graph](IO_Graph/_index.md): describes file data dependencies among application modules.

From V1, CLIO V1.1 is less verbose and is less prone to user error. This is because now it is not possible to define multiple streaming rules for the same file.
