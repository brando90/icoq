iCoq
====

iCoq is a regression proof selection tool for Coq.

Requirements
------------

- [`Coq 8.5.3`](https://github.com/proofengineering/coq) (modified version with `coqdigest` tool and proof-checking dependency extension)
- [`coq-ast`](https://github.com/proofengineering/coq-ast)
- [`coq-depends`](https://github.com/proofengineering/coq-depends)
- [`Java SE 8`](http://www.oracle.com/technetwork/java/javase/overview/index.html) (or later)

Note that for iCoq to be able to avoid checking unaffected proofs inside sections, your `.v` files must have the proper [annotations](https://coq.inria.fr/refman/async-proofs.html) that specify which section assumptions are used in a proof, e.g., `Proof using A_dec`.

Installation
------------

We recommend installing iCoq via [OPAM](http://opam.ocaml.org/doc/Install.html), which will automatically build and install its dependencies (except Java):
```
opam repo add proofengineering-dev http://opam-dev.proofengineering.org
opam install icoq
```

Note that we have only tested iCoq with OCaml version 4.02.3 or higher, so this requirement is enforced by the OPAM package.

Usage
-----

iCoq must be pointed at the directory where the `_CoqProject` file for your project resides.

We recommend adding an iCoq task in your project's `Makefile`, as follows:
```
icoq: _CoqProject
        icoq $(shell pwd) -timer
```
Then, you can simply run `make icoq`.

To instead run iCoq from the command line, go the directory with the `_CoqProject` file and run:
```
icoq $PWD -timer
```
