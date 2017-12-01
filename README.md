iCoq
====

iCoq is a regression proof selection tool for Coq, suitable for use in workflows involving continous integration services, e.g., [Travis CI](https://travis-ci.org).

Requirements
------------

- [`Coq 8.5.3`](https://github.com/proofengineering/coq) (modified version with `coqdigest` tool and proof-checking dependency extension)
- [`coq-ast`](https://github.com/proofengineering/coq-ast)
- [`coq-depends`](https://github.com/proofengineering/coq-depends)
- [`Java SE 8`](http://www.oracle.com/technetwork/java/javase/overview/index.html) (or later)

Note that for iCoq to be able to avoid checking unaffected proofs inside sections, your `.v` files must have the proper [annotations](https://coq.inria.fr/refman/async-proofs.html) that specify which section assumptions are used in each proof, e.g., `Proof using A_dec`.

Installation
------------

We recommend installing iCoq via [OPAM](http://opam.ocaml.org/doc/Install.html), which will automatically build and install its dependencies (except Java):
```
opam repo add proofengineering-dev http://opam-dev.proofengineering.org
opam install icoq
```

Note that we have only tested compiling iCoq components with OCaml version 4.02.3 or higher, so this requirement is enforced by the OPAM package.

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

To show debug information about the Coq proof-checking commands run by iCoq, use the `-debug` option, e.g.,
```
icoq $PWD -timer -debug
```

All metadata used by iCoq is stored in the `.icoq` subdirectory of the the directory where the `_CoqProject` file resides. Removing this subdirectory will make iCoq check all proofs in the project from scratch.

To find out more about the technique used in iCoq, see the [paper](https://doi.org/10.1109/ASE.2017.8115630) in the proceedings of the 2017 Automated Software Engineering conference.

Example
-------

Suppose that we have integrated iCoq into the Makefile of a Coq project as above. The first execution of iCoq could then have output as follows:
```
$ make icoq
icoq /path/to/project -timer
[TIMER] Config               :: 0 ms.
[TIMER] Creating file graph  :: 171 ms.
[TIMER] Writing file graph   :: 145 ms.
[TIMER] Compiling VO files   :: 899 ms.
[TIMER] coqdigest            :: 71 ms.
[TIMER] ModuleDigest         :: 982 ms.
[TIMER] ModuleDepends        :: 973 ms.
[TIMER] Creating object graph:: 2 ms.
Selected num of proofs       :: 276 / 276
[TIMER] Write object graph   :: 326 ms.
```
Suppose we now change a certain function definition on which many proofs depend (directly or indirectly), and then rerun iCoq. We could then get output as follows:
```
$ make icoq
icoq /path/to/project -timer
[TIMER] config               :: 0 ms.
[TIMER] Create new file graph:: 193 ms.
[TIMER] Read old file graph  :: 137 ms.
[TIMER] Closure of files     :: 0 ms.
[TIMER] Write file graph     :: 44 ms.
[TIMER] TopSort file graph   :: 2 ms.
[TIMER] Quick compiling      :: 545 ms.
[TIMER] Read modules         :: 9 ms.
[TIMER] coqdigest            :: 10 ms.
[TIMER] ModuleDigest         :: 146 ms.
[TIMER] Read object graph    :: 45 ms.
[TIMER] Diff of objects      :: 1 ms.
[TIMER] Update object graph  :: 0 ms.
[TIMER] Write modules        :: 5 ms.
[TIMER] Closure of objects   :: 5 ms.
Selected num of proofs       :: 21 / 276
[TIMER] New depends          :: 663 ms.
[TIMER] Update depends       :: 0 ms.
[TIMER] Write object graph   :: 280 ms
```
