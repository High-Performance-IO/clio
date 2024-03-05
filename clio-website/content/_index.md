+++
archetype = "home"
title = "CLIO: Coordination Language for IO"
+++

CLIO is a new I/O coordination language that allows users to annotate file-based workflow data dependencies with synchronization semantics information related to files and directories.

CLIO, aims to enable the transparent overlap of computation and I/O operations among distinct producer-consumer application modules.

The language used for expressing the I/O coordination language syntax and semantics is JSON (JavaScript Object Notation). JSON is not tied to any particular programming language or platform and is widely supported across various programming languages (Java, C++, Python, etc.).

Although it is not the most commonly adopted language in the context of high-level coordination languages for expressing parallel computations, it provides automatic syntax validation features through the JSON schema.  Using JSON can make the learning curve less steep for the users.

The semantics for CLIO is described in [Semantics](semantics/index.md). Please read this section as it is the heart of the whole language. In this pages we explain in depth the idea behind CLIO and how you can describe streaming capabilities.

The specification of the CLIO configuration file can be found at [Configuration file](configuration_file/_index.md). JSON schemas are also available to validate your configuration file against the official version.

To further illustrate how to write a capio configuration file, and help newcomers to CLIO, we have include complete examples for the varius streaming combination in the [examples](examples.md) page.

## Team

CLIO is a joint effort from UniTO and UniPI and is being developed by:

- [Alberto Riccardo Martinelli](https://alpha.di.unito.it/alberto-martinelli/)
- [Marco Edoardo Santimaria](https://alpha.di.unito.it/marco-santimaria/)
- [Iacopo Colonnelli](https://alpha.di.unito.it/iacopo-colonnelli)
- Massimo Torquati <massimo.torquati@unipi.it>
- [Marco Aldinucci](https://alpha.di.unito.it/marco-aldinucci)
