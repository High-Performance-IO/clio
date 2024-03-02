+++
archetype = "home"
title = "CLIO: Coordination Language for IO"
+++

CLIO is a new I/O coordination language that allows users to annotate file-based workflow data dependencies with synchronization semantics information related to files and directories.  

CLIO, aims to enable the transparent overlap of computation and I/O operations among distinct producer-consumer application modules.

The language used for expressing the I/O coordination language syntax and semantics is JSON (JavaScript Object Notation). JSON is not tied to any particular programming language or platform and is widely supported across various programming languages (Java, C++, Python, etc.).  

Although it is not the most commonly adopted language in the context of high-level coordination languages for expressing parallel computations, it provides automatic syntax validation features through the JSON schema.  Using JSON can make the learning curve less steep for the users.

## Semantics of CLIO  

The semantics for CLIO is described in [Semantics](semantics/index.md). Please read this section as it is the heart of the whole language.

## CLIO configuration file  

A valid CLIO configuration file is comprised of five different sections:

- [Workflow name](configuration_file/workflow_name/_index.md): identifies the application workflow composed by multiple application modules.
- [IO_Graph](configuration_file/IO_Graph/_index.md): describes file data dependencies among application modules.
- [Alias](configuration_file/alias/_index.md): groups under a convenient name a set of files or directories.
- [Permanent](configuration_file/permanent/_index.md): defines which files must be kept on the permanent storage at the end of the workflow execution.
- [Exclude](configuration_file/exclude/_index.md): identifies the files and directories not handled by CAPIO.
- [Home Node Policy](configuration_file/home_node_policy/_index.md): defines different file mapping policies to establish which CAPIO servers store which files

## Examples

To further illustrate how to write a capio configuration file, we have include several complete examples in the [example](examples.md) page.

## Team

CLIO is a joint effort from UniTO and UniPI and is being developed by:

- Alberto Riccardo Martinelli <albertoriccardo.martinelli@unito.it>  
- Marco Edoardo Santimaria <marcoedoardo.santimaria@unito.it>  
- Iacopo Colonnelli <iacopo.colonnelli@unito.it>  
- Massimo Torquati <massimo.torquati@unipi.it>  
- Marco Aldinucci <marco.aldinucci@unito.it>
