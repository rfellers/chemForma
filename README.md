# chemForma
Chemical Formula Specification for Mass Spectrometry 

## Goals of this project
1. Create a unified specification for a chemical formula to be written as a single line of ASCII text
2. Human readable, machine parsable, and optimized for the simple and common cases (e.g. H2O)
3. Handle specific MS use cases
   * Glycan atom support
   * Isotope support
   * Negative cardinalities

Why not InChI? simply uses Hill convention and doesn't go any further. Could consider InChI custom layer?

ptmlist.txt
 + water looks OK (H2 O1), simple, negative support
 - required space between atoms, no glycan support, no isotopes

Unimod
 + handles glycans, isotopes handled as special atoms, negative support
 - water looks ugly (H(2) O), required space between atoms

PSI-MOD
 + isotopes handled using prefix parenthetical, negative support
 - no glycan support, water looks ugly (H 2 O 1), required space between atoms and cardinality
