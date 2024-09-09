
## how to verify these models?

These models were checked using ProVerif version 2.05
See the official ProVerif website here: https://bblanche.gitlabpages.inria.fr/proverif/

Here is how you can install ProVerif on Linux, specifially Ubuntu 24.04:

```bash
sudo apt install opam
opam init
eval $(opam env)
opam switch list-available
opam switch create 4.13.1
eval $(opam env --switch=4.13.1)
opam update
opam install lablgtk
eval $(opam env)
wget https://bblanche.gitlabpages.inria.fr/proverif/proverif2.05.tar.gz
tar xzf proverif2.05.tar.gz
cd proverif2.05
./build
```

That results in a `proverif` binary in the current working directory:
```bash
./proverif -help
Proverif 2.05. Cryptographic protocol verifier, by Bruno Blanchet, Vincent Cheval, and Marc Sylvestre
  -test 		display a bit more information for debugging
  -in <format> 		choose the input format (horn, horntype, spass, pi, pitype)
  -out <format> 	choose the output format (solve, spass)
  -o <filename> 	choose the output file name (for spass output)
  -lib <filename> 	choose the library file (for pitype front-end only)
  -set <param> <value> 	equivalent to adding set <param> = <value> to the input file
  -TulaFale <version> 	indicate the version of TulaFale when ProVerif is used inside TulaFale
  -graph 			create the trace graph from the dot file in the directory specified
  -commandLineGraph 			Define the command for the creation of the graph trace from the dot file
  -gc 			display gc statistics
  -parse-only 		just parse the input file and report errors (if any)
  -html 			HTML display
  -help  Display this list of options
  --help  Display this list of options
```

Now, to verify the KEM Sphinx ProVerif model:
```bash

proverif kem_sphinx.passive.pv
```

Which should generate lots of output and at the end print a summary of the results
like this:

```
RESULT not event(ClientCreatedPacket(packet_3)) cannot be proved.

--------------------------------------------------------------
Verification summary:

Query not event(Mix1SharedSecretCorrect) cannot be proved.

Query not event(Mix1SharedSecretIncorrect) is true.

Query not event(Mix2SharedSecretCorrect) cannot be proved.

Query not event(Mix2SharedSecretIncorrect) is true.

Query not event(IntegrityCheckFailedMix1) is true.

Query not event(IntegrityCheckFailedMix2) is true.

Query not event(ReceivedFinalPayload) cannot be proved.

Query not event(Mix1ReceivedSphinxPacket(packet_3)) cannot be proved.

Query not event(Mix2ReceivedSphinxPacket(packet_3)) cannot be proved.

Query not event(Mix1GeneratedKeyPair(privkey)) cannot be proved.

Query not event(Mix2GeneratedKeyPair(privkey)) cannot be proved.

Query not event(ClientReceivedAllKeys(b)) cannot be proved.

Query not event(ClientCreatedPacket(packet_3)) cannot be proved.

--------------------------------------------------------------
```

