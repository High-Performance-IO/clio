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

## CLIO configuration file specifications

The specification of the CLIO configuration file can be found at [Configuration file](configuration_file/_index.md). JSON schemas are also available.

## Examples

To further illustrate how to write a capio configuration file, we have include several complete examples in the [example](examples.md) page.

## Json schema
The json schema for CLIO can be found at [schema/](schema/clio.json), and can be used to help write configurations files.

## Team

CLIO is a joint effort from UniTO and UniPI and is being developed by:

- Alberto Riccardo Martinelli <albertoriccardo.martinelli@unito.it>  
- Marco Edoardo Santimaria <marcoedoardo.santimaria@unito.it>  
- Iacopo Colonnelli <iacopo.colonnelli@unito.it>  
- Massimo Torquati <massimo.torquati@unipi.it>  
- Marco Aldinucci <marco.aldinucci@unito.it>
