A. INSTALL
You have to setup OPAM first.

1. Install Ocaml (>= 4.05.0) and Opam
(https://opam.ocaml.org/doc/Install.html).  After the installation is
done, setup opam by executing the following commands:

   $ opam switch 4.05.0 # opam switch create 4.05.0
   $ eval `opam config env`

2. Add the iCoq repo:

   $ opam repo add proofengineering-dev http://opam-dev.proofengineering.org

3. Install Coq 8.5.3:

   $ opam pin add coq 8.5.3~depends

4. Install coq-ast and coq-depends:

   $ opam install coq-ast coq-depends

5. Install mathcomp:

   $ opam install coq-mathcomp-ssreflect

Additionally, Java 8 has to be installed and Java executable needs to
be on the PATH.

B. EXAMPLE

You can use following command to run Verdi with iCoq.  (Note that we
use a version of Verdi that is annotated with "Proof using."
annotations; this version is available on GitHub.)

  $ ./tool.sh --icoq

The results for each revision will be in the "results" folder.
